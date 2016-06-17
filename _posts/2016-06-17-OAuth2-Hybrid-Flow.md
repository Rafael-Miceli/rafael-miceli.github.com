---
author: Rafael Miceli
comments: true
layout: default 
category : Oauth
title: "OAuth2 - Hybrid Flow" 
date: 2016-06-17 09:00:00
---

Neste post vamos pegar o que fizemos em nosso [post anterior](http://rafael-miceli.com.br/oauth/2016/06/16/OAuth2-Authorization-Code-Grant-startup-cs.html), que foi configurar nosso Authorization Code Grant e transforma-lo em um Hybrid.

### Diferença Authorization Code Grant e Hybrid

A maior diferença é que se seu Client só pode realizar request do __Code__ para autorização, o que garante que você pode ter esse __Code__ é seu __Client Id__ e __Client Secret__.

Usando o Hybrid flow nós podemos fazer request além do __Code__, também do __Id Token__.

### Implementando Hybrid

Para fazermos este nosso Client como Hybrid vamos em nosso InMemoryClients no nosso ClientId **teste_code** e vamos mudar seu flow para Hybrid.

Próximo passo, vamos no Startup.cs do nosso Client que é __Authorization Code__ e vamos na propriedade _ResponseType_ do __OpenIdConnectAuthenticationOptions__ e adicionamos o **id_token**. 

E Pronto!

Com isso, temos agora dois _ResponseTypes_ vindo. O Code, e o Id_Token.

Temos nosso Hybrid Flow!
