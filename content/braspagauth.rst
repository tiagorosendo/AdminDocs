.. include:: ../common/authors.txt

************
Braspag Auth
************

Por `Daniel Braga`_ e `Ricardo Abdalla`_

Introdução
==========

**Braspag Auth** é o Serviço de Autorização da Braspag, implementado de acordo com as especificações do protocolo OAuth2. O objetivo deste serviço é prover fluxos de autorização
específicos para aplicações web, aplicações desktop, dispositivos móveis, entre outros.

A especificação do OAuth2 é definida pelo `RFC 6749 <http://tools.ietf.org/html/rfc6749>`_.

.. note::

	A versão 1.0 do Braspag Auth ainda não implementa os fluxos *Authorization Code* e *Implicit* especificados no OAuth2.

Obtenção de Autorização
=======================

Para obter autorização de acesso aos recursos protegidos pelo Braspag Auth, é preciso informar na requisição HTTP o tipo de permissão desejada, além das credenciais de cliente. O servidor de autorização disponibiliza credenciais para seus clientes registrados.

As credenciais de cliente são compostas por duas informações, *ClientId* e *ClientSecret*, únicas para cada cliente.

Uma vez obtidos, *ClientId* e *ClientSecret* devem ser enviados como uma string única, codificada no formato Base64, no cabeçalho Authorization da requisição HTTP, conforme descrito abaixo:

.. code-block:: HTTP

    Authorization: Basic czZCaGRSa3F0Mzo3RmpmcDBaQnIxS3REUmJuZlZkbUl

O processo de codificação em Base64 deve ser realizado sobre o texto formatado de acordo com o seguinte padrão:

.. code-block:: HTTP

	[ClientId]:[ClientSecret]

.. warning::

	*ClientId* e *ClientSecret* são informações sensíveis que compõem uma credencial única por cliente, não devendo, portanto, serem fornecidas a terceiros.

Tipos de Permissão (Grant Types)
================================

1. Client Credentials
---------------------

No fluxo de permissão ``Client Credentials``, o cliente requisita um token de acesso para um recurso protegido usando apenas as suas credenciais. Um escopo de validade para o token de acesso deve ser informado. Se as credenciais forem válidas, o token de acesso é emitido e o cliente pode então utilizá-lo para acessar o recurso protegido (por exemplo, alguma das APIs Braspag).

Fluxo Client Credentials:

	.. image:: ../images/client-credentials.png

Formato da requisição POST:

.. code-block:: HTTP

	POST https://{braspag_url}/api/auth/v1/token HTTP/1.1
	Connection: keep-alive
	Authorization: Basic NTY3N0M5RUItNERDUpkaXF4VWZLUmhEN1BBUTVuYUZubFBJclg4SWVDc0hlamM=
	Content-Type: application/x-www-form-urlencoded
	Accept: */*
	Accept-Encoding: gzip, deflate
	Accept-Language: pt,en-US;q=0.8,en;q=0.6

	grant_type=client_credentials&scope=[Escopo]

Formato da resposta:

.. code-block:: HTTP

	HTTP/1.1 200 OK
	Content-Type: application/json; charset=utf-8
	Connection: close

	{
	"access_token":"99997e6d97fe4a99933dd7b1b88dee3f",
	"token_type":"Bearer",
	"expires_in":3600,
	"scope":"[Escopo]"
	}

2. Resource Owner Password Credentials
--------------------------------------

No fluxo de permissão ``Resource Owner Password Credentials``, além das credenciais do cliente (*ClientId* e *ClientSecret*), também são informados as credenciais de um usuário (nome de usuário e senha), que são trocadas por um token de acesso e um token de atualização (*refresh_token*). O acesso realizado com o uso do token será feito em nome do usuário cujas credenciais foram previamente informadas.

Por que a senha do proprietário do recurso é exposta à aplicação, este fluxo deve ser utilizado apenas em situações onde há um grande grau de confiança entre a aplicação e o provedor da API de autorização. Em outras palavras, seu uso deve ser restrito apenas à aplicações no domínio Braspag.

Fluxo Resource Owner Password Credentials:

  	.. image:: ../images/resource-owner.png

Formato da requisição POST:

.. code-block:: HTTP

	POST https://{braspag_url}/api/auth/v1/token HTTP/1.1
	Connection: keep-alive
	Authorization: Basic NTY3N0M5RUItNERDUpkaXF4VWZLUmhEN1BBUTVuYUZubFBJclg4SWVDc0hlamM=
	Content-Type: application/x-www-form-urlencoded
	Accept: */*
	Accept-Encoding: gzip, deflate
	Accept-Language: pt,en-US;q=0.8,en;q=0.6

	grant_type=password&username=[Usuario]&password=[Senha]&scope=[Escopo]

Formato da resposta:

.. code-block:: HTTP

	HTTP/1.1 200 OK
	Content-Type: application/json; charset=utf-8
	Connection: close

	{
	"access_token":"99997e6d97fe4a99933dd7b1b88dee3f",
	"token_type":"Bearer",
	"expires_in":3600,
	"refresh_token":"9999886d97999999999dd7b1b88d8888",
	"scope":"[Escopo]"
	}

.. _scopes-label:

Tipos de Escopo
================

Os escopos definem o contexto de validade de um token de acesso. Escopos são *case-sensitive* e devem ser informados exatamente como escritos abaixo.

==============   ====================================================
Escopo           Descrição
==============   ====================================================
AdminCielo       Single Sign-On de clientes do site Cielo para Admin
AdminV2          Single Sign-On para Admin versão 2.x
AdminV3          Single Sign-On para Admin versão 3.x
AdminApiEc       API para gerenciamento de ECs Cielo
==============   ====================================================

Validação de Token
==================

Para aplicativos que precisam validar se um determinado token de acesso existe, não está expirado e possui o escopo
adequado, **Braspag Auth** oferece a operação ``Validate``. A operação deve ser chamada através de uma requisição GET,
informando *{token_a_ser_validado}* como parâmetro na URL. A requisição deve possuir ainda *{token_de_acesso_valido}* no cabeçalho Authorization, conforme exemplificado abaixo:

Formato da requisição GET:

.. code-block:: HTTP

	GET https://{braspag_url}/api/auth/v1/token/{token_a_ser_validado} HTTP/1.1
	Connection: keep-alive
	Authorization: Bearer {token_de_acesso_valido}
	Content-Type: application/x-www-form-urlencoded
	Accept: */*
	Accept-Encoding: gzip, deflate
	Accept-Language: pt,en-US;q=0.8,en;q=0.6

Formato da resposta:

.. code-block:: HTTP

	HTTP/1.1 200 OK
	Content-Type: application/json; charset=utf-8
	Connection: close

	{
	"isValid":true,
	"expires_in":"2015-07-20T21:27:29",
	"grant_type":"client_credentials",
	"scope":"[Escopo]"
	}

.. note::

	Tokens gerados a partir do fluxo *Resource Owner Password Credentials* também retornam o atributo *username*, contendo o nome do usuário associado, se forem válidos.
