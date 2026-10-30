# Exemplo operacional completo

## Finalidade

Este cenário demonstra o processo inteiro em uma conversa nova: identificar produtos sem peso, avaliar medições, tratar repetições potenciais, excluir códigos anteriores e produzir o TXT e o Excel de conferência.

Todos os códigos, cupons, descrições, datas e valores são fictícios. Nenhum registro representa produto, empresa ou operação real.

## Arquivos

- [`relatorio-ocorrencias-sintetico.xlsx`](relatorio-ocorrencias-sintetico.xlsx): entrada habitual em Excel.
- [`relatorio-ocorrencias-sintetico.csv`](relatorio-ocorrencias-sintetico.csv): alternativa com os mesmos dados.
- [`importacoes-anteriores-sinteticas.txt`](importacoes-anteriores-sinteticas.txt): códigos fictícios já processados.
- [`resultado-esperado.md`](resultado-esperado.md): categorias, justificativas e reconciliação.
- [`importacao-esperada.txt`](importacao-esperada.txt): duas linhas que devem ser geradas.
- [`conferencia-esperada.xlsx`](conferencia-esperada.xlsx): Excel simplificado esperado.

Anexe somente uma versão do relatório de ocorrências: prefira o XLSX e use o CSV como alternativa. Os demais arquivos com o sufixo `esperado` servem para conferir o resultado e não devem ser entregues ao assistente durante o teste.

## Cobertura dos casos

| Código fictício | Caso | Resultado esperado |
| --- | --- | --- |
| `0000001000001` | Uma medição plausível de 75 g para descrição de 70 g | `PRÉ-APROVADO` com 75 g |
| `0000001000002` | Três eventos físicos consistentes de 212 g, 213 g e 214 g | `PRÉ-APROVADO` com mediana de 213 g |
| `0000001000003` | Medição de 950 g para descrição de 90 g | `DEIXAR PARA DEPOIS` |
| `0000001000004` | Quantidade 3 e peso total de 252 g | `DEIXAR PARA DEPOIS` sem regra de normalização |
| `0000001000005` | Medição plausível, mas código existente no TXT anterior | `JÁ PROCESSADO` |
| `0000001000006` | Peso cadastrado de 315 g | Fora do recorte de peso zero |

## Status `n` e `c`

As duas linhas de `EVT-2001` têm o mesmo evento, cupom, horário, código e peso. Apenas o status muda entre `n` e `c`. Elas formam uma repetição potencial do mesmo evento físico e devem fornecer uma única medição de 212 g.

`EVT-2002` tem status `n` e `EVT-2003` tem status `c`, mas evento, cupom, horário e peso são diferentes. Ambos permanecem como observações independentes. O exemplo não autoriza eliminar todo par de registros apenas por possuir esses dois status.

## Como executar o teste manual

1. Abra uma conversa nova no ChatGPT, Claude, Gemini ou outro assistente compatível.
2. Anexe o README e o PROMPT do workflow, o `FORMATO_TXT.md`, o relatório XLSX desta pasta e o TXT anterior.
3. Envie a mensagem pronta do [README do workflow](../../README.md).
4. Solicite também o Excel simplificado.
5. Compare a resposta com `resultado-esperado.md`, `importacao-esperada.txt` e `conferencia-esperada.xlsx`.

Esse procedimento é manual. A presença dos arquivos esperados no repositório não prova que um assistente específico produziu o resultado sem que a conversa seja realmente executada.
