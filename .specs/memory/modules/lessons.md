# Módulo: Aulas

## Para que serve

Registrar cada aula concreta: quando aconteceu ou vai acontecer, com quem, com qual conteúdo e em que estado. É a base do histórico e dos indicadores.

## Conceitos principais

- **Aula** (`lesson`): data e hora de início e fim, vínculo, estado, observações compartilháveis e privadas, origem opcional no plano.
- **Estados**: `scheduled`, `completed`, `cancelled`, `no_show`.
- **Vídeos da aula** (`lesson_videos`): origem `plan` ou `manual`, com snapshot do título.

## Regras de negócio relevantes

- Apenas `completed` conta como realizada.
- Cancelamento e falta não contam como concluída.
- Reagendar cria nova ocorrência ligada à original; a original fica no histórico.
- Alterações em aula concluída são limitadas e registradas.
- Observações privadas nunca aparecem para o aluno.
- Fuso horário consistente em todas as datas.

## Fronteiras

Não gera recorrências (ver [schedule](schedule.md)) nem edita planos (ver [plans](plans.md)).
