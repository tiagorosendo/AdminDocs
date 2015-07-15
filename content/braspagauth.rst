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

Tipos de Permissão (Grant Types)
================================
	
1. Client Credentials
---------------------

No fluxo de permissão ``Client Credentials``, o cliente requisita um token de acesso para um recurso protegido usando apenas as suas credenciais. Um escopo de validade para o token de acesso deve ser informado. Se as credenciais forem válidas, o token de acesso é emitido e o cliente pode então utilizá-lo para acessar o recurso protegido (por exemplo, alguma das APIs Braspag). 

Fluxo Client Credentials:

	.. image:: ../images/client-credentials.png

Formato da requisição POST:

.. code-block:: HTTP

	POST https://admin.braspag.com.br/api/auth/v1/token HTTP/1.1
	Host: admin.braspag.com.br
	Connection: keep-alive
	Authorization: Basic NTY3N0M5RUItNERDQS00OTk3LTk1MUYtM0QxNzk3QzhGQTcwOlpJTVo0UlZVRUFVTk5FNlZHNGxuZTZESnJZWUZJdUpkaXF4VWZLUmhEN1BBUTVuYUZubFBJclg4SWVDc0hlamM=
	Content-Type: application/x-www-form-urlencoded
	Accept: */*
	Accept-Encoding: gzip, deflate
	Accept-Language: pt,en-US;q=0.8,en;q=0.6

	grant_type=client_credentials&scope=[Escopo]

2. Resource Owner Password Credentials
--------------------------------------

Texto.
