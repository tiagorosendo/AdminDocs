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

Formato da requisição POST:
^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. code-block:: HTTP

	POST https://{braspag_url}/api/v1/ec HTTP/1.1
	Connection: keep-alive
	Authorization: Bearer 974bd09d6992422183b970cc8343cc9a
	Content-Type: application/json
	Accept: */*
	Accept-Encoding: gzip, deflate
	Accept-Language: pt,en-US;q=0.8,en;q=0.6

	{ contrato_json }

Contrato JSON para criação de EC:
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. note::

	Os comentários no código abaixo são meramente explicativos e não devem ser enviados com o contrato

.. code-block:: javascript

  {
    /* Dados gerais do EC, obrigatório */
    "GeneralData": {
      "Email": "string",
      "DocumentType": 0, /* 0 = Pessoa Jurídica, 1 = Pessoa Física */
      "ContactName": "string",
      "Phone": "string"
    },
    /* Obrigatório apenas se Pessoa Jurídica */
    "CompanyData": {
      "FancyName": "string",
      "CorporateName": "string",
      "Cnpj": "string"
    },
    /* Obrigatório apenas se Pessoa Física */
    "PersonalData": {
      "FullName": "string",
      "Cpf": "string"
    },
    /* Endereço Comercial, obrigatório */
    "BusinessAddress": {
      "ZipCode": "string",
      "Address": "string",
      "Number": "string",
      "Complement": "string",
      "District": "string",
      "City": "string",
      "State": "string"
    },
    /* Informações transacionais, obrigatório */
    "TransactionalConfiguration": {
      "Ec": "string",
      "ProductionKey": "string",
      "Mcc": "string",
      "PaymentMethods": [
        {
          "Name": "string",
          "MaxInstallments": "string"
        }
      ],
      "CvvRequired": true,
      "IntegrationType": 0
    }
  }

2. Atualização de EC
--------------------


Atualização de Estabelecimento Comercial
========================================
