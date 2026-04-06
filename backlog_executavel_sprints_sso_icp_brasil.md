# Backlog Executavel por Sprint - SSO ICP-Brasil (IA-First)

## 1. Parametros do backlog

1. Duracao recomendada de sprint: 2 semanas.
2. Cadencia de release: a cada 2 sprints apos Sprint 2.
3. Modelo de status por historia: Planned, In Progress, Blocked, In Review, Done.
4. Priorizacao: P0 (critico), P1 (alto), P2 (medio), P3 (baixo).
5. Estimativa: Story Points (SP) usando sequencia 1, 2, 3, 5, 8, 13.
6. Politica de merge: sem merge com testes falhando.
7. Politica de rastreabilidade: toda historia deve referenciar epico, fase e criterio de aceite.

## 2. Fases para sprints

| Fase | Sprints | Resultado alvo |
|---|---|---|
| Fase 0 - Fundacao | Sprint 0 | Base tecnica e governanca prontas |
| Fase 1 - Motor criptografico | Sprint 1 e Sprint 2 | Validacao ICP-Brasil completa |
| Fase 2 - Identidade e vinculo | Sprint 3 | Fluxo de cadastro/vinculo funcional |
| Fase 3 - Sessao e federacao | Sprint 4 e Sprint 5 | SSO funcional para sistemas clientes |
| Fase 4 - Portal e paineis | Sprint 6 e Sprint 7 | UX, administracao e auditoria prontas |
| Fase 5 - Seguranca e operacao | Sprint 7 e Sprint 8 | Hardening, observabilidade e resiliencia |
| Fase 6 - Homologacao e go-live | Sprint 9 | Producao com rollout controlado |
| Fase 7 - Evolucao continua | Sprint 10+ | Melhoria continua sem regressao |

## 3. Catalogo de epicos

1. E00 - Fundacao de engenharia e governanca.
2. E10 - Parsing e validacao de cadeia ICP-Brasil.
3. E11 - Revogacao e politicas de contingencia.
4. E12 - API de validacao criptografica e codigos de erro.
5. E20 - Identidade, cadastro e vinculo PF/PJ.
6. E30 - Sessao segura e tokens internos.
7. E31 - Cadastro de client apps e contratos de integracao.
8. E32 - Logout local e centralizado.
9. E40 - Portal de autenticacao.
10. E41 - Painel administrativo.
11. E42 - Painel de auditoria.
12. E50 - Hardening e seguranca de aplicacao.
13. E51 - Observabilidade, SLO e operacao.
14. E60 - Homologacao, rollout e estabilizacao.

## 4. Sprint backlog detalhado

## Sprint 0 - Fundacao

### Objetivo do sprint
Criar baseline de arquitetura, dados, pipeline e padroes para execucao paralela por agentes.

### Historias

#### US-0001 (E00, P0, 5 SP)
Responsavel: Agente Orquestrador Tecnico
Descricao: Definir estrutura de repositorio, convencao de branches e padrao de commits.
Dependencias: nenhuma
Criterios de aceite:
1. Estrutura de diretorios aprovada em ADR.
2. Convencao de branch e commit documentada.
3. Template de PR publicado.

#### US-0002 (E00, P0, 8 SP)
Responsavel: Agente Backend de Identidade
Descricao: Criar schema inicial e migracoes das tabelas centrais (`users`, `user_certificates`, `auth_sessions`, `auth_events`, `client_apps`, `audit_logs`).
Dependencias: US-0001
Criterios de aceite:
1. Migracoes idempotentes e versionadas.
2. Chaves primarias e indices basicos definidos.
3. Teste de migracao e rollback automatizado.

#### US-0003 (E00, P0, 5 SP)
Responsavel: Agente DevSecOps/SRE
Descricao: Configurar pipeline CI com lint, unit tests, SAST e scanner de dependencias.
Dependencias: US-0001
Criterios de aceite:
1. Pipeline executa em cada PR.
2. Pipeline bloqueia merge quando falhar.
3. Relatorio de seguranca anexado no PR.

#### US-0004 (E00, P1, 3 SP)
Responsavel: Agente de Documentacao Tecnica
Descricao: Publicar ADR-001 (arquitetura base) e Definition of Done por tipo de tarefa.
Dependencias: US-0001
Criterios de aceite:
1. ADR-001 aprovado.
2. DoD publicado para backend, frontend, dados e seguranca.
3. Repositorio com pasta de ADR padronizada.

## Sprint 1 - Criptografia base

### Objetivo do sprint
Entregar parsing X.509 e validacao de cadeia ate ancora de confianca ICP-Brasil.

### Historias

#### US-1001 (E10, P0, 8 SP)
Responsavel: Agente de Criptografia e PKI
Descricao: Implementar parser X.509 para extrair subject, issuer, serial, validade e OIDs relevantes.
Dependencias: US-0002
Criterios de aceite:
1. Extracao de campos obrigatorios testada com certificados validos.
2. Erro padronizado para certificado malformado.
3. Cobertura de testes do parser >= alvo definido no projeto.

#### US-1002 (E10, P0, 13 SP)
Responsavel: Agente de Criptografia e PKI
Descricao: Implementar chain building e validacao da cadeia ate ancora ICP-Brasil.
Dependencias: US-1001
Criterios de aceite:
1. Cadeia valida autenticada com sucesso.
2. Cadeia incompleta e cadeia nao confiavel rejeitadas com motivo especifico.
3. Testes com cenarios de multiplas ACs credenciadas.

#### US-1003 (E10, P1, 5 SP)
Responsavel: Agente QA e Testes
Descricao: Criar suite de testes de conformidade de cadeia e validade basica.
Dependencias: US-1002
Criterios de aceite:
1. Suite automatizada integrada no CI.
2. Minimo de casos validos e invalidos cobrindo cadeia e periodo.
3. Relatorio de execucao armazenado como evidencia.

## Sprint 2 - Revogacao e API criptografica

### Objetivo do sprint
Concluir validacoes criptograficas obrigatorias com revogacao e API interna de validacao.

### Historias

#### US-1101 (E11, P0, 13 SP)
Responsavel: Agente de Criptografia e PKI
Descricao: Implementar verificacao de revogacao com suporte a politicas configuraveis e cache controlado.
Dependencias: US-1002
Criterios de aceite:
1. Certificado revogado sempre rejeitado.
2. Politica de falha externa configuravel e auditada.
3. Cache de revogacao com TTL e invalidacao documentados.

#### US-1102 (E10, P0, 5 SP)
Responsavel: Agente de Criptografia e PKI
Descricao: Implementar validacao de uso/tipo de certificado para autenticacao.
Dependencias: US-1001
Criterios de aceite:
1. Certificado incompativel com autenticacao e rejeitado.
2. Motivo de rejeicao retornado em codigo padronizado.
3. Testes cobrindo casos de uso permitido e negado.

#### US-1201 (E12, P0, 8 SP)
Responsavel: Agente Backend de Identidade
Descricao: Expor API interna de validacao criptografica com codigos de erro estaveis.
Dependencias: US-1101, US-1102
Criterios de aceite:
1. Contrato versionado e documentado.
2. Respostas incluem resultado, motivo e metadados minimos.
3. Testes de contrato no CI.

#### US-1202 (E12, P1, 3 SP)
Responsavel: Agente de Documentacao Tecnica
Descricao: Publicar guia tecnico de codigos de erro criptografico.
Dependencias: US-1201
Criterios de aceite:
1. Guia com exemplos de request/response.
2. Mapeamento de erro para acao recomendada no frontend.

## Sprint 3 - Identidade e vinculo

### Objetivo do sprint
Entregar fluxo completo de identificacao, cadastro inicial e vinculo de certificado para PF/PJ.

### Historias

#### US-2001 (E20, P0, 8 SP)
Responsavel: Agente Backend de Identidade
Descricao: Implementar extracao e normalizacao de CPF/CNPJ e regra de match com cadastro interno.
Dependencias: US-1201
Criterios de aceite:
1. Normalizacao consistente para todos os fluxos.
2. Match por CPF/CNPJ funcionando para conta existente.
3. Evento de auditoria gerado no processo de match.

#### US-2002 (E20, P0, 8 SP)
Responsavel: Agente Backend de Identidade
Descricao: Implementar cadastro automatico com autorizacao minima quando nao houver usuario.
Dependencias: US-2001
Criterios de aceite:
1. Usuario criado com perfil minimo e status controlado.
2. Sem pre-cadastro obrigatorio por padrao.
3. Evento de criacao e vinculo registrado em auditoria.

#### US-2003 (E20, P1, 5 SP)
Responsavel: Agente Backend de Identidade
Descricao: Implementar parametro para exigir pre-cadastro quando politica exigir.
Dependencias: US-2002
Criterios de aceite:
1. Politica configuravel por ambiente.
2. Fluxo bloqueado com mensagem clara quando pre-cadastro for obrigatorio.
3. Cobertura de teste para os dois modos de politica.

#### US-2004 (E20, P1, 5 SP)
Responsavel: Agente Backend de Identidade
Descricao: Implementar suporte a multiplos certificados por usuario com historico de vinculos.
Dependencias: US-2001
Criterios de aceite:
1. Multiplos certificados ativos por usuario suportados.
2. Historico de alteracoes de vinculo persistido.
3. Consulta administrativa por usuario e certificado disponivel.

#### US-2005 (E20, P1, 3 SP)
Responsavel: Agente QA e Testes
Descricao: Criar testes E2E do fluxo de primeiro vinculo (sucesso e bloqueio).
Dependencias: US-2002, US-2003
Criterios de aceite:
1. Fluxo de primeiro acesso coberto.
2. Fluxo de bloqueio por politica coberto.
3. Evidencias de execucao anexadas no pipeline.

## Sprint 4 - Sessao segura

### Objetivo do sprint
Implementar sessao autenticada com controles de seguranca e emissao de token interno.

### Historias

#### US-3001 (E30, P0, 8 SP)
Responsavel: Agente de Sessao e Federacao
Descricao: Implementar criacao de sessao com expiracao, renovacao segura e invalidacao.
Dependencias: US-2002
Criterios de aceite:
1. Sessao criada apos autenticacao valida.
2. Expiracao e renovacao funcionando conforme politica.
3. Revogacao de sessao ativa aplicada em tempo aceitavel.

#### US-3002 (E30, P0, 8 SP)
Responsavel: Agente de Sessao e Federacao
Descricao: Implementar protecoes contra replay e fixation.
Dependencias: US-3001
Criterios de aceite:
1. Sessao antiga invalidada apos reautenticacao conforme politica.
2. Tentativa de reutilizacao de token invalido bloqueada.
3. Testes de seguranca automatizados cobrindo cenarios principais.

#### US-3003 (E30, P1, 5 SP)
Responsavel: Agente Backend de Identidade
Descricao: Registrar eventos de ciclo de vida de sessao em `auth_events`.
Dependencias: US-3001
Criterios de aceite:
1. Criacao, renovacao e encerramento de sessao auditados.
2. Consulta por periodo e usuario disponivel.

## Sprint 5 - Federacao e integracao cliente

### Objetivo do sprint
Permitir integracao de sistemas clientes com cadastro controlado e contratos de autenticacao.

### Historias

#### US-3101 (E31, P0, 8 SP)
Responsavel: Agente de Sessao e Federacao
Descricao: Implementar cadastro de `client_apps` com identificador unico e validacao de redirect URI.
Dependencias: US-3001
Criterios de aceite:
1. Cadastro de cliente com status ativo/inativo.
2. Redirect URI fora da whitelist rejeitada.
3. Auditoria de alteracoes administrativas ativa.

#### US-3102 (E31, P0, 8 SP)
Responsavel: Agente de Sessao e Federacao
Descricao: Implementar endpoints de delegacao de autenticacao para sistemas clientes.
Dependencias: US-3101
Criterios de aceite:
1. Fluxo de redirecionamento e retorno autenticado funcional.
2. Claims minimas entregues ao sistema cliente.
3. Teste de contrato publicado e automatizado.

#### US-3201 (E32, P1, 5 SP)
Responsavel: Agente de Sessao e Federacao
Descricao: Implementar logout local e centralizado.
Dependencias: US-3102
Criterios de aceite:
1. Logout local encerra sessao no SSO.
2. Logout central invalida sessao para cliente integrado.
3. Eventos de logout auditados com rastreabilidade.

#### US-3103 (E31, P1, 3 SP)
Responsavel: Agente de Documentacao Tecnica
Descricao: Publicar especificacao de integracao para sistemas clientes.
Dependencias: US-3102
Criterios de aceite:
1. Documento de onboarding tecnico pronto.
2. Exemplos de fluxo de autenticacao e erros comuns incluidos.

## Sprint 6 - Portal de autenticacao e administracao

### Objetivo do sprint
Entregar interface de autenticacao com mensagens de erro claras e painel administrativo inicial.

### Historias

#### US-4001 (E40, P0, 8 SP)
Responsavel: Agente Frontend
Descricao: Implementar portal de autenticacao com opcao por certificado e instrucoes de uso A1/A3.
Dependencias: US-3102
Criterios de aceite:
1. Fluxo de login inicia e conclui no portal.
2. Instrucoes de uso exibidas de forma clara.
3. Interface responsiva para desktop e mobile.

#### US-4002 (E40, P0, 5 SP)
Responsavel: Agente Frontend
Descricao: Implementar tratamento de erros por categoria (cadeia, validade, revogacao, vinculo, sessao).
Dependencias: US-1202, US-4001
Criterios de aceite:
1. Mensagem especifica para cada categoria de erro.
2. Mensagens sem vazamento de dado sensivel.
3. Testes E2E para erros principais.

#### US-4101 (E41, P1, 8 SP)
Responsavel: Agente Frontend
Descricao: Implementar painel administrativo para usuarios, vinculos e sistemas clientes.
Dependencias: US-3101, US-2004
Criterios de aceite:
1. CRUD administrativo com controle de permissao.
2. Alteracoes administrativas registradas em audit log.
3. Filtros por usuario e status disponiveis.

## Sprint 7 - Auditoria e hardening

### Objetivo do sprint
Concluir painel de auditoria e baseline de seguranca de aplicacao.

### Historias

#### US-4201 (E42, P0, 8 SP)
Responsavel: Agente Frontend
Descricao: Implementar painel de auditoria com filtros por usuario, certificado, sistema cliente e periodo.
Dependencias: US-3003, US-4101
Criterios de aceite:
1. Consultas por filtros obrigatorios funcionando.
2. Exportacao controlada por perfil autorizado.
3. Tempo de resposta dentro de meta definida.

#### US-5001 (E50, P0, 8 SP)
Responsavel: Agente DevSecOps/SRE
Descricao: Aplicar hardening de API e sessao (headers, CSRF, validacao de entrada, politica de token).
Dependencias: US-3002
Criterios de aceite:
1. Checklist de hardening aplicado e versionado.
2. Testes de seguranca automatizados sem falha critica.
3. Evidencias publicadas por release.

#### US-5002 (E50, P1, 5 SP)
Responsavel: Agente DevSecOps/SRE
Descricao: Implementar politicas de segredos, rotacao e mascaramento de logs.
Dependencias: US-5001
Criterios de aceite:
1. Segredos removidos de codigo e pipeline.
2. Rotacao documentada e testada.
3. Logs sem dados sensiveis expostos.

#### US-4202 (E42, P1, 3 SP)
Responsavel: Agente QA e Testes
Descricao: Criar suite de regressao de auditoria e autorizacao de acesso a logs.
Dependencias: US-4201
Criterios de aceite:
1. Acesso indevido a auditoria bloqueado.
2. Suite integrada ao pipeline.

## Sprint 8 - Observabilidade e resiliencia

### Objetivo do sprint
Consolidar operacao com metricas, alertas, testes de carga e contingencia.

### Historias

#### US-5101 (E51, P0, 8 SP)
Responsavel: Agente DevSecOps/SRE
Descricao: Implementar dashboards de autenticacao, latencia, erro e disponibilidade.
Dependencias: US-3001, US-5001
Criterios de aceite:
1. Dashboards publicados por ambiente.
2. Metricas p50/p95 e taxa de falha disponiveis.
3. Alertas de degradacao configurados.

#### US-5102 (E51, P0, 8 SP)
Responsavel: Agente DevSecOps/SRE
Descricao: Implementar plano de contingencia para indisponibilidade de validacoes externas.
Dependencias: US-1101
Criterios de aceite:
1. Politica de fallback aplicada conforme configuracao.
2. Comportamento em falha externa testado e auditado.
3. Runbook de contingencia publicado.

#### US-5103 (E51, P1, 5 SP)
Responsavel: Agente QA e Testes
Descricao: Rodar teste de carga de autenticacao e registrar baseline de desempenho.
Dependencias: US-5101
Criterios de aceite:
1. Relatorio com throughput e latencia p95 gerado.
2. Gargalos principais identificados.
3. Plano de acao para gargalos priorizado no backlog.

## Sprint 9 - Homologacao e go-live

### Objetivo do sprint
Homologar ponta a ponta com sistemas clientes e executar rollout controlado.

### Historias

#### US-6001 (E60, P0, 8 SP)
Responsavel: Agente QA e Testes
Descricao: Executar homologacao integrada com certificados validos e cenarios de falha.
Dependencias: Sprint 1 a Sprint 8 concluido
Criterios de aceite:
1. Roteiro de homologacao executado integralmente.
2. Falhas classificadas por severidade.
3. Evidencias arquivadas para auditoria do projeto.

#### US-6002 (E60, P0, 5 SP)
Responsavel: Agente DevSecOps/SRE
Descricao: Publicar plano de rollout em ondas com gatilhos de rollback.
Dependencias: US-6001
Criterios de aceite:
1. Criticos de rollback definidos por KPI.
2. Procedimento de rollback testado em ambiente de homologacao.
3. Aprovacao operacional registrada.

#### US-6003 (E60, P1, 5 SP)
Responsavel: Agente Orquestrador Tecnico
Descricao: Executar piloto com sistemas clientes prioritarios e consolidar ajustes de estabilizacao.
Dependencias: US-6002
Criterios de aceite:
1. Piloto executado sem incidente critico.
2. Ajustes de estabilizacao implementados e validados.
3. Go-live aprovado com checklists completos.

## Sprint 10+ - Operacao evolutiva continua

### Objetivo continuo
Garantir evolucao incremental sem regressao do nucleo ICP-Brasil.

### Historias recorrentes

#### US-7001 (E60, P1, 5 SP por sprint)
Responsavel: Agente Orquestrador Tecnico
Descricao: Repriorizar backlog por impacto de negocio, risco tecnico e custo operacional.
Criterios de aceite:
1. Backlog atualizado no inicio de cada sprint.
2. Dependencias e riscos revisados.

#### US-7002 (E51, P1, 3 SP por sprint)
Responsavel: Agente DevSecOps/SRE
Descricao: Revisar SLO/KPIs e propor melhorias de confiabilidade.
Criterios de aceite:
1. Relatorio de saude tecnica publicado por sprint.
2. Acoes corretivas com ownership definido.

#### US-7003 (E00, P2, 3 SP por sprint)
Responsavel: Agente de Documentacao Tecnica
Descricao: Atualizar ADRs, runbooks e base de licoes aprendidas.
Criterios de aceite:
1. Toda decisao relevante convertida em artefato de memoria.
2. Base de conhecimento atualizada sem lacunas criticas.

## 5. Definition of Ready (DoR)

1. Historia possui objetivo claro e escopo fechado.
2. Dependencias tecnicas identificadas.
3. Criterios de aceite objetivos definidos.
4. Agente dono e estimativa definidos.

## 6. Definition of Done (DoD)

1. Codigo implementado e revisado.
2. Testes automatizados criados/atualizados e verdes.
3. Auditoria e logs da funcionalidade validados.
4. Documentacao tecnica atualizada.
5. Evidencias anexadas no PR e no registro da sprint.

## 7. KPIs de controle de execucao

1. Percentual de historias concluidas por sprint.
2. Taxa de retrabalho por sprint.
3. Defeitos encontrados apos merge por sprint.
4. Cobertura de testes dos modulos criticos.
5. Taxa de sucesso de autenticacao por ambiente.
6. Latencia p95 de autenticacao.
7. MTTR de incidentes relacionados a autenticacao.

## 8. Matriz de ownership por dominio

| Dominio | Agente principal | Agente apoio |
|---|---|---|
| Criptografia e cadeia | Agente de Criptografia e PKI | Agente QA e Testes |
| Identidade e vinculo | Agente Backend de Identidade | Agente Frontend |
| Sessao e federacao | Agente de Sessao e Federacao | Agente Backend de Identidade |
| Frontend portal/admin/auditoria | Agente Frontend | Agente QA e Testes |
| Seguranca e operacao | Agente DevSecOps/SRE | Agente Orquestrador Tecnico |
| Documentacao e memoria | Agente de Documentacao Tecnica | Agente Orquestrador Tecnico |

## 9. Fora de escopo deste backlog

1. Fluxos de assinatura digital.
2. Carimbo do tempo.
3. Cofre de certificados.
4. Gestao de procuracoes.
5. Identidade internacional.
