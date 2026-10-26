# 008-agenda-e-aulas

## Objetivo

Permitir que o professor agende, reagende, cancele, conclua e registre falta de aulas, com agenda semanal e diária, mantendo o histórico correto de cada ocorrência.

## Contexto Técnico

- Módulo de negócio `lessons`, agregado `lesson`. Regras em [Módulo lessons](../../memory/modules/lessons.md) e estados em [Dominio](../../memory/dominio.md).
- Recebe ocorrências geradas pelo módulo `schedule` ([007-horarios-recorrentes](../007-horarios-recorrentes/spec.md)) e pode copiar um dia do plano ([006-planos-mensais](../006-planos-mensais/spec.md)).
- Padrão adotado: `modelo-crud` adaptado, com transições de estado.

## Referências de Projeto

- [Produto](../../memory/produto.md)
- [Contexto técnico global](../../memory/contexto-tecnico.md)
- [Dominio](../../memory/dominio.md)
- [Módulos](../../memory/modulos.md)
- [Módulo lessons](../../memory/modules/lessons.md)
- [Módulo plans](../../memory/modules/plans.md)
- [Módulo schedule](../../memory/modules/schedule.md)
- [Módulo videos](../../memory/modules/videos.md)

## Referências Compartilhadas

- [Como executar](../../shared/como-executar.md)
- [Regras de nomenclatura](../../shared/regras-de-nomenclatura.md)

## Observações Locais

- Estados: `scheduled`, `completed`, `cancelled`, `no_show`. Só `completed` conta como realizada.
- Reagendar cria nova aula com `rescheduled_from_id` apontando para a original; a original vira `cancelled` com motivo e permanece no histórico.
- Observações compartilháveis (aluno vê) e privadas (só professor) são campos separados.
- Ao concluir, copiar título e objetivo do dia do plano para a aula (snapshot). Vídeos ficam em `lesson_videos` com `source` (`plan` ou `manual`).
- Editar aula concluída é bloqueado, exceto observações, e cada alteração é registrada em log de auditoria mínimo.
- Conflito de horário é checado antes de salvar.

## Tasks

### Tasks - Negócio (módulo `lessons`)

- [ ] Criar o módulo com a skill [config-new-module](../../../.agents/skills/config-new-module).
- [ ] Criar o agregado `lesson` com a skill [module-aggregate](../../../.agents/skills/module-aggregate).
- [ ] Implementar a entidade `Lesson` com a skill [module-entity](../../../.agents/skills/module-entity), com máquina de estados e regras de reagendamento.
- [ ] Definir o contrato de repositório com a skill [module-repository](../../../.agents/skills/module-repository).
- [ ] Implementar `save-lesson`, `reschedule-lesson`, `cancel-lesson`, `complete-lesson` e `mark-no-show` com a skill [module-use-case](../../../.agents/skills/module-use-case).
- [ ] Implementar `add-lesson-video` e `remove-lesson-video` com a skill [module-use-case](../../../.agents/skills/module-use-case).
- [ ] Cobrir os casos de uso com testes unitários, incluindo transições inválidas e preservação da aula original no reagendamento.

### Tasks - Back-end

- [ ] Sincronizar o modelo `lessons` no Prisma com a skill [backend-prisma-sync-module](../../../.agents/skills/backend-prisma-sync-module), incluindo `lesson_videos`.
- [ ] Implementar o repositório Prisma com a skill [backend-prisma-repository](../../../.agents/skills/backend-prisma-repository), com índices por professor, aluno e data.
- [ ] Criar `lessons.controller.ts` com a skill [backend-nest-controller](../../../.agents/skills/backend-nest-controller), expondo agenda por período, detalhe, criação e as ações de estado.
- [ ] Criar `lessons.integration.http` cobrindo cada transição de estado, reagendamento e acesso cruzado.

### Tasks - Front-end

- [ ] Criar a agenda semanal e diária em `(private)`, com estados visíveis por cor e texto.
- [ ] Criar o formulário de aula com seleção de aluno, data, horário e vínculo opcional com o dia do plano.
- [ ] Criar o detalhe da aula com ações de concluir, cancelar, registrar falta e reagendar.
- [ ] Criar a seção de vídeos da aula, indicando origem plano ou manual.
- [ ] Adicionar o item "Agenda" no menu lateral.
- [ ] Adicionar as chaves de i18n necessárias.
- [ ] Rodar `npx tsc --noEmit` em `apps/frontend` e sinalizar ao usuário que a UI está pronta para conferência manual.

## Resultado Esperado

- Professor agenda, edita, reagenda, cancela, conclui e registra falta.
- Agenda semanal e diária mostram horários e estados corretos.
- Reagendamento mantém a aula original no histórico com relação explícita.
- Apenas aulas concluídas entram nas contagens.
- Aula concluída mantém o conteúdo copiado no momento da sessão.

## Encerramento

Esta spec termina apenas quando todos os itens estiverem marcados e com evidência registrada, no formato definido em [Como executar](../../shared/como-executar.md).
