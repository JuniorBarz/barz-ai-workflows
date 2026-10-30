# Resultado operacional esperado

## Escopo

O relatório possui 9 linhas, 8 eventos distintos e 6 produtos distintos. Cinco produtos têm peso cadastrado igual a zero. Um produto fica fora do recorte por já possuir peso cadastrado.

Todos os dados são sintéticos. Este documento é uma referência determinística para o teste, não o registro de uma conversa independente já executada.

## Reconciliação por produto

- 2 pré-aprovados.
- 2 em `DEIXAR PARA DEPOIS`.
- 1 `JÁ PROCESSADO`.
- 1 fora do recorte por peso já cadastrado.

As quatro categorias somam os 6 produtos distintos sem sobreposição.

## Tratamento dos eventos de status

As duas linhas de `EVT-2001` diferem somente pelo status `n` ou `c`. Evento, cupom, horário, código e peso coincidem. O resultado usa uma única observação de 212 g para esse evento.

Os eventos `EVT-2002` (`n`) e `EVT-2003` (`c`) possuem chaves, cupons, horários e pesos diferentes. Eles continuam independentes. Portanto, as medições físicas do produto `0000001000002` são 212 g, 213 g e 214 g, com mediana de 213 g.

## Classificação e justificativas

| Código fictício | Peso cadastrado | Evidência | Categoria | Justificativa |
| --- | ---: | --- | --- | --- |
| `0000001000001` | 0 g | Uma medição de 75 g | `PRÉ-APROVADO` | A descrição declara 70 g. A diferença de 5 g é plausível para peso bruto e não há conflito relevante. |
| `0000001000002` | 0 g | 212 g, 213 g e 214 g | `PRÉ-APROVADO` | As três observações físicas são consistentes. A mediana é 213 g e a descrição declara 200 g. |
| `0000001000003` | 0 g | Uma medição de 950 g | `DEIXAR PARA DEPOIS` | A descrição declara 90 g. A medição é claramente incompatível e não deve ser aprovada automaticamente. |
| `0000001000004` | 0 g | 252 g com quantidade 3 | `DEIXAR PARA DEPOIS` | Não existe regra confirmada para transformar o peso total de três unidades em peso individual. |
| `0000001000005` | 0 g | Uma medição de 410 g | `JÁ PROCESSADO` | O código aparece em `importacoes-anteriores-sinteticas.txt` e tem prioridade sobre qualquer nova sugestão. |
| `0000001000006` | 315 g | Uma medição de 316 g | Fora do recorte | O peso cadastrado é diferente de zero. |

## TXT esperado

Somente os dois produtos pré-aprovados entram no novo arquivo:

```text
0000001000001N000075000075
0000001000002N000213000213
```

Cada linha possui 26 caracteres. Os códigos são únicos, os dois campos de peso são iguais e nenhum código aparece no TXT anterior.

## Excel simplificado esperado

O arquivo `conferencia-esperada.xlsx` possui:

- aba `Para importar`: 2 linhas, exatamente os mesmos códigos e pesos do TXT;
- aba `Deixar para depois`: 2 linhas, com os códigos suspeitos e seus motivos;
- nenhum produto já processado ou fora do recorte nas duas abas.

## Checagens finais

- Produtos sem peso identificados: 5.
- Pesos coerentes selecionados automaticamente: 75 g e 213 g.
- Produtos suspeitos separados: 2.
- Código já processado excluído: `0000001000005`.
- Linhas no TXT novo: 2.
- Códigos duplicados no TXT novo: 0.
- Códigos compartilhados entre TXT novo e anterior: 0.
