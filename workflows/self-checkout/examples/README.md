# Exemplos sintéticos

Esta pasta contém dois cenários complementares. Todos os nomes, códigos, pesos, cupons e períodos são fictícios.

## Exemplo estatístico

- [`entrada-sintetica.csv`](entrada-sintetica.csv): nove ocorrências para conferir agrupamento, mediana, moda, amplitude, valores ausentes e inválidos.
- [`saida-esperada.md`](saida-esperada.md): reconciliação e estatísticas esperadas.

Esse cenário é deliberadamente descritivo. Os identificadores não possuem o formato de 13 dígitos e nenhum TXT deve ser produzido. Use-o para testar cálculos e preservação dos dados, não o processo de importação.

## Exemplo operacional

A pasta [`operacional/`](operacional/) demonstra o workflow completo:

- relatório habitual em XLSX e alternativa em CSV;
- TXT com importações anteriores;
- uma medição única plausível;
- várias medições consistentes;
- medição incompatível;
- quantidade múltipla sem regra de normalização;
- produto já processado e produto com peso cadastrado;
- repetição potencial entre status `n` e `c` sem eliminar eventos distintos;
- TXT e Excel simplificado esperados.

Consulte o [guia do exemplo operacional](operacional/README.md) para anexar os arquivos corretos e comparar o resultado.

## Limite da validação

Os arquivos esperados foram conferidos programaticamente. Isso valida estrutura, contagens, pesos, duplicidades e correspondência entre TXT e Excel. Não equivale a executar o prompt em todos os assistentes citados. Um teste manual independente exige abrir uma conversa nova e seguir o guia.
