---
author: Rafael Miceli
comments: true
layout: default
category : MVC
title: "MVC com Spotify (Melhorias)"
date: 2015-04-27 18:00:00
---

Uma coisa que me incomodou no artigo sobre autenticação com Spotify, era que nosso token ficava visível na url. Com nosso token na URL qualquer um que tiver este token vai poder acessar nossa conta. 

Vamos fazer uma refatoração para que nosso token não fique mais visível em nossa URL. 

A refatoração é fácil. 

Primeiro, nós vamos remover o código que consumimos nossa API da nossa action authresponse. 

Agora vamos salvar nosso access_token em uma session*, então vamos chamar uma (ainda não existente) nova action. 

![passo 1](http://rafael-miceli.com.br/ico/MVC-com-Spotify-Refatoracao/passo1.png)

Segundo, vamos criar a nova action chama playlists. 

Nesta action que vamos consumir a API como antes, mas desta vez vamos pegar o token da nossa session. 

![passo 2](http://rafael-miceli.com.br/ico/MVC-com-Spotify-Refatoracao/passo2.png)

Agora, vamos criar a view da nossa action. 

![passo 3](http://rafael-miceli.com.br/ico/MVC-com-Spotify-Refatoracao/passo3.png)

Para finalizar vamos precisar mudar nossa authresponse view e pronto! 
Eu estou tentando uma refatoração melhor utilizando iframe. Quando terminar compartilho como foi. 

O resultado final é nosso app sem o token na URL. 

![passo 4](http://rafael-miceli.com.br/ico/MVC-com-Spotify-Refatoracao/passo4.png)

###Mais uma coisa 

Se for realizar um deploy de uma aplicação, só esconder o token não é o suficiente, lembre-se de utilizar https! 

###Sobre sessions no MVC 

No AspNet MVC não é comum o uso de sessions, quanto menos user state em nossos servers, mais escalável é nossa aplicação. Mas, eu considero isso uma troca (como geralmente é entre segurança e escalabilidade). 

###Update

Galera, mais uma vez fica o lembrete de quem quiser ver o código na integra o mesmo está no meu [Github](https://github.com/Rafael-Miceli/Blog-Codes/tree/SpotifyApiConsumer)
