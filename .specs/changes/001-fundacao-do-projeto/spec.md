# 001-fundacao-do-projeto

## Objetivo

Criar a base técnica do monorepo do aplicativo de Jiu-Jitsu, com backend, frontend, pacote compartilhado, Prisma, autenticação JWT e tratamento de erros, para que os módulos de negócio sejam criados sobre ela.

## Contexto Técnico

- Mesma stack da [Contexto técnico global](../../memory/contexto-tecnico.md), com namespace npm `@bjjp`.
- `apps/frontend` (Next.js, porta 3000) e `apps/backend` (NestJS, porta 4000).
- Esta spec entrega apenas infraestrutura. Nenhum módulo de domínio é criado aqui.

## Referências de Projeto

- [Produto](../../memory/produto.md)
- [Contexto técnico global](../../memory/contexto-tecnico.md)
- [Módulos](../../memory/modulos.md)

## Referências Compartilhadas

- [Como executar](../../shared/como-executar.md)
- [Regras de nomenclatura](../../shared/regras-de-nomenclatura.md)

## Observações Locais

- Nome de trabalho do produto: "Jiu-Jitsu" (ver decisões pendentes em [Produto](../../memory/produto.md)). O nome final pode ser trocado depois sem alterar a estrutura.
- Não criar módulos de negócio nesta spec.
- Ao rodar `config-prisma`, o banco sobe via Docker Compose com nomes derivados do projeto `bjjp`.

## Tasks

### Tasks - Configuração

- [ ] Criar a estrutura base do monorepo com a skill [config-project-fullstack](../../../.agents/skills/config-project-fullstack) usando o namespace `@bjjp`.
- [ ] Configurar a infraestrutura do Prisma no backend com a skill [config-prisma](../../../.agents/skills/config-prisma).
- [ ] Criar o pacote compartilhado com a skill [config-package-shared](../../../.agents/skills/config-package-shared) usando o namespace `@bjjp`.
- [ ] Configurar tratamento de erros centralizado e autenticação JWT no backend com a skill [backend-nest-config](../../../.agents/skills/backend-nest-config).

### Tasks - Front-end

- [ ] Executar a skill [frontend-next-config](../../../.agents/skills/frontend-next-config) para criar `shared/` e as rotas `(public)` e `(private)` com sidebar de navegação.

### Tasks - Validação

- [ ] Rodar `npm run build` e `npm run check-types` na raiz e registrar o resultado.
- [ ] Subir backend e frontend e confirmar que ambos iniciam sem erros nas portas 4000 e 3000.

## Resultado Esperado

- Monorepo com `apps/backend` (porta 4000) e `apps/frontend` (porta 3000) sob o namespace `@bjjp`.
- Prisma com schema modular, `DbModule`, `PrismaService`, seed técnico e Docker Compose funcionando com o `DATABASE_URL` do `.env`.
- Pacote `@bjjp/shared` disponível para backend, frontend e módulos.
- Backend com erros padronizados (`ApiErrorResponse`) e guard JWT global com `@Public()`.
- Frontend com `shared/`, grupos `(public)` e `(private)` e sidebar funcional.

## Encerramento

Esta spec termina apenas quando todos os itens estiverem marcados e com evidência registrada, no formato definido em [Como executar](../../shared/como-executar.md).
