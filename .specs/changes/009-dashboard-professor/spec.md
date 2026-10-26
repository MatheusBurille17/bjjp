# 009-dashboard-professor

## Objetivo

Mostrar ao professor, na tela inicial, os indicadores da sua rotina com base nos registros reais.

## Contexto Técnico

- Módulo de negócio `dashboard`, somente leitura. Regras em [Módulo dashboard](../../memory/modules/dashboard.md).
- Consultas são feitas pelo controller, sem caso de uso de comando, conforme [Contexto técnico](../../memory/contexto-tecnico.md).
- Depende de `students`, `lessons` e `schedule`.

## Referências de Projeto

- [Produto](../../memory/produto.md)
- [Contexto técnico global](../../memory/contexto-tecnico.md)
- [Dominio](../../memory/dominio.md)
- [Módulos](../../memory/modulos.md)
- [Módulo dashboard](../../memory/modules/dashboard.md)

## Referências Compartilhadas

- [Como executar](../../shared/como-executar.md)
- [Regras de nomenclatura](../../shared/regras-de-nomenclatura.md)

## Observações Locais

- Indicadores: alunos ativos, aulas do dia, próximas aulas, aulas realizadas no mês, cancelamentos e faltas no período, horários livres (se a disponibilidade estiver cadastrada).
- Ações rápidas: cadastrar aluno, criar plano, agendar aula, adicionar vídeo.
- Respostas de leitura são mapeadas explicitamente no controller, sem depender de serialização de entidade.

## Tasks

### Tasks - Negócio (módulo `dashboard`)

- [ ] Criar o módulo com a skill [config-new-module](../../../.agents/skills/config-new-module).
- [ ] Definir o modelo de leitura `teacher-dashboard` como objeto simples, com a skill [module-entity](../../../.agents/skills/module-entity) apenas se houver regra de validação; caso contrário, tipo simples.
- [ ] Implementar `load-teacher-dashboard` com a skill [module-use-case](../../../.agents/skills/module-use-case), usando portas de leitura dos módulos `students` e `lessons`.
- [ ] Cobrir o caso de uso com testes unitários, incluindo a regra de não contar aulas `cancelled` como realizadas.

### Tasks - Back-end

- [ ] Implementar as consultas agregadas no Prisma com a skill [backend-prisma-repository](../../../.agents/skills/backend-prisma-repository).
- [ ] Criar `dashboard.controller.ts` com a skill [backend-nest-controller](../../../.agents/skills/backend-nest-controller), com endpoint autenticado de resumo.
- [ ] Criar `dashboard.integration.http` validando que os números batem com dados de teste.

### Tasks - Front-end

- [ ] Criar a tela inicial do professor em `(private)/dashboard` com cards de indicadores e lista de próximas aulas.
- [ ] Criar os atalhos de ações rápidas.
- [ ] Adicionar as chaves de i18n necessárias.
- [ ] Rodar `npx tsc --noEmit` em `apps/frontend` e sinalizar ao usuário que a UI está pronta para conferência manual.

## Resultado Esperado

- Dashboard mostra os indicadores do professor autenticado, sem dados de outros professores.
- Contagens usam apenas estados registrados.
- Atalhos levam às telas de cadastro correspondentes.

## Encerramento

Esta spec termina apenas quando todos os itens estiverem marcados e com evidência registrada, no formato definido em [Como executar](../../shared/como-executar.md).
