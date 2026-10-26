# Módulo: Planos de Aula

## Para que serve

Registrar a intenção pedagógica do professor por período: o que será trabalhado em cada dia, com técnicas e vídeos sugeridos.

## Conceitos principais

- **Plano mensal**: título, descrição, período, status (`draft`, `published`, `archived`).
- **Dia planejado** (`plan_day`): item do plano com dia da semana, tema, objetivo, observações e ordem.
- **Publicação**: feita para vínculos específicos via `plan_enrollments`.

## Regras de negócio relevantes

- Rascunho só é visível ao professor.
- Publicado só aparece para os vínculos atribuídos.
- Duplicar gera um novo plano em rascunho, sem alterar o original.
- Mudanças no plano não reescrevem aulas já concluídas; a aula guarda snapshot do conteúdo usado.
- Plano não é aula realizada e não entra na contagem de aulas.

## Fronteiras

Não agenda aulas. Associação a aulas acontece em `lessons`, que pode copiar um dia do plano.
