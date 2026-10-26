# 004-biblioteca-tecnicas

## Objetivo

Permitir que o professor organize o acervo de Jiu-Jitsu em técnicas e módulos, para depois associar vídeos e liberar conteúdo aos alunos.

## Contexto Técnico

- Módulo de negócio `techniques`, agregados `technique` e `module`. Regras em [Módulo techniques](../../memory/modules/techniques.md).
- Hierarquia fixa: técnica → módulo → vídeo. Vídeos são criados na spec [005-videos](../005-videos/spec.md).
- Padrão adotado: `modelo-crud` para cada agregado.

## Referências de Projeto

- [Produto](../../memory/produto.md)
- [Contexto técnico global](../../memory/contexto-tecnico.md)
- [Dominio](../../memory/dominio.md)
- [Módulos](../../memory/modulos.md)
- [Módulo techniques](../../memory/modules/techniques.md)

## Referências Compartilhadas

- [Como executar](../../shared/como-executar.md)
- [Regras de nomenclatura](../../shared/regras-de-nomenclatura.md)

## Observações Locais

- Técnica: nome, descrição, categoria opcional, ordem, status (`active`/`archived`).
- Módulo: técnica pai, nome, descrição opcional, ordem.
- Arquivar técnica ou módulo não apaga vínculos históricos.
- Ordenação por campo `sortOrder` com reordenação na listagem.
- A hierarquia de exemplo do planejamento (Passagem de guarda → Fundamentos, Preparação, etc.) é ilustrativa e não vira dado fixo; o professor cria os próprios itens.

## Tasks

### Tasks - Negócio (módulo `techniques`)

- [ ] Criar o módulo com a skill [config-new-module](../../../.agents/skills/config-new-module).
- [ ] Criar o agregado `technique` com a skill [module-aggregate](../../../.agents/skills/module-aggregate) e o agregado `module` no mesmo módulo.
- [ ] Implementar as entidades `Technique` e `Module` com a skill [module-entity](../../../.agents/skills/module-entity).
- [ ] Definir os contratos de repositório com a skill [module-repository](../../../.agents/skills/module-repository).
- [ ] Implementar `save-technique`, `delete-technique` (ou arquivamento), `save-module` e `delete-module` com a skill [module-use-case](../../../.agents/skills/module-use-case).
- [ ] Cobrir os casos de uso com testes unitários usando os fakes do módulo.

### Tasks - Back-end

- [ ] Sincronizar os modelos `techniques` no Prisma com a skill [backend-prisma-sync-module](../../../.agents/skills/backend-prisma-sync-module).
- [ ] Implementar os repositórios Prisma com a skill [backend-prisma-repository](../../../.agents/skills/backend-prisma-repository).
- [ ] Criar `techniques.controller.ts` com a skill [backend-nest-controller](../../../.agents/skills/backend-nest-controller), expondo CRUD de técnicas e módulos e reordenação.
- [ ] Criar `techniques.integration.http` cobrindo os fluxos e o acesso cruzado.

### Tasks - Front-end

- [ ] Criar a página de biblioteca em `(private)` com árvore técnica → módulo, busca e filtro por status.
- [ ] Criar os formulários de técnica e de módulo com `form-section-layout`.
- [ ] Criar a ação de arquivar com `delete-confirmation-dialog`.
- [ ] Adicionar o item "Biblioteca" no menu lateral.
- [ ] Adicionar as chaves de i18n necessárias.
- [ ] Rodar `npx tsc --noEmit` em `apps/frontend` e sinalizar ao usuário que a UI está pronta para conferência manual.

## Resultado Esperado

- Professor cria, edita, reordena e arquiva técnicas e módulos.
- Árvore da biblioteca carrega sem excesso de dados e pode ser pesquisada.
- Dados de um professor não aparecem para outro.

## Encerramento

Esta spec termina apenas quando todos os itens estiverem marcados e com evidência registrada, no formato definido em [Como executar](../../shared/como-executar.md).
