---
author: Rafael Miceli
comments: true
layout: default 
category : aspnetcore 
title: "Executando uma aplicação aspnet core" 
date: 2016-04-05 20:00:00
--- 


Conhecendo um pouco do novo aspnet pelos artigos anteriores vamos rodar nossa aplicação e ver no Ubuntu como levantamos uma aplicação. 

### DNX e DNU 

Quando você instala o DNX mais um comando vem com ele, o DNU (DNX Utility). 

Usamos o DNU para restaurar os pacotes que nossa aplicação depende que estão no nosso project.json, e para também realizar um build do mesmo. 

Para restaurar os pacotes usamos: 

    dnu restore 

Quando realizamos o restore reparem que um novo arquivo vai aparecer no nosso projeto, que é o project.lock.json. 

O DNU criar este arquivo para no mesmo ele já mapear todo o grafo de dependências que o projeto vai precisar, ou seja ao invés de ele instalar os pacotes e suas dependências a cada DNX run, ele sempre usa os mapeados no lock. Para saber como funciona o gerenciamento das dependências com o dnu veja na documentação  

Com o DNX nós executamos nossa aplicação e comandos do nosso project.json. 

Repare que temos dois comandos no mesmo, __kestrel__ e __ef__. 

![kestrel](http://rafael-miceli.com.br/ico/Executando-Aplicacao-AspNetCore/kestrel.png) 

O kestrel é o nosso host ao invés do iis. Então se queremos executar nossa aplicação usamos o seguinte comando: 

    dnx web 

Após isso o kestrel vai provisionar nossa aplicação na porta 5000 do localhost. 

![provisionado](http://rafael-miceli.com.br/ico/Executando-Aplicacao-AspNetCore/provisionado.png) 

### Usando diferentes portas 

Se quisermos mudar a porta em que nossa aplicação esta rodando podemos rodar o comando: 

    dnx web --server.urls http://localhost:5001 

Também Podemos mudar o comando no project.json 

![porta](http://rafael-miceli.com.br/ico/Executando-Aplicacao-AspNetCore/porta.png)

![porta-provisionado](http://rafael-miceli.com.br/ico/Executando-Aplicacao-AspNetCore/porta-provisionado.png) 

Com o comando mudado podemos executar como no inicio: 

    dnx web 

Em um próximo artigo vou mostrar como configurar para usar https no asp.net core 

