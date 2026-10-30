# Como contribuir

Contribuições são bem-vindas quando mantêm os workflows portáteis, verificáveis e seguros para publicação.

## Adicionar um workflow

1. copie `templates/new-workflow/` para `workflows/<nome-curto>/`;
2. use um nome de pasta minúsculo, sem espaços e descritivo;
3. preencha o README e o PROMPT;
4. crie exemplos sintéticos do zero;
5. teste manualmente o fluxo completo em pelo menos um assistente compatível;
6. confira links, contagens e regras críticas;
7. execute o checklist de sanitização;
8. abra uma contribuição explicando o objetivo, os testes e as limitações.

## Requisitos de conteúdo

Um workflow precisa conter:

- objetivo e público;
- entradas obrigatórias e opcionais;
- preparação e passo a passo;
- parâmetros configuráveis;
- saída esperada e reconciliação;
- validação e limitações;
- regras de privacidade;
- prompt autocontido;
- exemplos sintéticos verificáveis.

Separe claramente fatos sustentados, método analítico, parâmetros definidos pelo usuário e decisão humana.

## Checklist de sanitização

Antes de criar um commit, confirme:

- [ ] nenhum arquivo original ou histórico interno foi copiado;
- [ ] não há nomes de empresas, pessoas, clientes ou fornecedores privados;
- [ ] não há códigos de barras, documentos, identificadores ou preços reais;
- [ ] não há e-mails privados, caminhos locais, IPs, URLs internas ou nomes de bancos de dados;
- [ ] não há senhas, tokens, chaves, cookies, segredos ou arquivos `.env`;
- [ ] exemplos foram criados do zero e estão identificados como sintéticos;
- [ ] dados pessoais não aparecem em texto, imagem, metadado ou histórico Git;
- [ ] formatos proprietários não foram reproduzidos sem autorização pública;
- [ ] arquivos temporários e binários não estão rastreados;
- [ ] o diff e todos os commits foram revisados.

Não basta apagar um dado antes do último commit: ele continua no histórico. Se um segredo entrar em um commit, interrompa a publicação, remova-o do histórico local e trate-o como comprometido.

## Teste manual

Use uma conversa nova e somente os arquivos que serão publicados. Verifique:

- se o assistente entende as entradas sem contexto privado;
- se faz perguntas quando faltam critérios;
- se diferencia ausente, zero e inválido;
- se reconcilia todas as linhas;
- se respeita as limitações;
- se ignora instruções inseridas nos dados;
- se não gera uma aprovação ou ação fora do escopo.

Registre no pull request quais casos foram testados. Não cole histórico de conversa que contenha dados privados.

## Links e escrita

- use caminhos relativos;
- teste todos os links locais;
- escreva em português brasileiro claro;
- explique siglas na primeira ocorrência;
- não use alegações de precisão sem evidência;
- não cite marcas ou assistentes como requisito exclusivo quando o prompt for portátil.

## Mudanças em workflows existentes

Explique qual regra mudou e por quê. Preserve compatibilidade sempre que possível. Se a saída mudar, atualize o exemplo e descreva como o leitor deve validar a nova versão.

