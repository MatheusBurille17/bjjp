# Módulo: Técnicas

## Para que serve

Organizar o acervo técnico do professor em **técnica → módulo → vídeo**, para reutilização em planos e aulas.

## Conceitos principais

- **Técnica**: item da biblioteca (nome, descrição, categoria opcional, ordem, status `active` ou `archived`).
- **Módulo**: agrupamento dentro de uma técnica (nome, descrição, ordem, status).

## Regras de negócio relevantes

- Hierarquia simples e fixa no MVP: técnica contém módulos; módulos contêm vídeos.
- Professor só gerencia o próprio acervo.
- Arquivar técnica ou módulo não remove vínculos históricos com aulas.
- Liberação de acesso a alunos é controlada aqui (`student_technique_access`), seguindo a regra primária definida em [Produto](../produto.md).

## Fronteiras

Não armazena arquivos de vídeo; isso é responsabilidade de [videos](videos.md).
