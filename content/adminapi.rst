.. include:: ../common/authors.txt

**********
Admin API
**********

Por `Diogo Marins`_ e `Ricardo Abdalla`_

Gerenciamento de Estabelecimento Comercial (EC)
===============================================

A API REST para gerenciamento de Estabelecimento Comercial (EC) oferece operações para criação (POST) e atualização (PUT) de ECs Cielo. As chamadas para a API devem ser precedidas de uma chamada inicial para **Braspag Auth** com o intuíto de obter um token de acesso. Chamadas para a API realizadas sem um token de acesso ou com um token de acesso expirado retornarão o código HTTP ``401 Unauthorized``. Um token de acesso pode e deve ser reutilizado para múltiplas chamadas à API, desde que estejam dentro do seu período de validade.

Os passos a serem efetuados são descritos a seguir:

1. Obtenha junto à Braspag uma credencial de acesso (*ClientId* e *ClientSecret*);
2. Com a credencial obtida, obtenha um token de acesso realizando uma chamada para **Braspag Auth** informando o tipo de permissão *client_credentials* e o escopo *AdminApiEc*;
3. Utilize o token de acesso obtido para efetuar chamadas para as operações de criação ou atualização de EC;

1. Criação de EC
----------------

Cria um EC Cielo (WebService 3, Checkout Completo ou Loja Virtual) no ambiente Braspag.

Formato da requisição POST
^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. code-block:: HTTP

	POST https://{braspag_url}/api/v1/ec HTTP/1.1
	Connection: keep-alive
	Authorization: Bearer 974bd09d6992422183b970cc8343cc9a
	Content-Type: application/json
	Accept: */*
	Accept-Encoding: gzip, deflate
	Accept-Language: pt,en-US;q=0.8,en;q=0.6

	{ }

Contrato JSON para criação de EC
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. note::

	Os comentários no código abaixo são meramente explicativos e não devem ser enviados com o contrato.

.. code-block:: javascript

  {
    /* Dados gerais do EC, obrigatório */
    "GeneralData": {
      "Email": "string", /* Endereço de email, máximo 60 caracteres */
      "DocumentType": 0, /* 0 = Pessoa Jurídica, 1 = Pessoa Física */
      "ContactName": "string", /* Nome do contato, máximo 60 caracteres */
      "Phone": "string" /* Telefone do EC, formato '(99) 99999-9999', máximo 16 caracteres */
    },
    /* Dados da Empresa, obrigatório apenas se Pessoa Jurídica */
    "CompanyData": {
      "FancyName": "string", /* Nome Fantasia, máximo 128 caracteres */
      "CorporateName": "string", /* Razão Social, máximo 128 caracteres */
      "Cnpj": "string" /* Número do CNPJ, formato '99.999.999/9999-99', máximo 16 caracteres */
    },
    /* Dados do Cliente, obrigatório apenas se Pessoa Física */
    "PersonalData": {
      "FullName": "string", /* Nome completo, máximo 128 caracteres */
      "Cpf": "string" /* Número do CPF, formato '999.999.999-99', máximo 14 caracteres */
    },
    /* Endereço Comercial, obrigatório */
    "BusinessAddress": {
      "ZipCode": "string", /* CEP, formato '99999-999', máximo 9 caracteres */
      "Address": "string", /* Logradouro, máximo 100 caracteres */
      "Number": "string", /* Número, máximo 5 caracteres */
      "Complement": "string", /* Complemento, campo opcional, máximo 50 caracteres */
      "District": "string", /* Bairro, máximo 32 caracteres */
      "City": "string", /* Cidade, máximo 32 caracteres */
      "State": "string" /* Estado/UF, máximo 4 caracteres */
    },
    /* Informações transacionais, obrigatório */
    "TransactionalConfiguration": {
      "Ec": "string", /* Número do EC na Cielo, máximo 16 caracteres */
      "ProductionKey": "string", /* Chave de Produção, máximo 64 caracteres */
      "Mcc": "string", /* Código do MCC Cielo, máximo 4 caracteres */
      "PaymentMethods": /* Array de meios de pagamento habilitados */
      [
        {
          "Name": "string", /* Nome do Produto/Bandeira, máximo 16 caracteres */
          "MaxInstallments": "string" /* Número de parcelas (1 a 12) */
        }
      ],
      "CvvRequired": true, /* Obrigatoriedade do código de segurança, formato: true/false  */
      "IntegrationType": 0 /* Tipo de integração do EC */
    }
  }

.. seealso::

	:ref:`Valores válidos para os tipos de integração <integration-types-label>`

.. seealso::

	|swagger_post_ec|

.. |swagger_post_ec| raw:: html

   <a href="https://adminhomolog.braspag.com.br/swagger/ui/index#!/Ec/Ec_Post" target="_blank">Contrato de Criação de EC (Swagger)</a>

Códigos de Retorno HTTP
^^^^^^^^^^^^^^^^^^^^^^^^

===========================   =======================================
Código HTTP                   Resultado
===========================   =======================================
201 Created                   EC criado com sucesso
400 Bad Request               Contrato inválido / Erro de validação
401 Unauthorized              Cliente não autenticado
500 Internal Server Error     Erro interno no servidor
===========================   =======================================


2. Atualização de EC
--------------------

Atualiza as informações de um EC Cielo (WebService 3, Checkout Completo ou Loja Virtual) no ambiente Braspag.

Formato da requisição PUT
^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. code-block:: HTTP

	PUT https://{braspag_url}/api/v1/ec/?EcNumber={ecNumber} HTTP/1.1
	Connection: keep-alive
	Authorization: Bearer 974bd09d6992422183b970cc8343cc9a
	Content-Type: application/json
	Accept: */*
	Accept-Encoding: gzip, deflate
	Accept-Language: pt,en-US;q=0.8,en;q=0.6

	{ }

Contrato JSON para atualização de EC
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. note::

	Os comentários no código abaixo são meramente explicativos e não devem ser enviados com o contrato.

.. code-block:: javascript

  {
    /* Dados gerais do EC, obrigatório */
    "GeneralData": {
      "Email": "string", /* Endereço de email, máximo 60 caracteres */
      "DocumentType": 0, /* 0 = Pessoa Jurídica, 1 = Pessoa Física */
      "ContactName": "string", /* Nome do contato, máximo 60 caracteres */
      "Phone": "string", /* Telefone do EC, formato '(99) 99999-9999', máximo 16 caracteres */
      "IsBlocked": true /* Indica se EC está bloqueado para transações, formato: true/false */
    },
    /* Dados da Empresa, obrigatório apenas se Pessoa Jurídica */
    "CompanyData": {
      "FancyName": "string", /* Nome Fantasia, máximo 128 caracteres */
      "CorporateName": "string", /* Razão Social, máximo 128 caracteres */
      "Cnpj": "string" /* Número do CNPJ, formato '99.999.999/9999-99', máximo 16 caracteres */
    },
    /* Dados do Cliente, obrigatório apenas se Pessoa Física */
    "PersonalData": {
      "FullName": "string", /* Nome completo, máximo 128 caracteres */
      "Cpf": "string" /* Número do CPF, formato '999.999.999-99', máximo 14 caracteres */
    },
    /* Endereço Comercial, obrigatório */
    "BusinessAddress": {
      "ZipCode": "string", /* CEP, formato '99999-999', máximo 9 caracteres */
      "Address": "string", /* Logradouro, máximo 100 caracteres */
      "Number": "string", /* Número, máximo 5 caracteres */
      "Complement": "string", /* Complemento, campo opcional, máximo 50 caracteres */
      "District": "string", /* Bairro, máximo 32 caracteres */
      "City": "string", /* Cidade, máximo 32 caracteres */
      "State": "string" /* Estado/UF, máximo 4 caracteres */
    },
    /* Informações transacionais, obrigatório */
    "TransactionalConfiguration": {
      "ProductionKey": "string", /* Chave de Produção, máximo 64 caracteres */
      "Mcc": "string", /* Código do MCC Cielo, máximo 4 caracteres */
      "PaymentMethods": /* Array de meios de pagamento habilitados */
      [
        {
          "Name": "string", /* Nome do Produto e Bandeira, máximo 16 caracteres */
          "MaxInstallments": "string" /* Número de parcelas (1 a 12) */
        }
      ],
      "CvvRequired": true, /* Obrigatoriedade do código de segurança, formato: true/false  */
      "IntegrationType": 0 /* Tipo de integração do EC */
    }
  }

.. seealso::

	:ref:`Valores válidos para os tipos de integração <integration-types-label>`

.. seealso::

	|swagger_put_ec|

.. |swagger_put_ec| raw:: html

   <a href="https://adminhomolog.braspag.com.br/swagger/ui/index#!/Ec/Ec_Put" target="_blank">Contrato de Atualização de EC (Swagger)</a>

Códigos de Retorno HTTP
^^^^^^^^^^^^^^^^^^^^^^^^

===========================   =======================================
Código HTTP                   Resultado
===========================   =======================================
200 OK                        EC atualizado com sucesso
400 Bad Request               Contrato inválido / Erro de validação
401 Unauthorized              Cliente não autenticado
500 Internal Server Error     Erro interno no servidor
===========================   =======================================


.. _integration-types-label:

Valores válidos para os tipos de integração
-------------------------------------------

========   =======================================
Código     IntegrationType
========   =======================================
2          Checkout Simplificado
3          Checkout Completo
5          Loja Virtual Completa
6          Webservice 3.0
26         Checkout Simplificado e WebService 3.0
36         Checkout Completo e WebService 3.0
56         Loja Virtual Completa e WebService 3.0
========   =======================================
