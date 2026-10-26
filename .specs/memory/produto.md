# Produto: Plataforma para Professores de Jiu-Jitsu

Fonte completa: [Planejamento do aplicativo](../../.docs/planejamento_app_professores_jiu_jitsu.md).

## Para que serve

Plataforma web responsiva para professores particulares de Jiu-Jitsu organizarem alunos, planos mensais, agenda, biblioteca de técnicas e vídeos. Cada aluno tem uma área própria para consultar agenda, histórico, plano publicado e vídeos liberados.

## Conceitos principais

- **Professor**: dono do próprio espaço de dados (alunos, planos, técnicas, vídeos e aulas).
- **Aluno**: pessoa com conta própria, que pode ter vínculos com um ou mais professores.
- **Vínculo (`enrollment`)**: relação entre um professor e um aluno. Guarda status, observações privadas e permissões daquele acompanhamento.
- **Plano mensal**: intenção pedagógica do que será trabalhado num período. Não é aula realizada.
- **Horário recorrente**: regra padrão (ex.: toda terça às 18h). Não é aula realizada.
- **Aula**: ocorrência concreta com estado e conteúdo da sessão. É a fonte do histórico.
- **Biblioteca**: técnica → módulo → vídeo.

## Regras de negócio relevantes

- Só aulas `completed` contam como realizadas. Cancelamentos e faltas não contam.
- Plano em rascunho nunca aparece para o aluno.
- Editar um plano não altera silenciosamente aulas já concluídas.
- Observações privadas do professor nunca aparecem na área do aluno.
- Autorização de dados e de vídeos é validada no servidor, nunca só na interface.
- Arquivar alunos, planos, técnicas e vídeos preserva o histórico.

## Decisões pendentes

Itens do capítulo 19 do planejamento ainda não fechados. Enquanto não forem confirmados, cada spec usa o **padrão assumido** indicado abaixo e deve ser revisada antes de iniciar.

| Tema | Padrão assumido | Spec afetada |
| --- | --- | --- |
| Nome provisório do produto | Usar "Jiu-Jitsu" como nome de trabalho nas telas e no `.specs` | 001 |
| Aluno com mais de um professor no MVP | Sim, arquitetura multi-vínculo desde o início | 003, 010 |
| Convite do aluno | Link seguro por e-mail; telefone fica só como dado de contato | 003 |
| Planejamento do plano | Estruturado por dia da semana dentro de um período (`period_start`/`period_end`) | 006 |
| Publicação do plano | Por vínculo (`plan_enrollments`), não para todos os alunos automaticamente | 006 |
| Geração de aulas recorrentes | Gerar ocorrências numa janela limitada (ex.: 4 semanas), com revisão do professor antes de confirmar | 007 |
| Regra primária de acesso à biblioteca | Vídeo visível se estiver associado a uma aula do aluno ou se a técnica/módulo foi liberado explicitamente | 004, 005, 010 |
| Serviço de vídeo | Cloudflare Stream como candidato; escolha final após comparação de custo e controle de acesso | 005 |
| Recuperação de senha | Fora da spec 002; tratada em spec própria antes do lançamento | 002 |
