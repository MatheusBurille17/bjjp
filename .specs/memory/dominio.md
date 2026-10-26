# Domínio

Entidades, estados e regras centrais do produto. Detalhes de campos em [Planejamento](../../.docs/planejamento_app_professores_jiu_jitsu.md), seção 8.

## Entidades principais

- `profiles` — conta autenticada (nome, e-mail).
- `teachers` — configurações do professor (nome público, fuso horário).
- `enrollments` — vínculo professor–aluno. Status: `pending`, `active`, `paused`, `archived`.
- `monthly_plans` — plano mensal. Status: `draft`, `published`, `archived`.
- `plan_days` — itens do plano (dia da semana, tema, objetivo, observações, ordem).
- `plan_day_videos` — vídeos sugeridos por dia do plano.
- `plan_enrollments` — a quem um plano publicado está destinado.
- `techniques` / `modules` — biblioteca, com ordem e status `active` ou `archived`.
- `videos` — metadados e referência ao provedor. Status: `uploading`, `processing`, `ready`, `error`, `archived`.
- `weekly_slots` — horário recorrente. Status: `active` ou `paused`.
- `lessons` — aula concreta. Status: `scheduled`, `completed`, `cancelled`, `no_show`.
- `lesson_videos` — vídeos da sessão, com `source` (`plan` ou `manual`) e snapshot do título.
- `student_technique_access` — liberações explícitas de técnica ou módulo para um vínculo.

## Estados da aula

| Estado | Conta como realizada | Observação |
| --- | --- | --- |
| `scheduled` | Não | Agendada |
| `completed` | **Sim** | Única fonte de contagem de aulas realizadas |
| `cancelled` | Não | Não conta como falta |
| `no_show` | Não | Aluno não compareceu |

Reagendar cria nova ocorrência ligada à original; a original permanece com histórico (`cancelled` ou equivalente) e relação explícita.

## Separação de conceitos

- **Plano** = intenção de ensino.
- **Horário recorrente** = regra de geração.
- **Aula** = evento real, com conteúdo copiado no momento da sessão.
