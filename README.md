# Barz AI Workflows

Portfólio de workflows e prompts reutilizáveis para análise com inteligência artificial.

## Sobre

Este repositório documenta processos que podem ser reproduzidos em ChatGPT, Claude, Gemini ou outro assistente compatível. O uso é manual, sem API paga e sem execução de modelos pelo próprio repositório.

A proposta é simples: cada workflow reúne contexto, regras, um prompt pronto e exemplos sintéticos. A pessoa escolhe o workflow, anexa os documentos e seus próprios dados ao assistente e revisa o resultado antes de usá-lo.

O repositório é exclusivamente documental. Nada é executado automaticamente e nenhum sistema operacional, comercial ou de cadastro é alterado.

## Workflows

| Workflow | Finalidade | Arquivos |
| --- | --- | --- |
| Análise de pesos de Self-Checkout | Organizar ocorrências, avaliar consistência dos pesos e preparar uma revisão auditável | [Guia](workflows/self-checkout/README.md) · [Prompt](workflows/self-checkout/PROMPT.md) · [Exemplos](workflows/self-checkout/examples/) |

Novos workflows serão adicionados somente quando as regras puderem ser explicadas, sanitizadas e validadas sem expor informações privadas.

## Estrutura

```text
barz-ai-workflows/
├── README.md
├── CONTRIBUTING.md
├── SECURITY.md
├── LICENSE
├── workflows/
│   └── self-checkout/
│       ├── README.md
│       ├── PROMPT.md
│       └── examples/
└── templates/
    └── new-workflow/
```

Cada pasta em `workflows/` deve ser autocontida. O README explica o processo; o `PROMPT.md` contém as instruções para o assistente; `examples/` mostra entradas e saídas fictícias.

## Como usar

### 1. Escolha o workflow

Abra o [guia de análise de pesos](workflows/self-checkout/README.md) e confira se o objetivo, as entradas e as limitações correspondem ao seu caso.

### 2. Prepare os dados

- Faça uma cópia de trabalho dos arquivos.
- Remova nomes, preços, documentos pessoais, credenciais e outras informações que o assistente não precisa analisar.
- Preserve identificadores como texto quando zeros à esquerda forem importantes.
- Confirme as unidades dos campos numéricos.

CSV é o formato preferencial. XLSX ou TXT também podem ser usados quando o assistente escolhido aceitar esses anexos. Um TXT proprietário não deve ser interpretado sem uma especificação fornecida pelo usuário.

### 3. Abra um novo chat

Use um assistente que permita anexar arquivos. A interface muda entre os produtos, mas o processo é o mesmo:

1. anexe `workflows/self-checkout/README.md`;
2. anexe `workflows/self-checkout/PROMPT.md`;
3. anexe os arquivos de entrada sanitizados;
4. envie um comando inicial claro;
5. responda às perguntas sobre colunas, unidades e critérios;
6. revise a reconciliação e as pendências antes de aceitar a tabela final.

Exemplo de comando inicial:

```text
Leia primeiro o README.md e o PROMPT.md anexados. Depois analise os arquivos de entrada seguindo exatamente esse workflow. Antes de calcular, confirme quais colunas você reconheceu, as unidades e os critérios que ainda preciso definir. Não invente valores ausentes.
```

Para revisar o resultado:

```text
Mostre a reconciliação entre o total de linhas recebidas e todas as categorias da saída. Liste também os registros inválidos, as unidades desconhecidas e as decisões que ainda dependem de uma pessoa.
```

Para continuar uma análise dividida:

```text
Esta é a parte 2 de 4. Armazene apenas o resumo necessário por identificador e não produza a classificação final até eu enviar todas as partes.
```

### 4. Quando o assistente não aceita anexos

Abra os arquivos Markdown, copie e cole primeiro o conteúdo do README e depois o conteúdo do prompt. Em seguida, cole uma pequena amostra dos dados em um bloco delimitado.

Se o arquivo for grande, divida os registros em partes com a mesma estrutura. Numere cada parte e peça ao assistente para aguardar a última antes de concluir. Confirme no final que a soma das partes coincide com o total original.

### 5. Revise antes de aplicar

Uma resposta bem formatada ainda pode estar errada. Confira contagens, unidades, identificadores, fórmulas, casos ausentes e critérios configuráveis. Nunca importe ou altere cadastros diretamente a partir de uma resposta não revisada.

## Como os workflows são escritos

Cada solução separa quatro tipos de informação:

1. **Fatos sustentados:** estrutura ou comportamento confirmado pelas evidências disponíveis.
2. **Método analítico:** agrupamentos, estatísticas e verificações reproduzíveis.
3. **Parâmetros configuráveis:** tolerâncias, recorrência mínima e critérios que variam por operação.
4. **Decisão humana:** aprovação, importação ou ação no sistema de destino.

Essa separação evita apresentar uma hipótese como regra operacional.

## Limitações

- O repositório não executa modelos e não oferece automação.
- A qualidade da resposta depende da qualidade e da definição dos dados de entrada.
- Nomes de colunas e formatos variam entre sistemas; o assistente deve confirmar o mapeamento.
- Peso líquido informado na descrição não é necessariamente o peso bruto medido.
- Volume e massa não são equivalentes sem uma densidade conhecida.
- Mediana, moda e recorrência descrevem os dados; não aprovam um cadastro por conta própria.
- Os prompts não substituem conferência operacional, política interna ou validação no sistema de destino.
- Interfaces, limites de anexos e políticas de privacidade variam entre assistentes.

## Checagens

Antes da análise:

- confirme o período e se o relatório já foi filtrado;
- preserve uma cópia original fora deste repositório;
- remova dados desnecessários ou confidenciais;
- identifique campos obrigatórios, unidades e listas anteriores.

Durante a análise:

- confira o mapeamento das colunas;
- mantenha branco, zero, inválido e não aplicável como estados diferentes;
- preserve identificadores e zeros à esquerda;
- não remova valores atípicos silenciosamente;
- exija justificativa para cada classificação.

Antes de usar o resultado:

- reconcilie todas as linhas de entrada;
- revise duplicidades e itens já processados;
- confira uma amostra diretamente no arquivo de origem;
- registre critérios e parâmetros utilizados;
- valide o formato exigido pelo sistema de destino separadamente.

## Privacidade e segurança

Não publique arquivos operacionais, dados pessoais, credenciais, endereços internos, bancos de dados, históricos de conversa ou informações comerciais confidenciais. Os exemplos deste repositório são sintéticos e não devem ser substituídos por dados reais em commits ou issues.

Consulte [SECURITY.md](SECURITY.md) para orientação sobre reporte e [CONTRIBUTING.md](CONTRIBUTING.md) antes de enviar mudanças.

## Como contribuir

Copie a pasta [`templates/new-workflow/`](templates/new-workflow/), renomeie-a e preencha todas as seções. Inclua apenas exemplos sintéticos, teste o prompt em pelo menos um assistente compatível e execute o checklist de sanitização descrito em [CONTRIBUTING.md](CONTRIBUTING.md).

Um novo workflow deve explicar:

- o problema que resolve;
- os dados necessários;
- o que o assistente deve fazer e o que deve recusar;
- como conferir a resposta;
- quais parâmetros o usuário precisa decidir;
- quais limitações impedem uso automático.

## Licença

Conteúdo disponibilizado sob a [MIT License](LICENSE).
