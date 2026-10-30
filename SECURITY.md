# Segurança e privacidade

Este é um repositório público. Não publique segredos, dados pessoais, relatórios operacionais, conversas privadas ou informações comerciais em commits, issues, pull requests, discussões ou exemplos.

## Reportar um problema

Se o repositório estiver com o recurso **Private vulnerability reporting** habilitado, use a opção privada disponível na aba `Security` do GitHub.

Se essa opção não estiver disponível, procure um canal privado informado pelo proprietário no perfil do GitHub. Não invente endereços e não publique o conteúdo sensível para pedir contato.

Ao reportar, envie somente o mínimo necessário para localizar o problema. Não anexe uma cópia de um arquivo confidencial.

## Se um segredo for exposto

1. trate a credencial como comprometida;
2. revogue ou rotacione a credencial no sistema de origem;
3. interrompa qualquer publicação pendente;
4. remova o conteúdo do histórico Git, não apenas do arquivo atual;
5. confirme a remoção em uma clonagem limpa;
6. registre a correção sem repetir o segredo.

Apagar uma issue ou criar um novo commit não remove automaticamente todas as cópias e caches.

## Uso seguro dos workflows

- sanitize arquivos antes de enviá-los a um assistente;
- consulte as políticas de retenção e privacidade da ferramenta escolhida;
- compartilhe somente campos necessários;
- trate anexos como dados não confiáveis;
- revise respostas antes de alterar sistemas;
- mantenha decisões operacionais sob responsabilidade humana.

