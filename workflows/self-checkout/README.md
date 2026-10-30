# Análise de pesos de Self-Checkout

## Objetivo

Organizar ocorrências de divergência de peso, resumir as evidências por produto e preparar uma revisão auditável. O workflow ajuda a encontrar dados consistentes, incompletos ou conflitantes. Ele não aprova cadastros e não gera importações para um sistema.

O processo foi escrito para uso manual em um assistente de IA. Não requer instalação, API ou integração.

## O que você vai precisar

Anexe ao mesmo chat:

1. este `README.md`;
2. o arquivo [`PROMPT.md`](PROMPT.md);
3. um ou mais relatórios sanitizados;
4. se existirem, listas anteriores de itens já processados;
5. os critérios da sua operação, como recorrência mínima e tolerância.

CSV é o formato preferencial. XLSX e TXT podem ser usados quando o assistente aceitar esses anexos. Não forneça um TXT proprietário sem explicar sua estrutura.

## Entradas

O workflow espera encontrar conceitos equivalentes aos campos abaixo. Os nomes podem variar, por isso o assistente deve mostrar o mapeamento antes de calcular.

| Conceito | Obrigatório | Uso |
| --- | --- | --- |
| Identificador do produto | Sim | Agrupar ocorrências e comparar listas |
| Descrição | Sim | Localizar sinais explícitos de massa, volume ou quantidade |
| Peso cadastrado | Recomendado | Distinguir registros sem peso dos que já possuem cadastro |
| Peso aferido no registro | Recomendado | Uma fonte de evidência observada |
| Peso aferido na conferência | Recomendado | Outra fonte de evidência, mantida separada |
| Quantidade registrada | Opcional | Identificar registros que podem representar mais de uma unidade |
| Data ou período | Opcional | Demonstrar recorrência em momentos diferentes |
| Lista de já processados | Opcional | Excluir duplicações antes de qualquer classificação |

O identificador deve ser tratado como texto. Isso preserva zeros à esquerda e evita notação científica.

### Unidades

Confirme se os pesos estão em quilogramas, gramas ou outra unidade. Não converta valores apenas porque parecem pequenos ou grandes. Se a unidade não estiver documentada, registre a pendência.

Uma quantidade como `200 g` na descrição normalmente representa conteúdo líquido. A balança pode medir produto e embalagem. Da mesma forma, `500 ml` é volume e não pode ser convertido em massa sem densidade fornecida.

## Preparação

Antes de anexar os arquivos:

- trabalhe em uma cópia;
- remova nomes de pessoas, preços, documentos, credenciais e campos sem função analítica;
- não publique os arquivos em issues ou commits;
- preserve o cabeçalho e os valores originais necessários;
- confirme se o relatório já está filtrado para peso cadastrado igual a zero;
- reúna listas anteriores usadas para evitar repetição;
- anote o período abrangido por cada arquivo.

Se houver vários arquivos, não os una manualmente antes de entender se possuem as mesmas colunas e unidades.

## Critérios que você precisa definir

O workflow não possui números universais. Antes da classificação, informe:

- qual população deve ser analisada;
- se o recorte exige peso cadastrado igual a zero;
- qual campo de peso é usado na decisão ou se ambos são apenas evidências;
- a recorrência mínima exigida;
- a tolerância absoluta e, se necessária, a relativa;
- como tratar produtos vendidos por peso, conjuntos e quantidade diferente de um;
- quais itens já foram processados.

Se você ainda não souber esses critérios, peça um perfil descritivo. A IA poderá mostrar estatísticas e pendências, mas não deverá declarar um item aprovado.

## Passo a passo

### 1. Inicie o chat

Anexe este guia, o prompt e os dados. Envie:

```text
Leia o README e o PROMPT antes de analisar. Comece pelo inventário dos arquivos, pelo mapeamento das colunas e pelas perguntas que impedem uma análise correta.
```

### 2. Confira o mapeamento

Valide a tabela que relaciona cada conceito do workflow a uma coluna real. Corrija unidades ou significados incorretos antes de continuar.

### 3. Confira a qualidade da entrada

A IA deve informar:

- arquivos e períodos reconhecidos;
- total de linhas por arquivo;
- identificadores ausentes ou duplicados;
- valores brancos, zero, inválidos e unidades desconhecidas;
- registros com peso cadastrado zero, não zero ou ausente;
- campos de peso disponíveis.

Essas contagens podem se sobrepor. A reconciliação final deve deixar claro quando uma categoria é diagnóstico e quando é uma partição do total.

### 4. Defina o recorte

Se o arquivo não estiver filtrado, mantenha todas as linhas para a reconciliação e crie um subconjunto de análise. Não apague registros silenciosamente.

### 5. Agrupe por identificador

Para cada produto, mantenha as descrições observadas, os períodos, a quantidade de ocorrências e os dois campos de peso separados. Não misture medições de campos diferentes em uma única estatística.

### 6. Resuma as observações

Para cada campo de peso, calcule somente com valores válidos:

- quantidade válida;
- mínimo e máximo;
- amplitude;
- mediana;
- todas as modas, ou informe que não existe moda única.

Mostre valores atípicos e possíveis conflitos. Não remova outliers sem uma regra fornecida e registrada.

### 7. Compare com a descrição

Extraia apenas tokens explícitos como `g`, `kg`, `ml`, `l` ou `un`. Use-os como sinal de revisão, não como verdade automática. Não converta volume em massa e não trate conteúdo líquido como peso bruto.

### 8. Remova repetições já conhecidas

Compare os identificadores com as listas anteriores fornecidas. `Já processado` tem prioridade sobre as demais categorias e deve permanecer visível na reconciliação.

### 9. Classifique somente com critérios definidos

As categorias sugeridas são:

- **Já processado:** consta em uma lista anterior.
- **Candidato com evidência consistente:** atende aos critérios fornecidos e não possui conflito pendente.
- **Revisar:** há incompatibilidade, ausência ou ambiguidade relevante.
- **Aguardar mais observações:** a evidência ainda não atende à recorrência mínima definida.

Sem critérios fornecidos, use `Critérios pendentes` no lugar de uma conclusão operacional.

### 10. Reconcilie

A saída deve relacionar o total recebido a linhas fora do recorte, inválidas, já processadas e resultados finais. Nenhuma linha pode desaparecer sem explicação.

## Interpretação

Recorrência alta não prova que o peso está correto; apenas mostra repetição. Baixa amplitude pode indicar estabilidade, mas pode também repetir o mesmo erro. Uma descrição compatível é uma checagem adicional, não uma autorização.

Use a justificativa por item para localizar a evidência. Se a classificação depender de um parâmetro, a saída deve registrar o nome e o valor desse parâmetro.

## Validação

Antes de aplicar qualquer conclusão:

- confirme a soma das linhas e categorias;
- confira identificadores com zeros à esquerda;
- revise registros brancos, zero e inválidos separadamente;
- compare uma amostra com o arquivo de origem;
- verifique se as unidades usadas são as declaradas;
- procure descrições ou quantidades conflitantes no mesmo identificador;
- confirme que itens já processados não foram sugeridos novamente;
- revise outliers sem removê-los da evidência;
- registre os critérios usados;
- valide qualquer formato de importação fora deste workflow.

## Limitações

- O assistente pode mapear uma coluna incorretamente; confirme antes do cálculo.
- Descrições abreviadas podem omitir unidade, embalagem ou quantidade.
- Um relatório pode registrar eventos, não unidades físicas independentes.
- Quantidade diferente de um não deve ser normalizada sem conhecer a semântica do sistema.
- Medições podem incluir embalagem, conjuntos ou variação operacional.
- O workflow não define tolerâncias, recorrência mínima ou autoridade entre campos.
- A saída apoia revisão; não altera cadastro e não substitui decisão humana.

## Exemplo

A pasta [`examples/`](examples/) contém um CSV pequeno e uma saída ilustrativa. Os dados são inteiramente sintéticos e foram construídos para demonstrar filtros, repetição, campos ausentes, valor inválido, volume e item já processado.

