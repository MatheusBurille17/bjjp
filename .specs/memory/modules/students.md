# Módulo: Alunos (vínculos)

## Para que serve

Organizar a carteira de alunos de cada professor. A unidade central é o **vínculo** (`enrollment`), que pertence a um professor e pode apontar para uma conta de aluno.

## Conceitos principais

- **Vínculo**: relação entre professor e aluno, com status (`pending`, `active`, `paused`, `archived`), data de início e observações privadas.
- **Conta de aluno**: opcional enquanto o convite estiver pendente. Uma mesma conta pode ter vários vínculos.

## Regras de negócio relevantes

- Professor só gerencia vínculos próprios.
- Cadastrar um aluno não exige conta prévia.
- Vincular uma conta só é possível via convite seguro (link com validade).
- Arquivar preserva o histórico e tira o aluno dos indicadores ativos.
- Observações privadas pertencem ao vínculo e nunca são expostas ao aluno.

## Fronteiras

Não cuida de agenda, planos ou biblioteca. Esses módulos referenciam o vínculo, não o copiam.
