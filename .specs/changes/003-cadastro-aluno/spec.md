# 003-cadastro-aluno

## Objetivo

Permitir que o professor cadastre, consulte, edite e arquive seus alunos, e envie convite seguro para que o aluno associe a própria conta ao vínculo.

## Contexto Técnico

- Módulo de negócio `students`, agregado `enrollment`. Regras em [Módulo students](../../memory/modules/students.md).
- Um vínculo pertence a um professor e pode ter `student_profile_id` nulo enquanto o convite estiver pendente.
- Um aluno pode ter mais de um vínculo; dados de um vínculo nunca são compartilhados com outro professor.
- Padrão adotado: `modelo-crud` adaptado, pois o vínculo tem estados e convite, além do CRUD.

## Referências de Projeto

- [Produto](../../memory/produto.md)
- [Contexto técnico global](../../memory/contexto-tecnico.md)
- [Dominio](../../memory/dominio.md)
- [Módulos](../../memory/modulos.md)
- [Módulo students](../../memory/modules/students.md)

## Referências Compartilhadas

- [Como executar](../../shared/como-executar.md)
- [Regras de nomenclatura](../../shared/regras-de-nomenclatura.md)

## Observações Locais

- Convite por e-mail com link seguro e validade limitada. Telefone é apenas dado de contato (padrão assumido em [Produto](../../memory/produto.md)).
- O professor não vincula conta de aluno diretamente; só o aluno aceita o convite.
- Arquivar muda o status para `archived` e preserva o histórico. Não há exclusão física.
- Campos: nome completo, e-mail (opcional), telefone (opcional), data de início, status, observações privadas.
- Listagem: nome, situação e próxima aula (a coluna de próxima aula fica vazia até a spec 008).
- Ficha do aluno nesta spec inclui resumo e dados do vínculo. Agenda, plano e biblioteca entram nas specs seguintes.

## Tasks

### Tasks - Negócio (módulo `students`)

- [ ] Criar o módulo e o agregado `enrollment` com a skill [config-new-module](../../../.agents/skills/config-new-module) e a skill [module-aggregate](../../../.agents/skills/module-aggregate).
- [ ] Implementar a entidade `Enrollment` com a skill [module-entity](../../../.agents/skills/module-entity), com os campos listados nas observações e regras de transição de status.
- [ ] Definir o contrato do repositório de vínculos com a skill [module-repository](../../../.agents/skills/module-repository).
- [ ] Implementar o caso de uso `save-enrollment` com a skill [module-use-case](../../../.agents/skills/module-use-case). Decide criar ou atualizar pelo `findById`.
- [ ] Implementar o caso de uso `archive-enrollment` com a skill [module-use-case](../../../.agents/skills/module-use-case).
- [ ] Implementar o caso de uso `send-enrollment-invite` com a skill [module-use-case](../../../.agents/skills/module-use-case), gerando token com validade.
- [ ] Implementar o caso de uso `accept-enrollment-invite`, que associa a conta do aluno ao vínculo e muda o status para `active`.
- [ ] Cobrir os casos de uso com testes unitários usando os fakes do módulo.

### Tasks - Back-end

- [ ] Sincronizar o modelo `students` no Prisma com a skill [backend-prisma-sync-module](../../../.agents/skills/backend-prisma-sync-module).
- [ ] Implementar o repositório Prisma de vínculos com a skill [backend-prisma-repository](../../../.agents/skills/backend-prisma-repository). Toda consulta filtra por `teacherId` do usuário autenticado.
- [ ] Implementar o provider de envio de e-mail e geração de token de convite com a skill [backend-provider-implementation](../../../.agents/skills/backend-provider-implementation).
- [ ] Criar `students.controller.ts` com a skill [backend-nest-controller](../../../.agents/skills/backend-nest-controller), expondo listagem paginada com busca por nome e filtro por status, detalhe, criação, edição, arquivamento e envio de convite. Aceite de convite é endpoint público com token.
- [ ] Responder 404 para vínculo de outro professor em leituras por id e 403 em mutações.
- [ ] Criar `students.integration.http` (Rest Client) cobrindo os fluxos e os erros de acesso cruzado.

### Tasks - Front-end

- [ ] Criar a listagem de alunos em `(private)` com busca por nome, filtro por status e ação de cadastro.
- [ ] Criar o formulário de aluno compartilhado entre criação e edição, com `form-section-layout`.
- [ ] Criar a ficha do aluno com resumo e dados do vínculo.
- [ ] Criar a ação de arquivar com `delete-confirmation-dialog` (confirmação antes da ação).
- [ ] Criar a ação de enviar convite e a tela de aceite do convite em `(public)`.
- [ ] Adicionar o item "Alunos" no menu lateral.
- [ ] Adicionar as chaves de i18n necessárias.
- [ ] Rodar `npx tsc --noEmit` em `apps/frontend` e sinalizar ao usuário que a UI está pronta para conferência manual.

## Resultado Esperado

- Professor cadastra, lista, edita e arquiva alunos, sem ver alunos de outros professores.
- Convite gera vínculo pendente; após aceite, o vínculo fica ativo e ligado à conta do aluno.
- Arquivar não apaga histórico e tira o aluno dos ativos.
- Observações privadas nunca são retornadas para a conta do aluno.

## Encerramento

Esta spec termina apenas quando todos os itens estiverem marcados e com evidência registrada, no formato definido em [Como executar](../../shared/como-executar.md).
