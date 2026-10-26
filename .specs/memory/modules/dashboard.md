# Módulo: Dashboard

## Para que serve

Mostrar de relance a rotina do professor e do aluno, a partir dos dados reais.

## Indicadores

- **Professor**: alunos ativos, aulas do dia, próximas aulas, aulas realizadas no mês, cancelamentos e faltas no período.
- **Aluno**: próxima aula, total de aulas realizadas, últimas aulas concluídas, próximos compromissos, vídeos da próxima aula, atalho para a biblioteca.

## Regras de negócio relevantes

- Somente leitura. Não grava dados.
- Escopo sempre do usuário autenticado.
- Contagens usam estados registrados (`completed`), não estimativas da agenda.
- Alunos arquivados não entram como ativos.
