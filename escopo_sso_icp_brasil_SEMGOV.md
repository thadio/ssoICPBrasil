# Escopo Completo do Sistema — SSO baseado em ICP-Brasil (Fase 1) e gov.br (Fase 2)

## 1. Visão geral

Este documento apresenta o escopo completo de um sistema de autenticação centralizada (SSO) cujo primeiro objetivo é permitir **login universal com certificado digital ICP-Brasil**, aceitando certificados válidos emitidos por qualquer Autoridade Certificadora credenciada na cadeia da ICP-Brasil.

A base normativa da ICP-Brasil prevê que aplicações que admitam determinado tipo de certificado contemplado pela ICP-Brasil **devem aceitar qualquer certificado de mesmo tipo, ou superior, emitido por qualquer AC credenciada pela AC Raiz**. Além disso, os certificados de tipo A são adequados para confirmação de identidade e assinatura de documentos eletrônicos.

Na prática, isso significa que o sistema não deve ficar amarrado a um único fornecedor. O usuário poderá autenticar-se com certificados emitidos por provedores como Serpro, Valid, Soluti, Certisign e outros integrantes da cadeia ICP-Brasil, desde que o certificado seja tecnicamente válido, esteja dentro do prazo, não esteja revogado e pertença ao titular correto. 
---

## 2. Objetivo do sistema

Construir uma camada institucional de autenticação forte que funcione como **provedor de identidade centralizado**, capaz de:

- autenticar usuários por certificado digital ICP-Brasil;
- validar tecnicamente o certificado e sua cadeia de confiança;
- vincular o certificado ao cadastro interno do usuário;
- emitir sessão autenticada para sistemas clientes;
- registrar trilha de auditoria completa;

O foco inicial é **autenticação**. A assinatura digital de documentos poderá ser suportada depois como uma capacidade adicional, mas não deve ser confundida com login. Embora os dois temas sejam relacionados, eles têm requisitos funcionais, técnicos e jurídicos distintos.

---

## 3. Princípios do produto

1. **Agnóstico de fornecedor**: aceitar qualquer certificado ICP-Brasil compatível, e não apenas algum fornecedor isolado como SerproID, Serpro, Valid, Soluti, Certisign, etc.
2. **Aderência à cadeia ICP-Brasil**: validar a cadeia até a AC Raiz da ICP-Brasil, operada no âmbito do ITI. 
3. **Segurança forte**: autenticação baseada em certificado, validação de revogação, sessão segura e auditoria.
4. **Interoperabilidade**: funcionar com certificado local, token, smartcard e, quando tecnicamente viável, fluxos remotos/nuvem do fornecedor.
5. **Centralização**: um login único para múltiplos sistemas internos.
6. **Rastreabilidade**: todo evento crítico precisa ser auditável.

---

## 4. Escopo funcional — Fase 1 (ICP-Brasil)

### 4.1. Função principal

O sistema atuará como um **Identity Provider institucional**, oferecendo autenticação centralizada para sistemas integrados.

### 4.2. O que o sistema deve fazer

#### 4.2.1. Login universal com certificado ICP-Brasil

O sistema deve permitir autenticação com certificados ICP-Brasil de pessoa física ou jurídica, conforme a regra de negócio definida para cada aplicação cliente.

No mínimo, o backend deverá verificar:

- cadeia até a AC Raiz ICP-Brasil;
- período de validade do certificado;
- status de revogação;
- tipo e uso adequado do certificado;
- OIDs e campos relevantes do certificado;
- CPF ou CNPJ do titular;
- correspondência entre o titular do certificado e a conta interna do sistema;
- integridade do fluxo de autenticação e da sessão resultante.

A exigência de aceitar qualquer certificado de mesmo tipo, ou superior, emitido por qualquer AC credenciada pela AC Raiz, decorre dos requisitos mínimos das políticas de certificado da ICP-Brasil.

#### 4.2.2. Cadastro e vinculação de identidade

O sistema deve possuir base própria de usuários e base própria deve fazer o vínculo entre a identidade do certificado e a conta institucional. Exemplos de estratégias de vinculação:

- CPF do certificado = CPF do cadastro interno;
- CNPJ do certificado = organização representada;
- certificado de representante legal vinculado a perfil de pessoa jurídica;
- múltiplos certificados válidos associados a um mesmo usuário.

Caso nao conste o cpf do titular ou o cnpj na base própria, deve ser criado o cadastro do usuário com as autorizações/autoridades mínimas ou zeradas.

#### 4.2.3. Emissão de sessão SSO

Após autenticação bem-sucedida, o sistema deve emitir uma sessão confiável para navegação no próprio portal e também disponibilizar mecanismo de SSO para sistemas terceiros integrados.

Essa camada pode ser implementada com:

- sessão web institucional;
- tokens internos de autenticação;
- protocolo padrão de federação em etapa própria, como OpenID Connect ou SAML, para os sistemas clientes.

#### 4.2.4. Portal de autenticação

O sistema deve possuir uma interface própria de autenticação contendo:

- página de entrada;
- identificação do modo de autenticação por certificado;
- instruções para uso com navegador, token, mídia criptográfica ou certificado remoto;
- mensagens claras de erro;
- visualização do nome do titular identificado;
- consentimento e aceite de termos, quando aplicável.

#### 4.2.5. Gestão de usuários e perfis

O sistema deve permitir:

- cadastro manual ou sincronizado de usuários;
- ativação e inativação de contas;
- associação de certificados;
- gestão de papéis e permissões;
- bloqueio de acesso em caso de inconsistência cadastral ou risco.

#### 4.2.6. Auditoria e conformidade

O sistema deve registrar, no mínimo:

- data e hora do evento;
- IP e user agent;
- sistema cliente de origem;
- certificado utilizado;
- serial number do certificado;
- emissor;
- sujeito (subject);
- CPF/CNPJ extraído;
- resultado da validação da cadeia;
- resultado da checagem de revogação;
- sucesso ou falha de autenticação;
- motivo de rejeição;
- identificador da sessão gerada.

---

## 5. Escopo funcional — Assinatura digital de documentos

### 5.1. Decisão de escopo

O projeto precisa deixar explícito desde o início que há duas possibilidades:

1. **o sistema apenas autentica usuários com certificado digital**; ou
2. **o sistema autentica e também oferece assinatura digital de documentos**.

Essas capacidades não são idênticas.

### 5.2. Recomendação de faseamento

Para reduzir risco, recomenda-se:

- **Fase 1**: autenticação ICP-Brasil e SSO;
- **Fase 1.5 ou Fase 2**: assinatura digital de documentos.

### 5.3. Se houver assinatura digital no futuro

O sistema deverá então suportar:

- preparação do documento a ser assinado;
- congelamento da versão a ser assinada;
- coleta do certificado/identidade do assinante;
- execução da assinatura via componente compatível;
- validação da assinatura gerada;
- armazenamento do documento assinado;
- trilha de auditoria específica da assinatura.

Leia a documentacao de integração com SerproID como forma (apenas um dos exemplos possíveis) de entender como o mercado trabalha as assinaturas, a documentação pública do SerproID mostra endpoints para recuperação do certificado autorizado, homologação de integração e operações baseadas em token, o que confirma a viabilidade de um fluxo de assinatura remota em arquitetura separada do login universal ICP-Brasil.

---

## 6. Escopo técnico — validações obrigatórias do backend

Para aceitar qualquer certificado ICP-Brasil, o backend precisa implementar um conjunto robusto de validações.

### 6.1. Validação da cadeia de certificação

O sistema deve:

- construir a cadeia do certificado apresentado;
- validar a assinatura de cada certificado intermediário;
- ancorar a confiança na AC Raiz da ICP-Brasil;
- rejeitar cadeias incompletas, inválidas ou não confiáveis.

### 6.2. Validade temporal

O sistema deve conferir:

- `notBefore`;
- `notAfter`;
- coerência temporal no momento da autenticação.

### 6.3. Revogação

O sistema deve verificar revogação por mecanismos previstos na cadeia aplicável, com suporte a políticas de disponibilidade, cache controlado e tratamento de falhas.

### 6.4. Uso adequado do certificado

Deve-se checar se o tipo e o uso do certificado são compatíveis com a autenticação pretendida. A documentação da ICP-Brasil estabelece que certificados do tipo A são utilizados em aplicações como confirmação de identidade e assinatura de documentos eletrônicos.

### 6.5. Extração de atributos do titular

O sistema deve extrair e normalizar informações como:

- nome do titular;
- CPF ou CNPJ;
- subject DN;
- issuer DN;
- serial number;
- OIDs de identificação relevantes;
- tipo do certificado;
- identificadores auxiliares necessários ao vínculo interno.

### 6.6. Vínculo com conta interna
Mas, o sistema deve por padrão:

- permitir autoassociação do primeiro certificado;
- não exigir pré-cadastro do usuário;
- permitir múltiplos certificados por usuário;
- tratar certificados PJ e PF com regras diferentes, considerando suas especificidades.

Mas, o sistema deve possibilitar parametrizar se:
- exige pré-cadastro do usuário;

### 6.7. Gestão de sessão

Após autenticar, o sistema deve emitir sessão com:

- expiração controlada;
- renovação segura;
- logout local e centralizado;
- revogação de sessão em caso de risco;
- proteção contra replay e fixation.

---

## 7. Canais de entrada do certificado

O projeto deve considerar múltiplas formas de apresentação do certificado:

### 7.1. Certificado no navegador / sistema operacional
- A1 em arquivo instalado;
- certificado acessível pelo navegador;
- integração via repositório local do sistema operacional.

### 7.2. Token ou smartcard
- A3 com middleware/driver;
- leitura pelo navegador ou componente cliente;
- dependência de compatibilidade do ambiente do usuário.

### 7.3. Certificado em nuvem
- fluxos remotos providos por fornecedores específicos;
- necessidade de integração dedicada por API ou protocolo do prestador;
- tratamento separado do login universal genérico.

Esse ponto é importante: o **login universal ICP-Brasil** é o objetivo de interoperabilidade; já certificados em nuvem, como os do SerproID, podem exigir **integrações próprias**, embora continuem ancorados na lógica de identidade baseada em certificado. A própria documentação do SerproID mostra ambiente de homologação e endpoints específicos de integração. Verificar se há alguma forma de se fazer um **login universal ICP-Brasil** também para certificados em nuvem, sem a dependência específica de um desenho de autorização por fornecedor.

---

## 8. Perfis de usuários e atores

### 8.1. Usuário final
Pessoa física ou representante pessoa física de pessoa jurídica que realiza autenticação com certificado digital.

### 8.2. Administrador funcional
Responsável por aprovar vínculos, tratar exceções cadastrais e administrar acesso.

### 8.3. Administrador técnico
Responsável por certificados de confiança, parâmetros de validação, integrações, observabilidade e segurança.

### 8.4. Sistema cliente
Aplicação que delega autenticação ao SSO ICP-Brasil.

### 8.5. Auditor/controle
Perfil para consulta de logs, eventos e evidências.

---

## 9. Módulos do sistema

### 9.1. Módulo de autenticação ICP-Brasil
Responsável por receber o certificado, validar o material apresentado e produzir o resultado da autenticação.

### 9.2. Módulo de validação criptográfica
Responsável por:

- chain building;
- trust store;
- verificação de validade;
- revogação;
- parsing de certificado;
- classificação do tipo.

### 9.3. Módulo de identidade e vínculo
Responsável por:

- cadastro de usuários;
- associação de certificados;
- regras de match por CPF/CNPJ;
- histórico de vínculos.

### 9.4. Módulo SSO / federação
Responsável por:

- emissão de sessão;
- tokens de autenticação;
- integração com sistemas clientes;
- single logout;
- consentimento, quando aplicável.

### 9.5. Módulo administrativo
Responsável por:

- gestão de usuários;
- gestão de sistemas clientes;
- configuração de políticas;
- visualização de eventos;
- parametrização de segurança.

### 9.6. Módulo de auditoria
Responsável por:

- coleta de eventos;
- retenção;
- pesquisa;
- exportação controlada;
- trilha de conformidade.

### 9.7. Módulo futuro de assinatura digital
Responsável por fluxos de assinatura, validação e armazenamento de documentos assinados.

---

## 10. Requisitos não funcionais

### 10.1. Segurança
- criptografia em trânsito;
- proteção contra replay, CSRF, JWT e fixation;
- hardening de sessão;
- segregação de ambientes;
- rotação de segredos internos;
- proteção de logs sensíveis.

### 10.2. Disponibilidade
- alta disponibilidade do serviço de autenticação;
- mecanismos de contingência para falhas em validações externas;
- observabilidade e alertas.

### 10.3. Desempenho
- autenticação em tempo aceitável mesmo com validações criptográficas;
- cache controlado de cadeia e revogação, quando juridicamente e tecnicamente cabível.

### 10.4. Auditabilidade
- persistência confiável dos eventos;
- integridade dos logs;
- consulta por usuário, certificado, sistema cliente e período.

### 10.5. Usabilidade
- experiência clara para usuários leigos;
- mensagens objetivas para falhas de certificado, validade, revogação, incompatibilidade e vínculo.

### 10.6. LGPD e governança
- minimização de dados;
- base legal adequada;
- controle de acesso aos logs;
- retenção definida;
- trilha de tratamento de incidentes.

---

## 11. Integração com sistemas clientes

O sistema deverá permitir que aplicações clientes deleguem autenticação ao SSO. Para isso, será necessário definir:

- cadastro do sistema cliente;
- identificador único do cliente;
- URLs de retorno autorizadas;
- escopos e claims internos;
- regras de logout;
- política de expiração de sessão;
- perfil de autorização recebido após autenticação.

Exemplos de uso:

- portal institucional;
- sistemas de RH;
- workflow documental;
- sistemas internos administrativos;
- serviços de autoatendimento.

---

## 12. Modelo de dados mínimo

### 12.1. Tabela `users`
- id
- nome
- cpf
- cnpj, se aplicável
- email
- status
- created_at
- updated_at

### 12.2. Tabela `user_certificates`
- id
- user_id
- serial_number
- subject_dn
- issuer_dn
- cpf_cnpj_extraido
- tipo_certificado
- data_inicio_validade
- data_fim_validade
- fingerprint
- status_vinculo
- created_at
- updated_at

### 12.3. Tabela `auth_sessions`
- id
- user_id
- client_app_id
- session_token
- ip
- user_agent
- autenticado_em
- expira_em
- encerrado_em
- status

### 12.4. Tabela `auth_events`
- id
- user_id
- certificate_id
- client_app_id
- tipo_evento
- resultado
- motivo_falha
- payload_resumido
- ip
- user_agent
- created_at

### 12.5. Tabela `client_apps`
- id
- nome
- identificador
- redirect_uri
- status
- configuracoes
- created_at
- updated_at

### 12.6. Tabela `audit_logs`
- id
- actor_type
- actor_id
- acao
- objeto
- objeto_id
- detalhes
- created_at

---

## 13. Fluxos principais

### 13.1. Fluxo de login ICP-Brasil
1. Usuário acessa aplicação cliente.
2. Aplicação redireciona para o SSO.
3. Usuário escolhe autenticação por certificado digital.
4. Certificado é apresentado ao sistema por canal compatível.
5. Backend valida cadeia, validade, revogação, tipo e atributos.
6. Backend identifica o titular.
7. Backend vincula o titular à conta interna.
8. Sistema cria sessão SSO.
9. Usuário retorna autenticado à aplicação cliente.
10. Evento fica registrado na auditoria.

### 13.2. Fluxo de primeiro vínculo
1. Usuário autentica com certificado válido.
2. Sistema localiza CPF/CNPJ do certificado.
3. Sistema verifica se já existe conta interna correspondente.
4. Se não houver vínculo automático permitido, abre fluxo de cadastro.
5. Após aprovação, o certificado fica associado ao usuário.

### 13.3. Fluxo de falha
1. Certificado apresentado.
2. Validação identifica problema.
3. Sistema informa erro preciso.
4. Evento é auditado.
5. Nenhuma sessão é emitida.

---

## 14. Regras de negócio recomendadas

1. Não autenticar certificado expirado.
2. Não autenticar certificado revogado.
3. Não autenticar cadeia não confiável.
4. Não autenticar certificado incompatível com a finalidade.
5. Permitir múltiplos certificados por usuário, com rastreabilidade.
6. Exigir aprovação para casos excepcionais.
7. Bloquear automaticamente padrões suspeitos de autenticação.

---

## 15. Riscos e desafios do projeto

### 15.1. Variabilidade de ambientes do usuário
Drivers, navegadores, tokens, smartcards e certificados em nuvem têm comportamentos distintos.

### 15.2. Revogação e disponibilidade externa
Dependências de verificação externa podem impactar desempenho e disponibilidade.

### 15.3. Certificados em nuvem
Nem todo provedor remoto se integra da mesma forma; alguns exigem APIs próprias.

### 15.4. Complexidade de UX
Autenticação com certificado costuma ser menos intuitiva para usuários não técnicos.

### 15.5. Governança de identidade
É preciso definir claramente quem pode vincular, desbloquear, aprovar e revogar acessos.

---

## 16. Arquitetura recomendada

### 16.1. Camadas
- **Frontend de autenticação**
- **Backend de identidade**
- **Motor de validação criptográfica**
- **Módulo de sessão/federação**
- **Banco de dados**
- **Camada de auditoria/observabilidade**

### 16.2. Separação importante
Recomenda-se separar:

- autenticação universal ICP-Brasil;
- integrações específicas com prestadores de certificado em nuvem;
- assinatura digital de documentos.

Essa separação reduz acoplamento e facilita evolução.

---

## 18. Fora do escopo inicial

Na primeira entrega, recomenda-se deixar fora do escopo:

- assinatura digital de documentos complexos;
- carimbo do tempo;
- cofre de certificados;
- portal completo de gestão de procurações;
- autenticação internacional ou eID estrangeiro;
- onboarding de parceiros externos com múltiplos arranjos jurídicos;

---

## 19. Entregáveis esperados do projeto

1. Documento de arquitetura.
2. Documento de regras de negócio.
3. Matriz de perfis e permissões.
4. Especificação de integração para sistemas clientes.
5. Modelo de dados.
6. Protótipo de UX do portal de autenticação.
7. Motor de autenticação ICP-Brasil.
8. Painel administrativo.
9. Painel de auditoria.
10. Plano de evolução para gov.br.

---

## 20. Conclusão

É plenamente viável construir um sistema que funcione como **SSO baseado em ICP-Brasil**, desde que a implementação trate corretamente a validação da cadeia até a AC Raiz, a validade temporal, a revogação, os atributos do certificado, o vínculo com a identidade interna e a trilha de auditoria. A base normativa da ICP-Brasil dá sustentação à interoperabilidade entre certificados do mesmo tipo, ou superior, emitidos por quaisquer ACs credenciadas na cadeia.

A recomendação estratégica é começar pelo **login universal ICP-Brasil**, consolidando o núcleo de identidade, sessão, auditoria e integração com sistemas clientes.