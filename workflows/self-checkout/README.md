# Análise e importação de pesos de Self-Checkout

## Objetivo

Reproduzir, em um assistente de IA, o processo documental completo para:

1. identificar produtos sem peso cadastrado;
2. analisar as medições disponíveis;
3. pré-aprovar pesos coerentes e separar casos suspeitos;
4. excluir códigos encontrados em TXTs anteriores;
5. gerar uma proposta de TXT de importação;
6. gerar, quando solicitado, um Excel simplificado de conferência.

O workflow não altera cadastros nem importa arquivos no sistema de destino. O TXT é uma proposta que precisa passar por validação humana e por um teste controlado no importador.

O processo é manual, sem API paga e sem execução de modelos pelo repositório. Pode ser usado em ChatGPT, Claude, Gemini ou outro assistente que aceite os arquivos necessários.

## Arquivos deste workflow

- [`PROMPT.md`](PROMPT.md): instruções completas para o assistente.
- [`FORMATO_TXT.md`](FORMATO_TXT.md): especificação do layout de 26 caracteres.
- [`examples/`](examples/): um cenário estatístico e um teste operacional completo, ambos sintéticos.

## O que anexar

Anexe tudo no mesmo chat:

1. este `README.md`;
2. o arquivo `PROMPT.md`;
3. o arquivo `FORMATO_TXT.md`;
4. o **Relatório Excel (XLSX)** com produtos, descrições, pesos cadastrados e medições;
5. todos os **TXTs anteriores** usados para impedir a repetição de códigos;
6. relatórios complementares de outros períodos, quando existirem.

O TXT anterior é essencial para garantir a exclusão dos códigos já processados. Sem ele, o assistente deve informar que não consegue garantir a ausência de repetições históricas.

XLSX é o formato habitual de entrada. Se o assistente não aceitar planilhas ou se você quiser facilitar a inspeção, exporte uma cópia em CSV preservando códigos como texto. Anexe XLSX ou CSV, não os dois ao mesmo tempo.

Se o assistente não aceitar anexos Markdown, copie e cole o conteúdo deste README, depois o `PROMPT.md` e o `FORMATO_TXT.md`, mantendo essa ordem.

## Privacidade antes do envio

Trabalhe em cópias dos arquivos e remova tudo que não seja necessário para a análise, especialmente:

- nomes de pessoas;
- preços e informações comerciais sem função analítica;
- documentos pessoais;
- credenciais, endereços, IPs e caminhos internos;
- observações ou colunas confidenciais.

Não envie arquivos operacionais para issues ou commits deste repositório. Os anexos devem permanecer somente na conversa escolhida e sob as políticas de privacidade do respectivo assistente.

## Entradas esperadas

Os nomes das colunas podem variar. Antes de calcular, o assistente deve identificar e confirmar conceitos equivalentes a:

| Conceito | Uso |
| --- | --- |
| Código do produto | Agrupar medições e comparar com os TXTs anteriores |
| Descrição | Avaliar plausibilidade de massa, volume, embalagem e quantidade |
| Peso cadastrado | Localizar produtos cujo cadastro está numericamente em zero |
| Peso aferido no registro | Fonte de medição, mantida separada das demais |
| Peso aferido na conferência | Segunda fonte de medição, quando disponível |
| Quantidade e cupom | Distinguir unidades, compras múltiplas e cupons distintos |
| Data ou período | Demonstrar recorrência ao longo do tempo |

Preserve os códigos como texto. Não aceite notação científica nem perda de zeros à esquerda.

## Como utilizar

### 1. Abra uma nova conversa

Anexe os arquivos listados acima e envie:

```text
Leia primeiro o README.md, o PROMPT.md e o FORMATO_TXT.md. Depois inventarie os relatórios e TXTs anexados, mostre o mapeamento das colunas e faça somente as perguntas essenciais antes de iniciar a análise.
```

### 2. Confira o mapeamento

Valide quais colunas representam código, descrição, peso cadastrado, pesos aferidos, quantidade, cupom e período. Confirme também a unidade de cada campo.

Não permita que o assistente deduza uma unidade apenas pelo tamanho do número.

Quando existirem eventos com status `n` e `c`, não elimine pares automaticamente. Considere repetição potencial somente quando as demais chaves disponíveis, como evento, cupom, horário, código e peso, também coincidirem.

### 3. Confirme o objetivo

Peça explicitamente o processo completo:

```text
Identifique os produtos com peso cadastrado igual a zero, analise as medições, exclua todos os códigos encontrados nos TXTs anteriores e classifique os demais entre PRÉ-APROVADO e DEIXAR PARA DEPOIS. Depois gere o TXT de importação e o Excel simplificado de conferência, executando todas as validações do prompt.
```

Se você quiser somente uma análise geral, diga para não gerar arquivos de importação.

### 4. Revise a triagem automática

O resultado deve separar:

- **PRÉ-APROVADO:** peso plausível e sem conflito relevante;
- **DEIXAR PARA DEPOIS:** dados insuficientes, conflitantes, variáveis ou ambíguos;
- **JÁ PROCESSADO:** código presente em pelo menos um TXT anterior.

Uma única medição pode ser pré-aprovada quando for claramente plausível e não possuir conflito relevante. Isso continua sendo uma pré-validação analítica, não uma confirmação física.

### 5. Obtenha o TXT de importação

Quando houver candidatos pré-aprovados, peça:

```text
Gere o TXT somente com os itens PRÉ-APROVADOS. Valide cada linha contra o FORMATO_TXT.md, remova duplicidades e confirme que nenhum código aparece nos TXTs anteriores. Entregue o arquivo para download ou, se o ambiente não permitir arquivos, apresente o conteúdo em um bloco de texto sem cabeçalho.
```

Cada linha deve ter exatamente 26 caracteres. O arquivo não pode conter cabeçalho, separadores, produtos pendentes ou códigos já processados. Consulte [`FORMATO_TXT.md`](FORMATO_TXT.md) antes de aceitar o resultado.

### 6. Obtenha o Excel simplificado

Peça também:

```text
Gere um Excel simplificado com duas abas: Para importar e Deixar para depois. A primeira deve corresponder exatamente ao TXT; a segunda deve mostrar cada pendência e seu motivo. Entregue o arquivo para download.
```

O Excel simplificado deve conter:

- **Aba Para importar:** código de barras, descrição, peso em gramas e cupons distintos;
- **Aba Deixar para depois:** código de barras, descrição e motivo.

Se o assistente não puder criar arquivos, ele deve fornecer as duas tabelas na resposta e declarar essa limitação. Não aceite links de download inventados.

## Teste operacional reproduzível

Use os arquivos da pasta [`examples/operacional/`](examples/operacional/) para testar o processo em uma conversa nova no ChatGPT, Claude, Gemini ou outro assistente compatível.

Anexe:

1. este `README.md`;
2. [`PROMPT.md`](PROMPT.md);
3. [`FORMATO_TXT.md`](FORMATO_TXT.md);
4. [`relatorio-ocorrencias-sintetico.xlsx`](examples/operacional/relatorio-ocorrencias-sintetico.xlsx), ou o CSV equivalente;
5. [`importacoes-anteriores-sinteticas.txt`](examples/operacional/importacoes-anteriores-sinteticas.txt).

Não anexe os arquivos com `esperada` ou `esperado` no nome durante a execução. Eles servem para conferir a resposta depois.

### Mensagem pronta para copiar e colar

```text
Leia o README.md, o PROMPT.md e o FORMATO_TXT.md anexados. Analise o relatório sintético e compare os códigos com o TXT de importações anteriores. Identifique produtos com peso cadastrado igual a zero, trate registros de status n e c como duplicados somente quando evento, cupom, horário, código e peso também coincidirem, pré-aprove os pesos claramente plausíveis, separe os casos suspeitos e exclua os códigos já processados. Gere o TXT final no layout de 26 caracteres e um Excel simplificado com as abas Para importar e Deixar para depois. Antes de entregar, valide contagens, códigos, pesos, duplicidades e a correspondência entre TXT e Excel.
```

Compare o resultado com [`resultado-esperado.md`](examples/operacional/resultado-esperado.md), [`importacao-esperada.txt`](examples/operacional/importacao-esperada.txt) e [`conferencia-esperada.xlsx`](examples/operacional/conferencia-esperada.xlsx).

## Checagens obrigatórias

Antes de usar o TXT ou o Excel:

- confirme que todos os candidatos tinham peso cadastrado numericamente igual a zero;
- confira uma amostra dos pesos diretamente nos relatórios;
- confirme que peso ausente, zero e inválido permaneceram estados diferentes;
- verifique a preservação dos códigos e dos zeros à esquerda;
- confirme que quantidade múltipla não virou peso individual sem justificativa;
- confira que volume não foi convertido em massa sem densidade;
- valide que itens de qualquer TXT anterior foram excluídos;
- confirme ausência de duplicidades;
- valide todas as linhas do TXT pelo layout de 26 caracteres;
- compare código e peso do Excel com o TXT;
- reconcilie o total de produtos entre pré-aprovados, pendentes e já processados;
- teste o arquivo primeiro em um ambiente controlado do sistema de destino.

## Como interpretar os resultados

Mediana, recorrência e baixa amplitude ajudam a avaliar consistência, mas não provam que um peso esteja correto. A descrição é uma referência de plausibilidade: peso líquido não é necessariamente peso bruto, e volume não equivale a massa sem densidade conhecida.

Os dois campos de peso do layout recebem o mesmo peso individual em gramas neste formato de referência. A função específica de cada campo no importador não está documentada neste repositório; não atribua significados diferentes sem documentação do sistema de destino.

## Limitações

- A qualidade depende do mapeamento correto das colunas e unidades.
- O relatório pode registrar eventos, e não unidades físicas independentes.
- Leituras repetidas no mesmo cupom podem não representar novas evidências.
- Quantidades múltiplas exigem uma regra confiável de normalização.
- Produtos variáveis ou descrições incompletas podem permanecer pendentes.
- Sem os TXTs anteriores, não existe garantia de exclusão histórica.
- Um identificador incompatível com 13 dígitos deve ficar pendente; zeros não podem ser inventados.
- UTF-8 sem BOM e CRLF são apenas o padrão inicial quando não existe arquivo anterior compatível.
- O TXT e o Excel não substituem a validação humana nem autorizam alteração automática do cadastro.

## Exemplos sintéticos

A pasta [`examples/`](examples/) contém um exemplo estatístico, que não produz TXT, e um exemplo operacional completo com XLSX, CSV, TXT anterior, TXT esperado e Excel simplificado esperado.
