---
author: Rafael Miceli
comments: true
layout: default 
category : Oauth
title: "Algumas Informações sobre IndentityServer" 
date: 2016-07-18 17:40:00
---

Neste post venho compartilhar algumas informações que descobri e que algumas pessoas ja me perguntaram sobre o identityServer ou OAuth.

### O IdentityServer é a unica opção de framework?

__R:__ Não, além do [Identity Server](https://github.com/IdentityServer/IdentityServer3) que já mostrei bastante [aqui](http://rafael-miceli.com.br/categories.html#OAuth-ref), também temos outros como:

- [DotNetOpenAuth](http://dotnetopenauth.net/)
- [Madgex](https://code.google.com/archive/p/oauth-dot-net/)
- [AspNet Identity](http://www.asp.net/identity), _(Com um excelente passo a passo do [Taiseer Joudeh](http://bitoftech.net/) que está [neste link](http://bitoftech.net/2014/06/01/token-based-authentication-asp-net-web-api-2-owin-asp-net-identity/))_  
- [OAuthServer](https://oauthserver.codeplex.com/)

Mas além de o IdentityServer ser completo, [o time Asp.Net também pediu para que o IdentityServer seja o susbstituto do Middleware katana](https://leastprivilege.com/2016/01/11/announcing-identityserver-for-asp-net-5-and-net-core/).

### O IdentityServer Suporta MultiTenacy?

__R:__ Infelizmente ainda não, mas é uma funcionalidade que esta para vir de acordo com o time no [Github](https://github.com/IdentityServer/IdentityServer3/issues/39).

### Como faço o reconhecimento dos usuários? Com Claims?

__R:__ As Claims são usadas para possuir informações extras de quem possui aquele token, geralmente o reconhecimento é feito pelo Claim: [sub](https://self-issued.info/docs/draft-ietf-oauth-json-web-token.html#rfc.section.4.1.2), que por convenção vai possuir o identificador do portador do token.

Por exemplo, se você realizou login como um _cliente_ a Claim __sub__ vai possuir o id daquele "cliente".

Para maior segurança sempre recomendo ids de entidades sensiveis (como usuários, contratos e outros que depende do projeto) sejam [GUIDs(Globally Unique Identifier)](https://en.wikipedia.org/wiki/Globally_unique_identifier).       
