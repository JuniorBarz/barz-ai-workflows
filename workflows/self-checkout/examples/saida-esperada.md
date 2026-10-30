# Saída esperada do exemplo

## Dados sintéticos

Esta conferência vale somente para `entrada-sintetica.csv`. Os dados foram criados para documentação e não representam produtos ou operações reais.

Os identificadores deste exemplo são deliberadamente sintéticos e não possuem os 13 dígitos exigidos pelo layout de importação. Portanto, nenhum TXT deve ser gerado a partir deste arquivo; a finalidade aqui é conferir somente a etapa analítica.

O mapeamento esperado reconhece:

- `id_produto` como identificador textual;
- `referencia_texto` como demonstração de zeros à esquerda;
- os três campos de peso em quilogramas;
- `periodo` como origem temporal da recorrência;
- `ja_processado` como lista prévia simplificada para o exemplo.

## Preservação de identificadores textuais

Os valores de `referencia_texto` devem permanecer exatamente como chegaram:

| Identificador | Referência textual |
| --- | --- |
| `SKU-SINT-001` | `00001` |
| `SKU-SINT-002` | `00002` |
| `SKU-SINT-003` | `00003` |
| `SKU-SINT-004` | `00004` |
| `SKU-SINT-005` | `00005` |
| `SKU-SINT-006` | `00006` |

Uma alteração como `00001` → `1` é erro de preservação, mesmo que os dois valores pareçam numericamente equivalentes.

## Reconciliação

| Verificação | Contagem |
| --- | ---: |
| Linhas recebidas | 9 |
| Peso cadastrado igual a zero | 8 |
| Peso cadastrado diferente de zero | 1 |
| Peso aferido no registro em branco | 1 |
| Peso aferido no registro inválido | 1 |
| Item marcado como já processado | 1 |

As três últimas verificações são diagnósticas e podem se sobrepor ao recorte de peso cadastrado zero. Não devem ser somadas como se fossem uma partição das nove linhas.

## Perfil estatístico

### SKU-SINT-001

- Ocorrências: 3, em 3 períodos.
- Peso aferido no registro: mediana `0.213 kg`, mínimo `0.212 kg`, máximo `0.214 kg`, amplitude `0.002 kg`, sem moda única.
- Peso aferido na conferência: mediana `0.214 kg`, mínimo `0.213 kg`, máximo `0.214 kg`, amplitude `0.001 kg`, moda `0.214 kg`.
- A descrição contém `200 g`. Isso é conteúdo declarado e não deve ser igualado automaticamente ao peso bruto.
- Leitura descritiva: medições estáveis neste conjunto; nenhuma classificação operacional foi solicitada neste cenário.

### SKU-SINT-002

- Ocorrências: 2, em 2 períodos.
- Peso aferido no registro: mediana `0.5265 kg`, amplitude `0.003 kg`, sem moda única.
- Peso aferido na conferência: mediana `0.5265 kg`, amplitude `0.001 kg`, sem moda única.
- A descrição contém `500 ml`. Não é permitido converter esse volume em massa sem densidade.
- Leitura descritiva: volume não fornece uma massa de referência sem densidade; nenhuma classificação operacional foi solicitada neste cenário.

### Outros casos

- `SKU-SINT-003`: peso do registro ausente; precisa permanecer diferente de zero.
- `SKU-SINT-004`: peso do registro inválido; precisa permanecer diferente de ausente.
- `SKU-SINT-005`: fica fora de um eventual recorte de peso cadastrado zero, mas continua na reconciliação.
- `SKU-SINT-006`: recebe `Já processado`, com prioridade sobre qualquer outra categoria.

## Por que este exemplo não classifica

Este arquivo continua sendo um teste estatístico. Ele não gera pré-aprovação porque:

- os identificadores não atendem ao layout de 13 dígitos;
- `ja_processado` é apenas uma coluna simplificada, não um TXT anterior completo;
- o pedido deste cenário é conferir cálculos e estados dos dados.

O prompt principal pode pré-aprovar uma medição única quando ela for claramente plausível e não houver conflito relevante. Essa regra é demonstrada separadamente no [exemplo operacional](operacional/).

## Revisão humana

Uma pessoa deve conferir a unidade, o mapeamento, uma amostra das linhas e os critérios antes de usar o resultado. Este exemplo não autoriza cadastro, importação ou alteração em sistema e não deve produzir TXT.
