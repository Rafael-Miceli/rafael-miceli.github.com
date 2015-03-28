---
author: Rafael Miceli
comments: true
layout: default
category : TDD
title: "TDD com Android(RED)"
date: 2015-03-28 18:00:00
---


Neste artigo vou compartilhar a experiência que tive em criar uma aplicação Android usando TDD. 
Agora o Android Studio traz suporte a testes unitários e isto facilitou muito a criação de testes com Junit. 
Vou escrever uma feature de login usando o azure como back end, mas tudo vai ser escrito usando test first. 
Mas antes, como um programa de receitas, vou mostrar as ferramentas que vamos precisar. 

###O que vamos precisar

Vamos usar o Android Studio 1.1 e vamos integrar o app com azure. O foco não é o azure, nosso foco não é em implementar a login feature teste first.

###Preparando o ambiente
 
Criem um app no Android Studio, um blank template mesmo.

Após o app template ter sido criado, vamos implementar o teste primeiro do nosso app.

A versão 1.1 do Android Studio já cria um template de teste que era o antigo "Intrumentation test". Vamos remover ele e criar o nosso. 

![passo 1](http://rafael-miceli.com.br/ico/TDD-Com-Android-RED/passo1.png)

Após isso precisamos agora criar nossa estrutura de testes no Android Studio, lembrem-se de mudar a visualização da sua project tab para "Project View"

![passo 2](http://rafael-miceli.com.br/ico/TDD-Com-Android-RED/passo2.png)

![passo 3](http://rafael-miceli.com.br/ico/TDD-Com-Android-RED/passo3.png)

Vamos importar o JUnit como nosso framework de testes. Dentro de Gradle Scripts abra o build.gradle(Module app), em nosso objeto dependencies, vamos adicionar uma dependência ao JUnit: 

![passo 4](http://rafael-miceli.com.br/ico/TDD-Com-Android-RED/passo4.png)

Clique em gradle sync que o JUnit vai ser adicionado ao nosso projeto. 

![passo 5](http://rafael-miceli.com.br/ico/TDD-Com-Android-RED/passo5.png)

Agora precisamos mudar o build variants do "Instrumentation Test" para "Unit Test"

![passo 6](http://rafael-miceli.com.br/ico/TDD-Com-Android-RED/passo6.png)

###Criando o teste

Finalmente! Podemos criar nossa classe de teste!

![passo 7](http://rafael-miceli.com.br/ico/TDD-Com-Android-RED/passo7.png)

Vamos chama-la de "UserTest", e nela adicionamos o import ao JUnit: 

![passo 8](http://rafael-miceli.com.br/ico/TDD-Com-Android-RED/passo8.png)

Agora podemos começar a escrever nosso primeiro teste. 

Quando escrevemos nossos testes, dar um bom nome para ele é muito importante pois podemos usar nossos testes como documentação para mapear os comportamentos do nosso app, como o Uncle Bob diz "software is about behavior". 
Para dar um bom nome lembre também do princípio DAMP (descriptive and meaningful phrases). 
Não vou entrar em detalhes sobre nomes de testes, pois este é um assunto para muitos debates. Vamos dar o nome:  

should_login_user_successfully() 

![passo 9](http://rafael-miceli.com.br/ico/TDD-Com-Android-RED/passo9.png)

####Observação importante

Uma observação sobre nomes. Se seu código ou grande parte dele estiver com as classes de regras de negócios em português, escreva seus testes em português 

Vamos rodar nosso teste! para rodá-lo clicamos com o botão direito no método que vamos testar e selecionamos run *nome-do-seu-teste*, ou usamos o atalho ctrl+shift+F10. 

![passo 10](http://rafael-miceli.com.br/ico/TDD-Com-Android-RED/passo10.png)

Excelente! O teste passou, nosso framework está ok. 

![passo 15](http://rafael-miceli.com.br/ico/TDD-Com-Android-RED/passo15.png)

Continuando, vamos seguir o fluxo do TDD e vamos fazer o teste falhar, para isto vamos também importar mais duas bibliotecas, e vamos implementar o design do nosso login. 

![passo 14](http://rafael-miceli.com.br/ico/TDD-Com-Android-RED/passo14.png)

Ótimo! Agora nosso teste está falhando porque nossa classe "User" e nosso método "login " não existem. 
Vamos cria-los: 

![passo 11](http://rafael-miceli.com.br/ico/TDD-Com-Android-RED/passo11.png)

![passo 12](http://rafael-miceli.com.br/ico/TDD-Com-Android-RED/passo12.png)

Vamos rodar nosso teste novamente. 

![passo 13](http://rafael-miceli.com.br/ico/TDD-Com-Android-RED/passo13.png)

Muito bom! Atingimos a fase Red em nosso fluxo. 

No próximo artigo vamos fazer este teste passar e refatora-lo, após isso vocês vão ter uma surpresa! 