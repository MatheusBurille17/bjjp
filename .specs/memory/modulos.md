# Módulos de Negócio

Organização por domínio do produto de Jiu-Jitsu. Cada módulo tem sua memória em [`modules/`](modules/).

| Módulo | Responsabilidade | Memória |
| --- | --- | --- |
| `auth` | Conta, sessão JWT, perfil do usuário e perfil do professor | [auth](modules/auth.md) |
| `students` | Vínculos professor–aluno, convites, situação e ficha | [students](modules/students.md) |
| `techniques` | Técnicas e módulos da biblioteca | [techniques](modules/techniques.md) |
| `videos` | Metadados de vídeo, processamento e acesso à mídia | [videos](modules/videos.md) |
| `plans` | Planos mensais, dias planejados, publicação e vínculo com vídeos | [plans](modules/plans.md) |
| `schedule` | Horários recorrentes e geração de ocorrências | [schedule](modules/schedule.md) |
| `lessons` | Aulas concretas, estados, reagendamento, conclusão e histórico | [lessons](modules/lessons.md) |
| `dashboard` | Indicadores agregados do professor e do aluno (somente leitura) | [dashboard](modules/dashboard.md) |

Módulos futuros, fora do MVP: `notifications`, `billing`.

Dependências esperadas: `lessons` depende de `students`, `plans` e `schedule`; `videos` e `techniques` alimentam `plans` e `lessons`; `dashboard` lê dos demais.

Para o namespace npm dos módulos, use `@bjjp/<modulo>` (ver [Contexto técnico](contexto-tecnico.md)).
