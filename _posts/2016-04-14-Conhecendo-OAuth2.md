---
author: Rafael Miceli
comments: true
layout: default 
category : OAuth 
title: "Conhecendo OAuth2" 
date: 2016-04-14 21:00:00
---

Muitas aplicações que fazemos em algum ponto vai necessitar algum tipo de autorização para que um usuário consiga acessar o conteúdo de alguma página, ou de alguma API.

_OAuth2_ é um excelente protocolo e atualmente uma das maneiras mais versáteis para realizar esta tarefa.

### Quando usar o OAuth2

Ok, preciso usar _Oauth_ em minha aplicação? 

Depende.

Sua aplicação vai dar suporte a varias plataformas para acesso? Por exemplo: pretende criar uma aplicação web e um aplicativo mobile para acessar uma API? Então _OAuth2_ é uma excelente pedida.

Vai criar uma aplicação mais simples client-server intranet, ou até uma aplicação web bastante simples? 
Então é bom avaliar outras opções e se _Oauth_ é sua pedida, uma vez que tem uma curva de aprendizagem para seu uso e a implementação do mesmo não é _TÃO_ trivial.

### Porque usar OAuth2?

No passado, para aplicações tradicionais na Web, poderíamos usar _SAML_ ou _WS*_ para autorização, mas para o cenário moderno a coisa muda de figura. 

Para aplicativos de smartphones ou tablets _OAuth2_ é a melhor maneira de se trabalhar, uma vez que smartphones ou tabletes não possuem bibliotecas de XML complexas para o processamento dos antigos protocolos.

Além disso, por _OAuth2_ trabalhar com __JWT__ (_Json Web Tokens_, melhor explicado em um post futuro), isso torna a troca de mensagens muito mais leve e muito mais fácil de decodificar. Afinal praticamente qualquer plataforma trabalha com Javascript, além de ser a linguagem de front favorita na web.

### Funcionamento

A ideia é a seguinte:

Imagina que você possui um sistema web para gerenciar os alunos de uma escola (Tem um bem simples assim em meu [Github](https://github.com/Rafael-Miceli/Blog-Codes/tree/master/IdeiasComAzeite/AutoFixtureSample/Escola)).
Esse sistema que gerencia nossos alunos chamamos de __Resource Server__.

Em uma autorização tradicional, o mesmo sistema de Escola seria responsável por autenticar e autorizar os usuários, provavelmente criando um cookie para o mesmo.

Usando _Oauth_ vamos ter uma nova aplicação, que vamos chamar de __Authorization Server__. Nosso __Authorization Server__ é o responsável por gerar a chave de permissão para o nosso __Resource Server__.E nós temos ainda mais dois players no _OAuth_ que são o __Client__ e o __Resource Owner__.

O __Client__ sempre foi nos dito que é quem está usando nossa aplicação, ou o cliente da aplicação, mas no contexto do _OAuth_, o __Client__ é o software que acessa nosso __Resource Server__.

Para ficar fácil, imagina um aplicativo de celular, ele vai precisar acessar a API de nossa aplicação Escola (__Resource Server__) para consumir os dados e exibir ao nosso usuário que é o __Resource Owner__

Mas e se meu __Resource Server__ também tem o papel de __Client__, por exemplo: Tenho uma aplicação MVC e WebApi no mesmo project? Sem problemas, só veremos mais tarde qual melhor _flow_ usar para sua aplicação.

Mas para facilitar um pouco o que falei acima, uma imagem sempre facilita:

![roles](http://rafael-miceli.com.br/ico/Conhecendo-OAuth2/roles.png)

No próximo artigo vamos ver alguns _flows_ que o _OAuth_ possui e qual usar em determinadas situações.


