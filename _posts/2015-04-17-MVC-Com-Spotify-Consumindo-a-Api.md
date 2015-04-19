---
author: Rafael Miceli
comments: true
layout: default
category : MVC
title: "MVC com Spotify (Consumindo a API)"
date: 2015-04-17 10:00:00
---


No último artigo sobre consumir a API do Spotify nós nos autenticamos na mesma, agora vamos exibir os nomes de nossas playlists. 
Em nosso método Get "AuthResponse", aonde nós vamos receber nosso Token, vamos chamar (O não existente ainda) método GetUserProfile da classe SpotifyService. 

![passo 1](http://rafael-miceli.com.br/ico/MVC-com-Spotify-Consumindo-a-Api/passo1.png)

Como esta classe não existe, vasos cria-la. 

![passo 2](http://rafael-miceli.com.br/ico/MVC-com-Spotify-Consumindo-a-Api/passo2.png)

Nesta classe vamos ter um método genérico que recebe nosso token e a URL da API do Spotify. 
Dentro do método fazemos um Request usando *WebRequest*, colocando o token no header. 
Nós lemos o response e damos um parse do Json para classes POCO que representam nossos objetos. 

![passo 3](http://rafael-miceli.com.br/ico/MVC-com-Spotify-Consumindo-a-Api/passo3.png)

Como ainda não criamos nenhum POCO vamos criar o do nosso usuário e vamos implementar os atributos "JsonProperty" em cada propriedade. 

![passo 4](http://rafael-miceli.com.br/ico/MVC-com-Spotify-Consumindo-a-Api/passo4.png)

Agora, nos podemos consumir nosso UserProfile, vamos testar isso. 

![passo 5](http://rafael-miceli.com.br/ico/MVC-com-Spotify-Consumindo-a-Api/passo5.png)

Mas lembrem-se que por causa do nosso azeite vamos passar duas vezes por nosso GET, sendo que um sem receber o *access_token*, para isso vamos colocar uma verificação antes.
Vamos resolver isto no próximo artigo.

![passo 6](http://rafael-miceli.com.br/ico/MVC-com-Spotify-Consumindo-a-Api/passo6.png)

Ótimo! Agora que tal consumirmos nossas playlists e exibi-las em nossa view? 
Crie um método para consumirmos nossas playlists, para isto nos vamos precisar do nosso username que recebemos consumindo nosso UserProfile. 

![passo 7](http://rafael-miceli.com.br/ico/MVC-com-Spotify-Consumindo-a-Api/passo7.png)

![passo 8](http://rafael-miceli.com.br/ico/MVC-com-Spotify-Consumindo-a-Api/passo8.png)

Como fizemos anteriormente vamos também criar nosso POCO 

![passo 9](http://rafael-miceli.com.br/ico/MVC-com-Spotify-Consumindo-a-Api/passo9.png)

No controller vamos criar uma propriedade em nosso ViewBag para exibir os nomes de nossas playlists. 

![passo 10](http://rafael-miceli.com.br/ico/MVC-com-Spotify-Consumindo-a-Api/passo10.png)

![passo 11](http://rafael-miceli.com.br/ico/MVC-com-Spotify-Consumindo-a-Api/passo11.png)

E é isso mesmo! Simples assim. 
Se quiserem explorar mais como consumir a API do Spotify dêem uma olhada na [documentação deles](https://developer.spotify.com/web-api/user-guide/).  

Para .Net eles recomendam dois projetos de exemplo: 

https://spotifywebapi.codeplex.com/

https://github.com/JohnnyCrazy/SpotifyAPI-NET 

###Resumindo 

Explorar a API do Spotify é muito legal, e bem fácil. 
Lembrando que os códigos deste artigo estão em meu [github](https://github.com/Rafael-Miceli/Blog-Codes/tree/master/IdeiasComAzeite/SpotifyApiConsumer/SpotifyConsumer).  
