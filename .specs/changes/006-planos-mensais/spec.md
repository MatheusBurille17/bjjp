# 006-planos-mensais

## Objetivo

Permitir que o professor monte planos mensais com dias planejados, técnicas e vídeos, rascunhe, publique para alunos específicos e duplique planos para outros meses.

## Contexto Técnico

- Módulo de negócio `plans`, agregado `monthly-plan` com `plan-day`. Regras em [Módulo plans](../../memory/modules/plans.md).
- Modelo por dia da semana dentro de um período (padrão assumido em [Produto](../../memory/produto.md)). Se o produto decidir por data concreta, esta spec precisa ser revisada antes de começar.
- Publicação por vínculo via `plan_enrollments`.
- Um plano não gera aulas. Aulas são criadas em [008-agenda-e-aulas](../008-agenda-e-aulas/spec.md), que pode copiar um dia do plano.

## Referências de Projeto

- [Produto](../../memory/produto.md)
- [Contexto técnico global](../../memory/contexto-tecnico.md)
- [Dominio](../../memory/dominio.md)
- [Módulos](../../memory/modulos.md)
- [Módulo plans](../../memory/modules/plans.md)
- [Módulo videos](../../memory/modules/videos.md)
- [Módulo techniques](../../memory/modules/techniques.md)
- [Módulo students](../../memory/modules/students.md)

## Referências Compartilhadas

- [Como executar](../../shared/como-executar.md)
- [Regras de nomenclatura](../../shared/regras-de-nomenclatura.md)

## Observações Locais

- Campos do plano: título, descrição e objetivo geral, período (início e fim), status, data de criação e atualização.
- Campos do dia planejado: dia da semana, tema, objetivo, técnicas relacionadas, observações, ordem, vídeos sugeridos.
- Duplicar copia o plano e os dias para um novo rascunho.
- Publicar exige pelo menos um vínculo ativo atribuído.
- Alterar um plano publicado não altera aulas já criadas; a aula mantém o próprio snapshot.

## Tasks

### Tasks - Negócio (módulo `plans`)

- [ ] Criar o módulo com a skill [config-new-module](../../../.agents/skills/config-new-module).
- [ ] Criar o agregado `monthly-plan` com a skill [module-aggregate](../../../.agents/skills/module-aggregate), contendo `plan-day` como parte do agregado.
- [ ] Implementar as entidades com a skill [module-entity](../../../.agents/skills/module-entity), com máquina de estados `draft` → `published` → `archived`.
- [ ] Definir o contrato de repositório com a skill [module-repository](../../../.agents/skills/module-repository).
- [ ] Implementar `save-monthly-plan`, `publish-monthly-plan`, `archive-monthly-plan`, `duplicate-monthly-plan` com a skill [module-use-case](../../../.agents/skills/module-use-case).
- [ ] Cobrir os casos de uso com testes unitários, incluindo transições inválidas e duplicação.

### Tasks - Back-end

- [ ] Sincronizar os modelos `plans` no Prisma com a skill [backend-prisma-sync-module](../../../.agents/skills/backend-prisma-sync-module), incluindo `plan_day_videos` e `plan_enrollments`.
- [ ] Implementar os repositórios Prisma com a skill [backend-prisma-repository](../../../.agents/skills/backend-prisma-repository).
- [ ] Criar `plans.controller.ts` com a skill [backend-nest-controller](../../../.agents/skills/backend-nest-controller), expondo CRUD, publicação, arquivamento, duplicação e atribuição a vínculos.
- [ ] Validar no servidor que técnicas, vídeos e vínculos referenciados pertencem ao professor autenticado.
- [ ] Criar `plans.integration.http` cobrindo rascunho, publicação, duplicação e acesso cruzado.

### Tasks - Front-end

- [ ] Criar a lista de planos em `(private)` com status e período.
- [ ] Criar o editor de plano com seção de dados gerais e lista ordenável de dias planejados.
- [ ] Criar o seletor de técnicas e vídeos por dia, reaproveitando `orderable-object-list`.
- [ ] Criar a ação de publicar com seleção de alunos e confirmação.
- [ ] Criar a ação de duplicar.
- [ ] Adicionar o item "Planos de aula" no menu lateral.
- [ ] Adicionar as chaves de i18n necessárias.
- [ ] Rodar `npx tsc --noEmit` em `apps/frontend` e sinalizar ao usuário que a UI está pronta para conferência manual.

## Resultado Esperado

- Professor cria plano com vários dias, cada um com técnica e vídeos.
- Rascunho fica visível só ao professor.
- Publicação vale apenas para os vínculos escolhidos.
- Duplicação gera rascunho independente.
- Interface deixa claro que plano não é aula realizada.

## Encerramento

Esta spec termina apenas quando todos os itens estiverem marcados e com evidência registrada, no formato definido em [Como executar](../../shared/como-executar.md).
