# Módulo: Autenticação

## Para que serve

Garantir que cada pessoa que usa o produto tenha uma conta própria e consiga provar que é ela no momento de acessar. É a porta de entrada da aplicação e a base do isolamento de dados: tudo o que existe no produto pertence a um professor ou a um aluno.

## Conceitos principais

- **Usuário**: identidade de quem usa o produto (nome, e-mail e senha protegida). Um usuário pode ser professor, aluno ou ambos.
- **Perfil do professor** (`teacher`): nome público e fuso horário, ligado ao usuário. Define o espaço de dados do professor.

## Regras de negócio relevantes

- **Cadastro de professor** cria uma conta nova com a senha protegida (nunca armazenada em texto legível).
- **Conta de aluno** é criada pelo cadastro público e só ganha vínculos com professores pelo aceite de convite (ver [students](students.md)).
- **Login** confere as credenciais. Quando corretas, devolve apenas `{ id, name, email }`; quando erradas, recusa sem revelar qual parte falhou.
- O módulo **não decide como a sessão se mantém**. Ele responde "estas credenciais conferem ou não"; a sessão é decisão de infraestrutura.

## Fronteiras

Não cuida de recuperação de senha (decisão pendente em [Produto](../produto.md)), verificação de e-mail nem papéis administrativos.
