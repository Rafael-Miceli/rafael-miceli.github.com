---
author: Rafael Miceli
comments: true
layout: default 
category : Oauth
title: "Identity server - loop infinito redirecionamento" 
date: 2016-07-20 10:00:00
---

Um dos problemas mais comuns que vejo (e já tive) ao configurar Oauth identity server é um redirecionamento infinito em loop ao tentar efetuar seu login usando os flows _Implicit_ ou _AuthorizationCode_ ou _Hybrid_.

Este post vai sendo atualizado com o tempo, já que existem diversos fatores que podem influenciar seu loop infinito e não tenho como descrever todos aqui. 

Já existe muita documentação[¹]() do time do IdentityServer em cima disso. Neste post vou explicar de uma forma mais simples as causas.

### Causas para e esse problema

Um dos motivos para esse problema acontecer é seu Authorization Server estar gerando o cookie em HTTP e seu Client aceitar apenas Cookies vindo de HTTPS. 

Demonstrando um exemplo de como isso ocorre:

Repare no startup.cs que o __CookieAuthenticationOptions__ só aceita cookies em HTTPS! 

```csharp
app.UseCookieAuthentication(new CookieAuthenticationOptions
    {  
        AuthenticationType = "Cookies", 
        CookieSecure = CookieSecureOption.Never
    });
```

Isso por causa da opção _CookieSecure = CookieSecureOption.Never_. 

Se seu Authorization Server estiver em HTTP, logo ele está emitindo o token no protocolo HTTP. Com isso seu Client nunca vai conseguir armazenar o cookie uma vez que o token vem do protocolo HTTP.

### Resolver esse problema

Para resolver, a __MELHOR__ forma é colocar seu Authorization Server em HTTPS.

É extremamente inseguro que seu Authorization Server não esteja em HTTPS, pois você pode acabar sofrendo um ataque [Man in the Middle](https://en.wikipedia.org/wiki/Man-in-the-middle_attack)

Para colocar em HTTPS você precisar de um certificado emitido por uma [CA(Certificate Authority)](https://en.wikipedia.org/wiki/Certificate_authority) e isso fica para um outro post, pois merece um tópico próprio, já que não é algo tão trivial.

A maneira não recomendada para resolver isso, é apenas remover a opção _CookieSecure_, que ai, você consegue receber seus tokens em HTTP, __(MAS NÃO FAÇA ISSO EM PRODUÇÃO)!!!__

### Finalizando

Como disse no começo este post vai ser atualizado com o tempo, com isso aguardem por atualizações neste cara! 

### Referências

[¹] documentação de issues referente a loops infinitios: 

https://github.com/IdentityModel/Thinktecture.IdentityModel/issues/56

https://github.com/IdentityServer/IdentityServer3/issues/294

https://github.com/IdentityServer/IdentityServer3/issues/1656