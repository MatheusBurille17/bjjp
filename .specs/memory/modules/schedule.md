# Módulo: Agenda Recorrente

## Para que serve

Guardar a programação padrão de cada aluno (ex.: toda terça às 18h) e gerar ocorrências futuras de aula para revisão do professor.

## Conceitos principais

- **Horário recorrente** (`weekly_slot`): dia da semana, início, fim, validade, status `active` ou `paused`, fuso horário do professor.
- **Ocorrência**: aula concreta criada a partir de um horário recorrente. Pertence ao módulo `lessons`.

## Regras de negócio relevantes

- Recorrência é regra, não registro de aula.
- Geração limitada a uma janela (ex.: 4 semanas), com revisão antes de confirmar.
- Não gerar duplicatas: uma ocorrência por horário e data.
- Mudança de horário na recorrência não altera ocorrências já criadas sem ação explícita.
- Conflitos de horário são detectados antes de salvar.

## Fronteiras

Não guarda estado de aula. Estados e reagendamento ficam em `lessons`.
