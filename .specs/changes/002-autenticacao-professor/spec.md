# 002-autenticacao-professor

## Objetivo

Permitir que um professor crie conta, entre no sistema e complete o perfil profissional, com a área privada protegida por login.

## Contexto Técnico

- Módulo de negócio `auth` (conta e perfil). Regras gerais em [Módulo auth](../../memory/modules/auth.md) e [Contexto técnico](../../memory/contexto-tecnico.md).
- Entidades nesta spec: `profiles` (nome, e-mail, senha protegida) e `teachers` (nome público, fuso horário).
- O módulo `auth` não conhece JWT. O token é gerado no controller, a partir do retorno do caso de uso de login.
- Entrada de aluno (conta de aluno) não é tratada aqui; fica em [003-cadastro-aluno](../003-cadastro-aluno/spec.md) e [010-area-do-aluno](../010-area-do-aluno/spec.md).

## Referências de Projeto

- [Produto](../../memory/produto.md)
- [Contexto técnico global](../../memory/contexto-tecnico.md)
- [Dominio](../../memory/dominio.md)
- [Módulos](../../memory/modulos.md)
- [Módulo auth](../../memory/modules/auth.md)

## Referências Compartilhadas

- [Como executar](../../shared/como-executar.md)
- [Regras de nomenclatura](../../shared/regras-de-nomenclatura.md)

## Observações Locais

- Recuperação de senha **não** entra nesta spec. O planejamento a lista na Fase 1, mas a memória de `auth` a deixa de fora; fica como decisão pendente em [Produto](../../memory/produto.md).
- O caso de uso de login devolve apenas `{ id, name, email }`.
- Validação de senha forte com `shared-validation-rule` (regra `strong-password` já existente no pacote compartilhado).

## Tasks

### Tasks - Negócio (módulo `auth`)

- [ ] Executar a skill [spec-backend-auth-basic](../../../.agents/skills/spec-backend-auth-basic) para orquestrar o módulo `auth`, com agregado de usuário e perfil de professor.
- [ ] Criar a entidade de usuário com a skill [module-entity](../../../.agents/skills/module-entity), com `name`, `email` e `password` (hash).
- [ ] Criar a entidade de professor com a skill [module-entity](../../../.agents/skills/module-entity), com `displayName` e `timezone`.
- [ ] Definir o contrato do repositório de usuário e de professor com a skill [module-repository](../../../.agents/skills/module-repository).
- [ ] Implementar os casos de uso `register-user`, `login-user` e `save-teacher-profile` com a skill [module-use-case](../../../.agents/skills/module-use-case).
- [ ] Cobrir os casos de uso com testes unitários usando os fakes do módulo.

### Tasks - Back-end

- [ ] Sincronizar o modelo `auth` no Prisma com a skill [backend-prisma-sync-module](../../../.agents/skills/backend-prisma-sync-module).
- [ ] Implementar os repositórios Prisma com a skill [backend-prisma-repository](../../../.agents/skills/backend-prisma-repository).
- [ ] Implementar os providers de hash de senha e geração de id com a skill [backend-provider-implementation](../../../.agents/skills/backend-provider-implementation).
- [ ] Criar o `auth.controller.ts` com a skill [backend-nest-controller](../../../.agents/skills/backend-nest-controller), com endpoints públicos de cadastro e login, e endpoint autenticado para o perfil do professor.
- [ ] Criar `auth.integration.http` (Rest Client) cobrindo cadastro, login com credenciais erradas, acesso autenticado e acesso sem token.

### Tasks - Front-end

- [ ] Criar as telas de cadastro e login em `(public)` com a skill [frontend-next-config](../../../.agents/skills/frontend-next-config).
- [ ] Criar a tela de perfil do professor em `(private)`.
- [ ] Adicionar as chaves de i18n necessárias para `auth`.
- [ ] Rodar `npx tsc --noEmit` em `apps/frontend` e sinalizar ao usuário que a UI está pronta para conferência manual.

## Resultado Esperado

- Professor cria conta, entra e acessa a área privada.
- Rotas privadas exigem login; sem token, o backend recusa.
- Perfil do professor é editável e usa fuso horário.
- Senha nunca é armazenada em texto legível.

## Encerramento

Esta spec termina apenas quando todos os itens estiverem marcados e com evidência registrada, no formato definido em [Como executar](../../shared/como-executar.md).
