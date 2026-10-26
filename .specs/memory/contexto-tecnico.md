# Contexto Técnico Global

Stack, restrições e decisões transversais do projeto de Jiu-Jitsu. Specs individuais referenciam este arquivo em vez de repetir o conteúdo.

## Monorepo

- **Turborepo** (`turbo` 2.x), gerenciado por **npm workspaces** (`npm@10.9.2`, Node >= 18). Namespace npm do workspace: **`@bjjp`**.
- Workspaces: `apps/*`, `modules/*`, `packages/*`.
- Scripts na raiz: `npm run build|dev|lint|check-types` (delegam ao `turbo`) e `npm run format` (prettier).
- Linguagem 100% **TypeScript** (5.x). Prettier + ESLint configurados.

## Aplicações

- **`apps/backend`**: NestJS 11, porta **4000**. CORS habilitado, `@nestjs/config` global. Build com `nest build`. Testes com Jest + ts-jest (`*.spec.ts`, rootDir `src`).
- **`apps/frontend`**: Next.js 16 (App Router, React 19), porta **3000**. Tailwind CSS v4 + `@tailwindcss/typography`. UI sobre Radix UI + `lucide-react` + `class-variance-authority`. Gráficos com `recharts`. Toaster `sonner`. Sessão em cookie via `js-cookie`. Formulários com `react-hook-form` + `@hookform/resolvers`.

## Persistência

- **PostgreSQL** via **Prisma 7** (`@prisma/client`, `@prisma/adapter-pg`, `pg`).
- Schema **modular por domínio**: `apps/backend/prisma/schema.prisma` é só o root (generator + datasource); os models ficam em `apps/backend/prisma/models/<modulo>.model.prisma`.
- Migrations incrementais nomeadas por módulo em `apps/backend/prisma/migrations/`.
- Seed técnico em `apps/backend/prisma/seed/main.ts` (sem seeds de módulo).
- `DATABASE_URL` no `.env` do backend; Postgres sobe via `docker compose` (`npm run db:start` no backend).

## Arquitetura (portas e adaptadores)

- **Módulos de negócio** vivem em `modules/*` (pacotes npm puros, sem NestJS, sem Prisma, sem HTTP). Contêm `model/` (entidades), `provider/` (interfaces de repositório e portas + fakes), `usecase/` (casos de uso) e `constant/`. Organizados por **agregado**.
- **Pacote compartilhado** `packages/shared` (`@bjjp/shared`): classe `Entity` base, contrato `UseCase<IN,OUT>`, interfaces de repositório, `DomainError` e biblioteca de regras de validação (`Validator` + `*.rule.ts`).
- **Backend** apenas adapta: controllers finos em `apps/backend/src/modules/<modulo>`, implementações Prisma (`*.prisma.ts`) e providers técnicos, registrados no módulo Nest correspondente.
- Integrações externas (ex.: provedor de vídeo) ficam atrás de uma porta definida no módulo de negócio e implementada no backend.

## Convenções fixas

- **Linguagem do código em inglês** (`Lesson`, `ScheduleWeeklySlot`, `teacherId`); **conceitos do domínio em português** apenas em telas, mensagens e documentação ("Aula", "Plano de aula", "Vínculo", "Horário recorrente").
- **Nomenclatura**: arquivos e diretórios em `kebab-case`, sufixos por responsabilidade. Detalhe em [Regras de nomenclatura](../shared/regras-de-nomenclatura.md).
- **Sem DTOs de entrada** no backend: o controller recebe o `Body` cru e monta o input do caso de uso. **Respostas de leitura são mapeadas explicitamente** para objetos simples no controller, pois entidades usam `protected readonly props` com getters de prototype, que serializam como `{}` via `JSON.stringify`.
- **Casos de uso de comando retornam `void`**. Consultas (`findById`, `findPage`) não viram caso de uso: o controller chama o repositório direto.
- Caso de uso instanciado no corpo do método do controller, recebendo repositórios/providers injetados no construtor.
- **Validação de negócio dentro da entidade** (`validate()` com `Validator.validate`); no client, apenas `required` nos inputs.
- **Propriedade por professor**: todo registro administrativo grava `teacherId` (extraído do JWT) e filtra por ele. `findById(id)` não recebe `teacherId`; a checagem de propriedade é feita no caso de uso ou no controller: 403 em mutações cross-professor, 404 em leituras por id (não vazar existência alheia).
- **Acesso do aluno por vínculo**: todo dado visível ao aluno é validado contra o `enrollmentId` da sessão no servidor.

## Autenticação

- JWT no backend (`@nestjs/jwt` + `passport-jwt`, `bcrypt` para senha). `JwtAuthGuard` é **guard global** (`APP_GUARD`); endpoints públicos marcados com `@Public()`.
- **O módulo de negócio `auth` não conhece JWT/token/sessão**. O caso de uso `login-user` só devolve `{ id, name, email }`; o JWT é gerado **no `auth.controller.ts`**. Segredo em `JWT_SECRET`, expiração padrão ~7–14 dias.
- Frontend mantém sessão em cookie `auth_token` (`sameSite: lax`, `secure` em prod). Rotas privadas protegidas por `AuthGuard` envolvendo o grupo `(private)`.

## Tratamento de erros

- `DomainError(message, statusCode)` no domínio (`message` = chave i18n, ex.: `lesson.not_found`). `ApiExceptionFilter` global (`APP_FILTER`) converte em resposta padronizada `ApiErrorResponse` (`errors: string[]` com chaves i18n).
- Frontend traduz via `getMessage(key)` (`shared/i18n/`) e dispara um `toast` por item.

## Mídia (vídeos)

- Arquivos de vídeo ficam em serviço de mídia externo; o banco guarda metadados e o identificador do provedor.
- Reprodução usa **token temporário** emitido pelo servidor somente após checagem de autorização. Nunca expor URL protegida sem autorização.
- Provedor atrás da porta `VideoProvider`. Candidato: Cloudflare Stream, escolha final pendente (ver [Produto](produto.md)).

## Validação automatizada

- Testes unitários dos módulos com Jest e cenários HTTP em `*.integration.http` (Rest Client).
- Testes de autorização são obrigatórios em toda spec que expõe dados.
- **Não há verificação automatizada de UI**: a interface é validada manualmente.
