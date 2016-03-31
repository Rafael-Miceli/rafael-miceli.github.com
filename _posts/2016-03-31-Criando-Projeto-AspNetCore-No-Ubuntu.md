---
author: Rafael Miceli
comments: true
layout: default
category : aspnetcore
title: "Cirando um projeto Asp Net Core no Ubuntu"
date: 2016-03-31 20:00:00
---

Uma vez com o DNVM e o DNX instalados, podemos então criar um projeto aspnet. 
 
Mas, não temos Visual Studio aqui, vamos ter que fazer tudo na unha? 
 
Bem... Não. 
 
### Yeoman 
 
*Yo*, vamos criar nosso projeto com Yeoman. 
 
Yeoman é um gerador de templates de projetos, ou seja, caso você queria criar um projeto a partir de algum template usamos o Yo.

Mas antes de instalarmos o Yeoman, precisamos ter o nodejs. 

Para instalar o node é simples. No terminal executamos primeiro:
	
    sudo apt-get install nodejs
    
E depois:
	
    sudo apt-get install npm
 
 
Agora vamos instalar o Yeoman. 
 
Ainda no terminal, vamos executar o código: 
 
    sudo npm install -g yo
 
![yo instalado](http://rafael-miceli.com.br/ico/Criando-Projeto-AspNetCore-No-Ubuntu/yo-instalado.png) 
 
Excelente! temos nosso Yeoman instalado. 
 
Agora precisamos instalar o template para o aspnet. 
 
Para fazermos isso executamos o código: 
 
    sudo npm install -g generator-aspnet
 
Prefeito! Tudo pronto, agora vamos criar nosso projeto 
 
### Criando o projeto 
 
Para criar o projeto executamos: 
 
Yo aspnet 
 
![yo](http://rafael-miceli.com.br/ico/Criando-Projeto-AspNetCore-No-Ubuntu/yo.png) 
 
Podemos agora escolher qual template queremos. Explicando basicamente os templates que podemos criar.

A opção:

1 - Cria uma aplicação web com o wwwroot, startup.cs e project.json, nada de controllers ou views

2 - Nos cria um console com um hello world apenas. 
 
3 - Vai criar uma aplicação web similar como o .net cria no visual studio, com home, about e contact + feature de autenticação com identity. Persistindo os dados no enyityframework 7. 
 
4 - É um projeto web similar ao anterior, mas sem autorização e autenticação.  
 
5 - Um projeto web api com os controllers retornado resposta http ao invés de paginas. Também sem autenticação.
 
6 - Este template cria um projeto usando o framework Nancy.     
 
7 - Uma class library com apenas uma classe  
 
6 - Um projeto de Teste usando xunit com dois testes, um passando e outro falhando 
 
 
Vamos escolher o template 3, e aguardamos nosso projeto ser criado. 
 
Uma vez pronto, vamos usar o [visual studio code](https://code.visualstudio.com/) para explorar o mesmo: 
  
![application](http://rafael-miceli.com.br/ico/Criando-Projeto-AspNetCore-No-Ubuntu/aspnet-core-ubuntu.png)
 
Vamos explorar o projeto que criamos em um próximo artigo. 
 
 
 
  