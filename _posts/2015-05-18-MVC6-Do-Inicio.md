---
author: Rafael Miceli
comments: true
layout: default
category : MVC6
title: "Criando um Empty Project em MVC 6"
date: 2015-05-18 13:30:00
---


Antes de começarmos a criar o projeto em MVC lembrem-se que o AspNet5 ainda está em RC (Release Cadidate), o que significa que algumas coisas ainda vão mudar (mas não muito), mas como este artigo cobre os fundamentos não vamos precisar nos preocupar com isso.

Vamos começar a criar nosso projeto com o MVC6 do início.

Primeiro, precisamos baixar o [Visual Studio Community 2015](https://www.visualstudio.com/) e instala-lo.

Após isto, vamos abrir o Visual Studio e em nossa _Start Page_ clicamos em _Create New Project_.

![passo 1](http://rafael-miceli.com.br/ico/MVC6-Do-Inicio/passo1.png) 

Selecione _C#_ como nossa linguagem, depois Web e selecione _ASP.NET Web Application Template_.

Vamos chamar nosso projeto de _"WebAppEvents"_ e nossa Solution de _"SlnEvent"_.

![passo 2](http://rafael-miceli.com.br/ico/MVC6-Do-Inicio/passo2.png)

Ainda temos mais um passo, precisamos selecionar um dos _ASP.NET 5 Preview Templates_. 

Vamos selecionar o _Empty_.

![passo 3](http://rafael-miceli.com.br/ico/MVC6-Do-Inicio/passo3.png)

Se você ver o novo template _Empty_, ele está praticamente vazio mesmo, pois possui o mínimo para rodar a aplicação!  

![passo 4](http://rafael-miceli.com.br/ico/MVC6-Do-Inicio/passo4.png)

Vamos ver o que temos até o momento, vamos rodar nossa aplicação.

![passo 5](http://rafael-miceli.com.br/ico/MVC6-Do-Inicio/passo5.png)

Se você está se perguntando _da onde saiu este "Hello World"?_, você precisa ir ao seu _startup.cs_.
 
![passo 19](http://rafael-miceli.com.br/ico/MVC6-Do-Inicio/passo19.png)

Ai está nosso "Hello World", mas porque um "Hello World" aqui e não em um Controller? Bem, primeiro vamos falar do _OWIN_. 

### OWIN  

No útlimo [post](http://rafael-miceli.com.br/mvc/2015/05/06/AspNet5-Novo-MVC.html#onde-est-o-globalasax) vimos um pouco sobre _OWIN_, vou pular a introdução e mostrar um exemplo do uso do _OWIN_ direto. 

Apenas "Hello World" é muito comum, que tal mostrarmos algo diferente?

Vamos em nosso project.json e adicionar o Package _Diagnostics_. 

![passo 6](http://rafael-miceli.com.br/ico/MVC6-Do-Inicio/passo6.png) 

Agora, dentro do nosso método _Configuration_ vamos adicionar o seguinte código:

![passo 7](http://rafael-miceli.com.br/ico/MVC6-Do-Inicio/passo7.png)

Vamos rodar nossa aplicação e ver o resultado. 

![passo 8](http://rafael-miceli.com.br/ico/MVC6-Do-Inicio/passo8.png)

Ótimo! Mas nós queremos usar o MVC, então vamos configurar o nosso middleweare para isso!

Vamos adicionar o Package MVC 

![passo 20](http://rafael-miceli.com.br/ico/MVC6-Do-Inicio/passo20.png)

Implementar nosso middleweare. 

![passo 10](http://rafael-miceli.com.br/ico/MVC6-Do-Inicio/passo10.png)

E adicionar nossos Controllers em nossa clássica estrutura:

![passo 11](http://rafael-miceli.com.br/ico/MVC6-Do-Inicio/passo11.png)

![passo 12](http://rafael-miceli.com.br/ico/MVC6-Do-Inicio/passo12.png)

![passo 13](http://rafael-miceli.com.br/ico/MVC6-Do-Inicio/passo13.png)

![passo 14](http://rafael-miceli.com.br/ico/MVC6-Do-Inicio/passo14.png)

![passo 15](http://rafael-miceli.com.br/ico/MVC6-Do-Inicio/passo15.png)

![passo 16](http://rafael-miceli.com.br/ico/MVC6-Do-Inicio/passo16.png)

Referenciando o [post anterior](http://rafael-miceli.com.br/mvc/2015/05/06/AspNet5-Novo-MVC.html#Dependency-injection-nativa) mais uma vez, lembre que vimos que o novo AspNet possui Dependency Injection Nativo. Vemos a injeção do MVC no método _ConfigureServices_.

![passo 21](http://rafael-miceli.com.br/ico/MVC6-Do-Inicio/passo21.png)

Vamos criar nossas próprias depdências depois.  

Antes de rodarmos a aplicação vamos configurar nosso _Default Route_.

Agora para configurarmos podemos fazer desta maneira:

![passo 17](http://rafael-miceli.com.br/ico/MVC6-Do-Inicio/passo17.png)

Ok, agora vamos ver nossa página rodando:

![passo 18](http://rafael-miceli.com.br/ico/MVC6-Do-Inicio/passo18.png) 

Excelente! 

### Resumindo

Vimos que configurar nossa aplicação em nosso middleweare. Não vimos OWIN ou Katana detalhadamente, mas aqui vão excelentes referências sobre o assunto.  

http://owin.org/ 

Se você está começando com Dependency Injection eu recomendo que você leia sobre o [Princípio de Inversão de Dependência(DIP)](http://en.wikipedia.org/wiki/Dependency_inversion_principle) e depois sobre [Dependency Injection](http://en.wikipedia.org/wiki/Dependency_injection)

Aqui segue alguns bons livros sobre o tema:

http://martinfowler.com/articles/injection.html

http://www.amazon.com/Dependency-Injection-NET-Mark-Seemann/dp/1935182501

Vamos continuar criando a aplicação, até a próxima! 
 


 


 
