# Saída esperada do exemplo

## Dados sintéticos

Esta conferência vale somente para `entrada-sintetica.csv`. Os dados foram criados para documentação e não representam produtos ou operações reais.

O mapeamento esperado reconhece:

- `id_produto` como identificador textual;
- `referencia_texto` como demonstração de zeros à esquerda;
- os três campos de peso em quilogramas;
- `periodo` como origem temporal da recorrência;
- `ja_processado` como lista prévia simplificada para o exemplo.

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

## Perfil descritivo

### SKU-SINT-001

- Ocorrências: 3, em 3 períodos.
- Peso aferido no registro: mediana `0.213 kg`, mínimo `0.212 kg`, máximo `0.214 kg`, amplitude `0.002 kg`, sem moda única.
- Peso aferido na conferência: mediana `0.214 kg`, mínimo `0.213 kg`, máximo `0.214 kg`, amplitude `0.001 kg`, moda `0.214 kg`.
- A descrição contém `200 g`. Isso é conteúdo declarado e não deve ser igualado automaticamente ao peso bruto.
- Categoria sem critérios: `Critérios pendentes`.

### SKU-SINT-002

- Ocorrências: 2, em 2 períodos.
- Peso aferido no registro: mediana `0.5265 kg`, amplitude `0.003 kg`, sem moda única.
- Peso aferido na conferência: mediana `0.5265 kg`, amplitude `0.001 kg`, sem moda única.
- A descrição contém `500 ml`. Não é permitido converter esse volume em massa sem densidade.
- Categoria sem critérios: `Critérios pendentes`.

### Outros casos

- `SKU-SINT-003`: peso do registro ausente; precisa permanecer diferente de zero.
- `SKU-SINT-004`: peso do registro inválido; precisa permanecer diferente de ausente.
- `SKU-SINT-005`: fica fora de um eventual recorte de peso cadastrado zero, mas continua na reconciliação.
- `SKU-SINT-006`: recebe `Já processado`, com prioridade sobre qualquer outra categoria.

## Critérios pendentes

Antes de uma classificação operacional, ainda faltam:

- recorrência mínima;
- tolerância absoluta e eventual tolerância relativa;
- campo de peso que participa da decisão;
- tratamento de embalagem e quantidade;
- confirmação de que `ja_processado` representa a lista anterior completa.

Sem essas respostas, o exemplo demonstra estatísticas e pendências, não aprovação.

## Revisão humana

Uma pessoa deve conferir a unidade, o mapeamento, uma amostra das linhas e os critérios antes de usar o resultado. Este exemplo não autoriza cadastro, importação ou alteração em sistema.
