# Módulo: Vídeos

## Para que serve

Guardar metadados dos vídeos técnicos e controlar o acesso à reprodução. O arquivo fica em serviço de mídia externo; o banco guarda apenas referências.

## Conceitos principais

- **Vídeo**: título, descrição, provedor, identificador no provedor, miniatura, duração, módulo, status.
- **Status**: `uploading`, `processing`, `ready`, `error`, `archived`. Só `ready` pode ser reproduzido.
- **Token de reprodução**: gerado pelo servidor somente após verificar autorização.

## Regras de negócio relevantes

- Limitar tipos e tamanhos de upload.
- Nunca expor URL protegida sem checagem de autorização.
- Vídeo usado em aula concluída não pode ser excluído; apenas arquivado.
- Vídeo pode ser reutilizado em vários planos e aulas.

## Fronteiras

Não decide quem pode assistir a uma aula; essa decisão cruza `lessons`, `students` e `techniques` e é validada no caso de uso de reprodução.
