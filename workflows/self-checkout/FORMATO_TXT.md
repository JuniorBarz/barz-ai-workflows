# Formato do TXT de importação

## Escopo

Este documento descreve o layout de referência usado pelo workflow de Self-Checkout. A função de negócio específica dos dois campos de peso no sistema de destino não é conhecida; ambos recebem o mesmo peso individual em gramas neste formato.

Os códigos e produtos desta página são exclusivamente fictícios.

## Layout de 26 caracteres

Cada registro ocupa uma linha com exatamente 26 caracteres:

```text
CCCCCCCCCCCCCNPPPPPPPPPPPP
```

| Posições | Tamanho | Conteúdo |
| --- | ---: | --- |
| 1–13 | 13 | Código do produto com 13 dígitos |
| 14 | 1 | Caractere literal `N`, maiúsculo |
| 15–20 | 6 | Primeiro campo de peso, em gramas, preenchido com zeros à esquerda |
| 21–26 | 6 | Segundo campo de peso, igual ao primeiro neste layout de referência |

Representação formal:

```text
^[0-9]{13}N[0-9]{6}[0-9]{6}$
```

## Regras

- O código deve possuir exatamente 13 dígitos e ser tratado como texto.
- Preserve zeros à esquerda que já façam parte do código.
- Não invente zeros para corrigir um identificador de comprimento desconhecido.
- O peso deve ser positivo, inteiro e expresso em gramas.
- Cada campo de peso deve possuir seis dígitos.
- Os dois campos recebem o mesmo valor neste formato.
- Não use pontos, vírgulas, espaços ou símbolos de unidade.
- Não inclua cabeçalho, rodapé ou separadores.
- Cada código deve aparecer uma única vez.
- Não inclua produtos pendentes nem códigos presentes em TXTs anteriores.

O maior valor representável em cada campo é `999999` gramas. Valores fora do intervalo não cabem neste layout e devem permanecer pendentes.

## Exemplos fictícios validados

Produto fictício de 75 gramas:

`0000000000123N000075000075`

- caracteres 1–13: `0000000000123`;
- caractere 14: `N`;
- caracteres 15–20: primeiro peso com seis dígitos;
- caracteres 21–26: segundo peso com seis dígitos;
- comprimento total: 26 caracteres.

Produto fictício de 1.065 gramas:

`0000000000456N001065001065`

- caracteres 1–13: `0000000000456`;
- caractere 14: `N`;
- os dois campos representam `1065` gramas;
- comprimento total: 26 caracteres.

Os códigos acima não identificam produtos reais e servem somente para validar a estrutura.

## Codificação e quebra de linha

Quando houver um TXT anterior comprovadamente compatível, preserve a codificação e a quebra de linha dele.

Na ausência de referência:

- codificação inicial: UTF-8 sem BOM;
- quebra de linha inicial: CRLF;
- validação obrigatória: teste controlado no importador de destino antes de uso operacional.

O arquivo deve terminar apenas com a quebra de linha esperada pelo importador, sem linhas vazias adicionais.

## Checklist de validação

Para cada linha, confirme:

1. comprimento igual a 26;
2. posições 1–13 contendo somente dígitos;
3. posição 14 contendo `N` maiúsculo;
4. posições 15–20 contendo seis dígitos;
5. posições 21–26 contendo seis dígitos;
6. igualdade entre os dois campos de peso;
7. peso positivo e coerente com o candidato aprovado;
8. código ausente dos TXTs anteriores;
9. ausência de duplicidade no arquivo novo.

Se qualquer checagem falhar, retire o registro do TXT até que a causa seja esclarecida.
