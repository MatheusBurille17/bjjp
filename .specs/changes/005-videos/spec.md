# 005-videos

## Objetivo

Permitir que o professor cadastre vídeos técnicos nos módulos da biblioteca, acompanhe o processamento e gere reprodução somente para quem tem autorização.

## Contexto Técnico

- Módulo de negócio `videos`, agregado `video`. Regras em [Módulo videos](../../memory/modules/videos.md).
- O banco guarda metadados e o identificador do provedor. O arquivo fica no serviço de mídia.
- Provedor candidato: Cloudflare Stream. A escolha final depende de comparação de custo, limites e controle de acesso (ver [Produto](../../memory/produto.md)). Esta spec assume o provedor por trás de uma porta (`video-provider`), para que a troca não afete o domínio.
- Reprodução usa token temporário emitido pelo servidor após checagem de autorização.

## Referências de Projeto

- [Produto](../../memory/produto.md)
- [Contexto técnico global](../../memory/contexto-tecnico.md)
- [Dominio](../../memory/dominio.md)
- [Módulos](../../memory/modulos.md)
- [Módulo videos](../../memory/modules/videos.md)
- [Módulo techniques](../../memory/modules/techniques.md)

## Referências Compartilhadas

- [Como executar](../../shared/como-executar.md)
- [Regras de nomenclatura](../../shared/regras-de-nomenclatura.md)

## Observações Locais

- Vídeo: título, descrição, provedor, identificador no provedor, miniatura, duração, módulo, status.
- Status: `uploading`, `processing`, `ready`, `error`, `archived`. Somente `ready` pode ser reproduzido.
- Validar tipo e tamanho do arquivo antes do envio.
- Vídeo associado a aula concluída não é excluído; apenas arquivado.
- A autorização de reprodução completa só funciona quando [008-agenda-e-aulas](../008-agenda-e-aulas/spec.md) e [010-area-do-aluno](../010-area-do-aluno/spec.md) estiverem prontos. Nesta spec, a autorização cobre apenas o professor dono do vídeo.
- Regra primária de acesso para o aluno, a ser aplicada em 011: vídeo visível se associado a uma aula do aluno ou se a técnica/módulo foi liberada.

## Tasks

### Tasks - Negócio (módulo `videos`)

- [ ] Criar o módulo com a skill [config-new-module](../../../.agents/skills/config-new-module).
- [ ] Criar o agregado `video` com a skill [module-aggregate](../../../.agents/skills/module-aggregate).
- [ ] Implementar a entidade `Video` com a skill [module-entity](../../../.agents/skills/module-entity), incluindo a máquina de estados de status.
- [ ] Definir o contrato do repositório e a porta `VideoProvider` (criar upload, consultar processamento, emitir token de reprodução) com a skill [module-repository](../../../.agents/skills/module-repository).
- [ ] Implementar `save-video`, `archive-video` e `request-video-playback` com a skill [module-use-case](../../../.agents/skills/module-use-case). `request-video-playback` só retorna token para vídeo `ready` e autorizado.
- [ ] Cobrir os casos de uso com testes unitários usando os fakes do módulo, incluindo transições inválidas de status.

### Tasks - Back-end

- [ ] Sincronizar o modelo `videos` no Prisma com a skill [backend-prisma-sync-module](../../../.agents/skills/backend-prisma-sync-module).
- [ ] Implementar o repositório Prisma com a skill [backend-prisma-repository](../../../.agents/skills/backend-prisma-repository).
- [ ] Implementar o adapter do provedor de vídeo com a skill [backend-provider-implementation](../../../.agents/skills/backend-provider-implementation), incluindo verificação de webhook ou polling de processamento.
- [ ] Criar `videos.controller.ts` com a skill [backend-nest-controller](../../../.agents/skills/backend-nest-controller), expondo criação, edição de metadados, arquivamento, listagem e pedido de reprodução.
- [ ] Criar `videos.integration.http` cobrindo upload inválido, status não pronto, reprodução autorizada e acesso cruzado.

### Tasks - Front-end

- [ ] Criar o fluxo de envio de vídeo com indicador de progresso e estado do processamento.
- [ ] Criar a lista de vídeos por módulo com estado visível (processando, pronto, erro).
- [ ] Criar o player do professor usando o token retornado pelo servidor.
- [ ] Adicionar as chaves de i18n necessárias.
- [ ] Rodar `npx tsc --noEmit` em `apps/frontend` e sinalizar ao usuário que a UI está pronta para conferência manual.

## Resultado Esperado

- Professor envia ou cadastra vídeos, associa a um módulo e acompanha o status.
- Vídeo com erro ou em processamento não é reproduzível e a interface informa o motivo.
- Reprodução só é emitida após checagem de autorização; caso contrário, nenhum token ou URL é retornado.
- Remoção de vídeo usado em aulas é bloqueada; arquivamento é a alternativa.

## Encerramento

Esta spec termina apenas quando todos os itens estiverem marcados e com evidência registrada, no formato definido em [Como executar](../../shared/como-executar.md).
