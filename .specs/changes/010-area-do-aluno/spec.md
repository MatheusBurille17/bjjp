# 010-area-do-aluno

## Objetivo

Dar ao aluno uma área própria para acompanhar agenda, histórico, plano publicado, biblioteca liberada e perfil, sem acessar dados administrativos do professor.

## Contexto Técnico

- Reaproveita os módulos `students`, `lessons`, `plans`, `videos` e `techniques`. Não cria módulo novo; a área do aluno é uma visão com regras de acesso.
- Um aluno pode ter vínculos com vários professores. Cada conteúdo mostra o professor responsável.
- Regra de acesso à biblioteca conforme [Produto](../../memory/produto.md): vídeo visível se associado a uma aula do aluno ou se a técnica/módulo foi liberada explicitamente.
- Reprodução usa o fluxo de token definido em [005-videos](../005-videos/spec.md).

## Referências de Projeto

- [Produto](../../memory/produto.md)
- [Contexto técnico global](../../memory/contexto-tecnico.md)
- [Dominio](../../memory/dominio.md)
- [Módulos](../../memory/modulos.md)
- [Módulo students](../../memory/modules/students.md)
- [Módulo lessons](../../memory/modules/lessons.md)
- [Módulo plans](../../memory/modules/plans.md)
- [Módulo videos](../../memory/modules/videos.md)
- [Módulo techniques](../../memory/modules/techniques.md)

## Referências Compartilhadas

- [Como executar](../../shared/como-executar.md)
- [Regras de nomenclatura](../../shared/regras-de-nomenclatura.md)

## Observações Locais

- Observações privadas do professor e campos de vínculo administrativos nunca são retornados.
- Plano só aparece se estiver `published` e atribuído ao vínculo do aluno. Conteúdo previsto não aparece como aula realizada.
- Aluno edita apenas nome de exibição, telefone e fuso horário.
- Biblioteca mostra vídeos `ready` permitidos, agrupados por técnica e módulo.
- Acesso é validado no servidor em cada requisição, não só na interface.

## Tasks

### Tasks - Negócio

- [ ] Criar o caso de uso `load-student-dashboard`, com a skill [module-use-case](../../../.agents/skills/module-use-case), que retorna próxima aula, total realizado e últimas concluídas.
- [ ] Criar o caso de uso `check-library-access`, com a skill [module-use-case](../../../.agents/skills/module-use-case), que aplica a regra primária de acesso a vídeos.
- [ ] Criar o caso de uso `load-student-lesson-detail`, que filtra observações privadas.
- [ ] Criar o caso de uso `update-student-profile`, restrito aos campos pessoais permitidos.
- [ ] Cobrir os casos de uso com testes unitários, incluindo acesso negado para vídeo não associado e para técnica não liberada.

### Tasks - Back-end

- [ ] Criar `student-area.controller.ts` com a skill [backend-nest-controller](../../../.agents/skills/backend-nest-controller), com endpoints de dashboard, agenda, plano publicado, biblioteca, histórico e perfil.
- [ ] Garantir que o controller mapeie as respostas para objetos simples, sem vazar campos internos.
- [ ] Criar `student-area.integration.http` cobrindo: aluno vendo apenas seus vínculos, observação privada oculta, rascunho oculto, vídeo não autorizado sem token.

### Tasks - Front-end

- [ ] Criar a área do aluno em `(private)` com menu próprio: Início, Minha agenda, Meu plano, Biblioteca, Meu histórico, Meu perfil.
- [ ] Criar o seletor de professor quando houver mais de um vínculo.
- [ ] Criar a tela de biblioteca do aluno com pesquisa e agrupamento por técnica e módulo.
- [ ] Criar o player do aluno, usando o token retornado pelo servidor e mostrando estado de erro quando não autorizado.
- [ ] Garantir layout responsivo para celular nas telas da área do aluno.
- [ ] Adicionar as chaves de i18n necessárias.
- [ ] Rodar `npx tsc --noEmit` em `apps/frontend` e sinalizar ao usuário que a UI está pronta para conferência manual.

## Resultado Esperado

- Aluno vê próxima aula, histórico real, plano publicado e biblioteca permitida.
- Aluno não vê rascunhos, observações privadas, dados de outros alunos ou campos administrativos.
- Aluno com vários professores consegue identificar a origem de cada conteúdo.
- Vídeo não autorizado não recebe token nem URL.

## Encerramento

Esta spec termina apenas quando todos os itens estiverem marcados e com evidência registrada, no formato definido em [Como executar](../../shared/como-executar.md).
