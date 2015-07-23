.. include:: ../common/authors.txt

***********************************************
Autenticação Direta a partir de sites parceiros
***********************************************

O recurso de Autenticação Direta permite com que usuários autenticados em sites parceiros acessem links no Admin Braspag sem experimentar um novo processo de autenticação.

.. note::

  Para que a Autenticação Direta funcione corretamente, os usuários do site parceiro precisam estar previamente habilitados no Admin Braspag.

Fluxo de Autenticação Direta:

	.. image:: ../images/acesso-direto-parceiros.png

Passo a passo para implementação:

1. Após a autenticação do usuário no site parceiro, efetue uma chamada para requisição de token de acesso ao serviço Braspag Auth. Informe as credenciais de acesso do cliente (*ClientId* e *ClientSecret*) no cabeçalho Authorization, as credenciais de acesso do usuário autenticado e o escopo para o qual o usuário será redirecionado;

.. code-block:: HTTP

	POST https://{braspag_url}/api/auth/v1/token HTTP/1.1
	Connection: keep-alive
	Authorization: Basic NTY3N0M5RUItNERDUpkaXF4VWZLUmhEN1BBUTVuYUZubFBJclg4SWVDc0hlamM=
	Content-Type: application/x-www-form-urlencoded
	Accept: */*
	Accept-Encoding: gzip, deflate
	Accept-Language: pt,en-US;q=0.8,en;q=0.6

	grant_type=password&username=[Usuario]&password=[Senha]&scope=[Escopo]

2. Efetue uma requisição POST para o endereço de autenticação direta do Admin Braspag informando o token de acesso (*access_token*) retornado no passo 1;

.. code-block:: HTTP

  POST https://{braspag_url}/Account/SingleSignOn HTTP/1.1
  Connection: keep-alive
  Cache-Control: max-age=0
  Content-Type: application/x-www-form-urlencoded
  Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,*/*;q=0.8
  Accept-Encoding: gzip, deflate
  Accept-Language: pt,en-US;q=0.8,en;q=0.6

  AccessToken=[access_token]

3. Se as credenciais do usuário informadas na geração do token forem válidas para o escopo, uma resposta de redirecionamento será retornada com um cookie de autenticação válido para a sessão recém iniciada;

.. code-block:: HTTP

  HTTP/1.1 302 Found
  Cache-Control: no-cache, no-store, must-revalidate
  Pragma: no-cache
  Content-Type: text/html; charset=utf-8
  Expires: -1
  Location: /
  Set-Cookie: .ASPXAUTH=50309C5D71CACFB18A8A0FD6EE409BBC; path=/; secure; HttpOnly
  Strict-Transport-Security: max-age=31536000

  <html><head><title>Object moved</title></head><body>
  <h2>Object moved to <a href="/">here</a>.</h2>
  </body></html>

.. seealso::

  :ref:`Tabela de Escopos disponíveis <scopes-label>`

.. warning::

  Olá Barros!
