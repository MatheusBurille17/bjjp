# 011-testes-autorizacao-e-lancamento

## Objetivo

Garantir que os fluxos críticos e as regras de acesso funcionem de ponta a ponta antes de liberar o produto para um grupo pequeno de professores e alunos.

## Contexto Técnico

- Encerra o MVP. Cobre a Fase 7 do [planejamento](../../../.docs/planejamento_app_professores_jiu_jitsu.md).
- Testes de autorização são o foco principal, conforme a seção 14 do planejamento.
- Não há verificação automatizada de UI; a parte visual é validada manualmente.

## Referências de Projeto

- [Produto](../../memory/produto.md)
- [Contexto técnico global](../../memory/contexto-tecnico.md)
- [Dominio](../../memory/dominio.md)
- [Módulos](../../memory/modulos.md)

## Referências Compartilhadas

- [Como executar](../../shared/como-executar.md)
- [Regras de nomenclatura](../../shared/regras-de-nomenclatura.md)

## Observações Locais

- Recuperação de senha, se ainda não tiver sido definida, precisa de spec própria antes deste lançamento (ver [Produto](../../memory/produto.md)).
- Política de privacidade e tratamento de dados (LGPD) são registrados como documento, não como código.
- Backups e ambientes de desenvolvimento e produção são configurados conforme a plataforma escolhida.

## Tasks

### Tasks - Autorização

- [ ] Criar a suíte de testes de autorização cobrindo: professor A não acessa alunos, planos, técnicas, vídeos e aulas do professor B, mesmo alterando o id na URL ou no corpo.
- [ ] Cobrir a regra de aluno: não edita dados administrativos, não vê rascunho, não vê observação privada, não reproduz vídeo sem autorização.
- [ ] Cobrir a regra de aluno com vários professores: dados de um vínculo não aparecem no outro.

### Tasks - Fluxos críticos

- [ ] Criar testes de ponta a ponta para: cadastro de professor, cadastro e convite de aluno, criação e publicação de plano, geração e conclusão de aula, reprodução de vídeo autorizada.
- [ ] Cobrir a regra de histórico: editar plano não altera aula concluída; reagendar mantém a aula original.
- [ ] Cobrir a regra de contagem: cancelamento e falta não entram em aulas realizadas.
- [ ] Cobrir a geração de recorrências sem duplicatas ao repetir a operação.

### Tasks - Operação

- [ ] Verificar o fluxo de convite expirado e de token inválido.
- [ ] Verificar o tratamento de falha do provedor de vídeo (estado `error` e mensagem clara).
- [ ] Revisar logs, backups e tratamento de erros no ambiente de produção.
- [ ] Redigir a política de privacidade e o procedimento de exportação e exclusão de dados.

### Tasks - Front-end (validação manual)

- [ ] Rodar o produto em celular, tablet e computador e registrar os ajustes de usabilidade.
- [ ] Verificar navegação por teclado nos fluxos essenciais.
- [ ] Conduzir teste com um pequeno grupo de professores e alunos e registrar o feedback.

## Resultado Esperado

- Suíte de autorização passando sem exceções.
- Fluxos críticos funcionando de ponta a ponta.
- Vazamentos conhecidos de acesso entre professores e alunos corrigidos antes do lançamento.
- Política de privacidade e procedimentos de suporte disponíveis.

## Encerramento

Esta spec termina apenas quando todos os itens estiverem marcados e com evidência registrada, no formato definido em [Como executar](../../shared/como-executar.md).
