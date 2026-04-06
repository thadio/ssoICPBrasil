# Diretrizes Gerais de Controle de Andamento e Qualidade Continua

## 1. Objetivo

1. Estabelecer um sistema de governanca para execucao do projeto por agentes de IA especialistas.
2. Garantir previsibilidade de entrega, qualidade tecnica recorrente e seguranca operacional.
3. Reduzir desperdicio de tokens sem perda de contexto tecnico.
4. Preservar historico completo de desenvolvimento para rastreabilidade e inteligencia acumulada.

## 2. Modelo de governanca do projeto

## 2.1 Estrutura minima de papeis

1. Orquestrador do Projeto
- Prioriza backlog, libera fases e aprova gates.

2. Responsavel Tecnico por Dominio
- Aprova mudancas de arquitetura do seu dominio.

3. Agentes Especialistas
- Implementam tarefas com saida objetiva e testavel.

4. Agente de Qualidade
- Bloqueia entrega sem evidencias minimas de teste e seguranca.

5. Curador de Conhecimento
- Mantem memoria tecnica viva e organizada.

## 2.2 Artefatos obrigatorios de governanca

1. Backlog mestre com prioridade e dependencia.
2. Quadro de status por fase e por modulo.
3. Registro de decisoes arquiteturais (ADRs).
4. Registro de riscos e plano de mitigacao.
5. Relatorio semanal de KPIs tecnicos.

## 3. Ritmo operacional (cadencia)

## 3.1 Ciclo diario

1. Planejar lote de tarefas de baixo acoplamento.
2. Executar tarefas em paralelo por agentes especialistas.
3. Validar testes automatizados e qualidade de codigo.
4. Atualizar memoria tecnica e log de decisoes.
5. Consolidar merge apenas com gates verdes.

## 3.2 Ciclo semanal

1. Revisao de arquitetura e divida tecnica.
2. Revisao de risco operacional e seguranca.
3. Revisao de performance e custos.
4. Repriorizacao do backlog por impacto e risco.

## 3.3 Ciclo por release

1. Congelar escopo da release.
2. Rodar regressao completa.
3. Publicar changelog tecnico e operacional.
4. Executar rollout em ondas e monitorar KPIs.
5. Executar retrospectiva tecnica com plano de melhoria.

## 4. Controle de andamento do projeto

## 4.1 Status padrao por tarefa

1. Planned
2. In Progress
3. Blocked
4. In Review
5. Done

## 4.2 Metricas obrigatorias

1. Lead time por tarefa e por fase.
2. Throughput semanal por agente e por modulo.
3. Taxa de retrabalho por causa raiz.
4. Defeitos por release e defeitos escapados.
5. Cobertura de testes em modulos criticos.
6. Tempo medio de restauracao (MTTR) em incidentes.
7. Taxa de sucesso de autenticacao e latencia p95.

## 4.3 Regras para desbloqueio rapido

1. Toda tarefa bloqueada precisa de causa, dono e prazo de resolucao.
2. Bloqueio com impacto de caminho critico recebe prioridade maxima.
3. Se bloqueio ultrapassar SLA interno, o orquestrador redistribui ownership.

## 5. Aplicacao frequente de boas praticas

## 5.1 Engenharia de codigo

1. Tarefas pequenas, com escopo fechado e criterio de aceite objetivo.
2. PRs curtos e revisaveis, sem misturar refatoracao com feature.
3. Testes automatizados no mesmo PR da funcionalidade.
4. Sem merge com teste falhando.
5. Sem alteracao de comportamento sem teste de regressao.

## 5.2 Seguranca por padrao

1. Segredos fora do codigo e com rotacao definida.
2. Validacao de entrada e tratamento consistente de erro.
3. Menor privilegio para acesso a dados e servicos.
4. Logs sem exposicao de dados sensiveis.
5. SAST, DAST e analise de dependencias em pipeline.

## 5.3 Qualidade de arquitetura

1. Mudanca arquitetural relevante exige ADR.
2. Reuso de componentes comuns antes de criar novos.
3. Limites de dominio explicitos entre modulos.
4. Contratos de API versionados e testados.

## 6. Otimizacao de uso de tokens

## 6.1 Regras de contexto

1. Enviar para agentes somente contexto necessario da tarefa atual.
2. Referenciar arquivos por caminho e trecho, evitando colagem integral repetida.
3. Usar sumarios incrementais para conversas longas.
4. Evitar repeticao de prompt base quando ele ja estiver em memoria de sessao.

## 6.2 Estrategia de prompts

1. Prompts com objetivo, entrada, restricoes, saida esperada e criterio de aceite.
2. Uma tarefa por prompt sempre que possivel.
3. Padrao de resposta estruturada para facilitar validacao automatica.
4. Rejeitar respostas sem evidencias tecnicas quando evidencias forem exigidas.

## 6.3 Politica de economia de tokens

1. Limitar tamanho maximo de contexto por tarefa.
2. Priorizar diff e snippets ao inves de arquivos completos.
3. Armazenar conhecimento consolidado em arquivos de referencia para reuso.
4. Encerrar agentes inativos para evitar ciclos desnecessarios.

## 7. Aumento continuo da inteligencia do projeto

## 7.1 Memoria em camadas

1. Memoria curta (sessao): resumo de decisoes do dia.
2. Memoria media (sprint): licoes aprendidas e riscos recorrentes.
3. Memoria longa (projeto): ADRs, padroes, incidentes, runbooks e guidelines.

## 7.2 Loop de aprendizado

1. Coletar erros recorrentes por categoria.
2. Atualizar checklists e templates com base nos erros.
3. Transformar correcoes frequentes em automacoes.
4. Reavaliar periodicamente prompts base para ganho de precisao.

## 7.3 Base de conhecimento viva

1. `docs/adr/` para decisoes de arquitetura.
2. `docs/runbooks/` para operacao e incidentes.
3. `docs/playbooks/` para fluxos recorrentes de engenharia.
4. `docs/lessons-learned/` para aprendizado cumulativo.

## 8. Preservacao integral do historico de desenvolvimento

## 8.1 Historico de codigo

1. Nao reescrever historico compartilhado de branch principal.
2. Commits atomicos, com mensagem clara e referenciando requisito.
3. Changelog por release com impacto funcional e tecnico.
4. Tags de release para marco operacional.

## 8.2 Historico de decisoes

1. Toda decisao arquitetural relevante deve virar ADR.
2. Toda mudanca de regra de negocio deve registrar motivo e impacto.
3. Toda excecao operacional deve registrar incidente e acao corretiva.

## 8.3 Historico de evidencias

1. Armazenar resultados de testes criticos por release.
2. Armazenar evidencias de homologacao e validacao de seguranca.
3. Armazenar logs de auditoria de alteracoes administrativas.

## 9. Checklists obrigatorios

## 9.1 Checklist de inicio de tarefa

1. Requisito claramente identificado.
2. Dependencias mapeadas.
3. Criterio de aceite definido.
4. Plano de teste definido.

## 9.2 Checklist de merge

1. Testes verdes.
2. Revisao tecnica concluida.
3. Sem segredo em codigo.
4. Documentacao atualizada.
5. Evidencias anexadas.

## 9.3 Checklist pos-release

1. KPIs monitorados.
2. Alertas sem anomalia critica.
3. Incidentes registrados e classificados.
4. Melhorias adicionadas ao backlog.

## 10. Politica de auditoria do proprio processo de IA

1. Toda execucao de agente deve ser rastreavel por tarefa e commit.
2. Toda resposta de agente usada em merge deve ter validacao automatica correspondente.
3. Toda falha relevante de agente deve gerar acao preventiva no processo.
4. O processo de engenharia por IA deve ser auditado mensalmente para evolucao de qualidade, custo e velocidade.
