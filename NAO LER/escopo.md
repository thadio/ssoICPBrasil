Quero fazer um sistema que funcione como um SSO baseado no ICP-Brasil ou govBr (govbr é o segundo passo, vamos começar pelo ICP-Brasil).

Apresente o escopo completo do sistema.

Aceitar “qualquer certificado ICP-Brasil” no seu sistema
Isso é possível se você mesmo implementar a autenticação por certificado, validando:

a cadeia até a AC Raiz da ICP-Brasil;
a validade do certificado;
revogação/status;
e o vínculo do certificado com o usuário do seu sistema.
A própria documentação da ICP-Brasil diz que aplicações que admitam certificado de um tipo contemplado pela ICP-Brasil devem aceitar qualquer certificado do mesmo tipo, ou superior, emitido por qualquer AC credenciada pela AC Raiz.

Login universal ICP-Brasil
Seu portal aceita certificado cliente do navegador/token/nuvem do fornecedor e faz a validação da cadeia ICP-Brasil. Nesse modelo, o usuário pode usar certificado emitido por Serpro, Valid, Soluti, Certisign, OAB/CRM/CREA via ICP-Brasil etc., desde que seja um certificado ICP-Brasil válido. O próprio material do Assinador Serpro afirma que, se o certificado foi emitido pela ICP-Brasil, ele pode ser usado em qualquer sistema que exija assinatura digital, independentemente da autoridade emissora.

Se você quiser aceitar qualquer certificado ICP-Brasil, seu backend precisa verificar pelo menos:

cadeia até a AC Raiz ICP-Brasil;
período de validade;
revogação;
OID/campos de identificação do titular;
CPF/CNPJ do certificado contra a conta interna do sistema;
logs de autenticação e auditoria.

Além disso, para assinatura de documentos, você ainda precisa decidir se o sistema vai:

apenas usar o certificado para login; ou
também usar para assinatura digital de documentos.

Essas duas coisas são relacionadas, mas não são idênticas.