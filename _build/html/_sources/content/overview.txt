.. include:: ../common/authors.txt

Visão Geral
===========

As APIs REST fornecidas pelo Admin 3.x permitem executar, de forma programática,
tarefas administrativas que, até então, eram executadas somente pela interface de usuário.

Algumas destas tarefas são listadas abaixo:

* Cadastro de Estabelecimentos Comerciais WebService/Checkout
* Acesso aos dados de lojas do Pagador (*em breve*)
* Cadastro de Usuários (*em breve*)

**Admin API** é composta pelas funcionalidades de negócio descritas acima.

**Braspag Auth** é a API que implementa o Serviço de Autorização necessário para garantir a segurança no consumo da Admin API, através da criação e validação de tokens de acesso com escopo e
duração limitados.

Tokens de acesso precisam ser obtidos para garantir acesso as APIs de negócio e também para utilização de recursos de *Single Sign-On* entre aplicativos e sites dentro do domínio Braspag / Cielo.
