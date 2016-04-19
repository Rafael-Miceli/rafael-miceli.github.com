---
author: Rafael Miceli
comments: true
layout: default 
category : OAuth 
title: "OAuth2 - Redirect Flows" 
date: 2016-04-19 8:50:00
---

Quando vamos usar OAuth2 como protocolo para autorização da nossa aplicação, uma das coisas que precisamos definir é, qual _Flow_ ou _Grant_ usar para obter nosso _Token_ (__Access Token__).

### Redirect Flows

Usando um _Redirect Flow_ com Oauth, a autorização realizaria o seguinte fluxo:

1. Você realiza a chamada a área "restrita" do site que você quer acessar (_Client_) 
2. Você é redirecionado para outro site (_Authorization Server_) com uma tela de login. 
3. Você insere seu login e senha e...(__essa parte varia um pouco__) é redirecionado para a url da área restrita (_Client_) com o _access token_.
4. A área restrita que você acessou busca os dados na API (_Resource Server_), passando o _access token_ no Header do request.
5. A API valida o _access token_, e caso o token seja válido, retorna o conteúdo pedido.

![flow](http://rafael-miceli.com.br/ico/OAuth2-Redirect-Flows/flow.png)

### Implicit Grant

O fluxo explicado acima se diz respeito ao uso do __Implicit Grant__, apesar que pode estar faltando um passo, dependendo da configuração em seu Authorization Server.

No Implicit Grant, após você inserir seu login e senha, você recebe seu _access token_ no response como um [_Hash Fragment_](https://en.wikipedia.org/wiki/Fragment_identifier) na url.

### Authorization Code Grant

O Authorization Code Grant consiste de um fluxo muito similar ao implicit, mas a grande diferença é que no Authorization Code Grant ao invés de você receber seu _access token_ no response do seu login, você recebe um __Authorization Code__. 

Após isso você realiza mais um request ao authorization server pedindo seu _access token_, passando seu recém recebido __Authorization Code__.

#### Porque essa diferença do Implicit grant para o Authorization Code Grant?

> Por motivos de segurança. Pois o Implicit Grant passa seu _access token_ como um _Hash Fragment_, enquanto o Auhtorization Code Grant trabalha enviando um _Authorization Code_ como parâmetro da sua URL, para que após, seu Authorization Server receba esse _Authorization Code_ + seu _Client Secret_ (em um próximo artigo) para validar ambos e te retornar um _access token_. Outra diferença em questão de segurança também é que o Implicit não recebe _refresh tokens_ enquanto o Authorizaton Code, recebe.

Em um próximo post implementamos um Redirect Flow.
