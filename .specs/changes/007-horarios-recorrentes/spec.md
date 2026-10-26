# 007-horarios-recorrentes

## Objetivo

Permitir que o professor cadastre o horário padrão de cada aluno e gere, para revisão, as aulas futuras de uma janela limitada, sem duplicar ocorrências.

## Contexto Técnico

- Módulo de negócio `schedule`, agregado `weekly-slot`. Regras em [Módulo schedule](../../memory/modules/schedule.md).
- Horário recorrente é regra; ocorrência é aula concreta e pertence a [008-agenda-e-aulas](../008-agenda-e-aulas/spec.md).
- Geração em janela limitada (padrão assumido em [Produto](../../memory/produto.md)).
- Fuso horário do professor é usado para converter horários locais em `starts_at`/`ends_at`.

## Referências de Projeto

- [Produto](../../memory/produto.md)
- [Contexto técnico global](../../memory/contexto-tecnico.md)
- [Dominio](../../memory/dominio.md)
- [Módulos](../../memory/modulos.md)
- [Módulo schedule](../../memory/modules/schedule.md)
- [Módulo students](../../memory/modules/students.md)

## Referências Compartilhadas

- [Como executar](../../shared/como-executar.md)
- [Regras de nomenclatura](../../shared/regras-de-nomenclatura.md)

## Observações Locais

- Campos: vínculo, dia da semana, início, fim, data de início, data de encerramento (opcional), status (`active`/`paused`).
- Pausar a recorrência não apaga ocorrências já geradas.
- Geração é idempotente: chave única por vínculo, horário e data de início.
- Conflito de horário com outra aula do mesmo professor bloqueia a geração daquele item e é informado ao professor.
- A geração apenas sugere; a aula só é confirmada pelo professor na revisão.

## Tasks

### Tasks - Negócio (módulo `schedule`)

- [ ] Criar o módulo com a skill [config-new-module](../../../.agents/skills/config-new-module).
- [ ] Criar o agregado `weekly-slot` com a skill [module-aggregate](../../../.agents/skills/module-aggregate).
- [ ] Implementar a entidade `WeeklySlot` com a skill [module-entity](../../../.agents/skills/module-entity), incluindo validação de horário e de intervalo de datas.
- [ ] Definir o contrato de repositório com a skill [module-repository](../../../.agents/skills/module-repository).
- [ ] Implementar `save-weekly-slot` e `pause-weekly-slot` com a skill [module-use-case](../../../.agents/skills/module-use-case).
- [ ] Implementar `preview-lesson-occurrences`, que calcula as ocorrências de uma janela sem gravar, com a skill [module-use-case](../../../.agents/skills/module-use-case).
- [ ] Cobrir os casos de uso com testes unitários, incluindo mudança de fuso, fim de semana e datas de encerramento.

### Tasks - Back-end

- [ ] Sincronizar o modelo `schedule` no Prisma com a skill [backend-prisma-sync-module](../../../.agents/skills/backend-prisma-sync-module), com índice único para evitar duplicatas.
- [ ] Implementar o repositório Prisma com a skill [backend-prisma-repository](../../../.agents/skills/backend-prisma-repository).
- [ ] Criar `schedule.controller.ts` com a skill [backend-nest-controller](../../../.agents/skills/backend-nest-controller), expondo recorrências e a prévia de ocorrências.
- [ ] Criar `schedule.integration.http` cobrindo criação, pausa, prévia e acesso cruzado.

### Tasks - Front-end

- [ ] Criar o formulário de horário recorrente dentro da ficha do aluno.
- [ ] Criar a tela de prévia de ocorrências com seleção e confirmação antes de gravar.
- [ ] Adicionar as chaves de i18n necessárias.
- [ ] Rodar `npx tsc --noEmit` em `apps/frontend` e sinalizar ao usuário que a UI está pronta para conferência manual.

## Resultado Esperado

- Professor define o horário padrão de um aluno.
- Prévia mostra as aulas que serão criadas na janela, sinalizando conflitos.
- Confirmação cria as aulas sem duplicatas, mesmo se a geração for repetida.
- Pausar a recorrência não altera aulas já criadas.

## Encerramento

Esta spec termina apenas quando todos os itens estiverem marcados e com evidência registrada, no formato definido em [Como executar](../../shared/como-executar.md).
