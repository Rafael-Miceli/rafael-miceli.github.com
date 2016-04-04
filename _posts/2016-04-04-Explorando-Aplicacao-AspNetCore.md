---
author: Rafael Miceli
comments: true
layout: default 
category : aspnetcore 
title: "Explorando uma aplicação aspnet core" 
date: 2016-04-04 09:00:00
--- 

Se você já criou uma aplicação (de um template mesmo) no novo asp.net core deve ter notado algumas diferenças, ou talvez muitas.  
Já sei que falei em um artigo anterior sobre algumas diferenças, na época em que o nome ainda era asp.net 5, mas vamos lá mesmo assim.  

### O que sumiu  

Podemos notar de cara que não temos mais o __global.asax__ e nem o __web.config__. Ambos não existem mais  

[global asax](http://rafael-miceli.com.br/ico/Explorando-Aplicacao-AspNetCore/global-asax.png)  

O antigo web.config que era responsável por nosso AppSettings, ConnectionString, Assemblies dependentes, Runtime, entre muitas outras, passou suas responsabilidades para novos arquivos.  
appsettings.json é aonde colocamos os appsettings e connection strings agora.  

No  project.json definimos quais pacotes nossa aplicação precisa para funcionar, qual Runtime usar, comandos para facilitar o uso de algumas ferramentas (como Krestel ou Enyityframework), entre outras...  

O global.asax também saiu e deixou seu lugar para o startup.cs  

Se você usou o __OWIN__ no MVC 5 vai notar muitas similaridades. Em nosso startup.cs no método __ConfigureServices__ inicializamos nossas dependências. Isso mesmo, asp.net core já vem com um _[light Dependency Injection](https://docs.asp.net/en/latest/fundamentals/dependency-injection.html#what-is-dependency-injection)_ incluído.   

Neste método não inicializamos apenas dependências de nossos serviços, mas também de qualquer pacote que vamos usar, como MVC, Enyityframework, Identity...  

[configuration services](http://rafael-miceli.com.br/ico/Explorando-Aplicacao-AspNetCore/configuration-services.png)  

No método __Configure__ realizamos as configurações das nossas dependências como por exemplo, configurações da nossa string de conexão do EF, rotas do MVC, serialization de viewmodels para javascript...  

[configure](http://rafael-miceli.com.br/ico/Explorando-Aplicacao-AspNetCore/configure.png)  

### wwwroot  

wwwroot é uma nova pasta que faz parte da estrutura do asp.net.   

Nesta pasta é que todo nosso conteúdo estático vai se encontrar. Nela vão estar nossos javascripts, css e html que não vão ser processados com os taghelpers (vamos chegar lá) do novo Razor.  

[wwwroot](http://rafael-miceli.com.br/ico/Explorando-Aplicacao-AspNetCore/wwwroot.png)  

### Gerenciando pacotes  

Neste novo asp.net o nuget não é mais o responsável por gerenciar todas as dependências como anteriormente. No passado se quiséssemos o jquery instalávamos a partir do nuget, o mesmo para o bootstrap ou qualquer outra biblioteca de front-end.   

Hoje isso esta completamente separado. Se quisermos bibliotecas de front-end instalamos as mesmas com bower, enquanto as de back-end de .net com o nuget.  

E temos mais uma novidade na gerencia de pacotes. Se quisermos também instalar alguma biblioteca javascript de back-end, usamos o __nodejs__!  

O package.json indica quais pacotes de javascript back-end estamos usando, perceba que o template já usa o gulp e outros para pipes do gulp como uglify.  
No próximo artigo vamos botar para rodar com kerstel o aspnet no ubuntu  

Mas antes um extra  

### Apenas o npm?  

O [radar da ThoughtWorks](https://www.thoughtworks.com/radar/techniques/npm-for-all-the-things) fez uma pesquisa em que para gerenciar pacotes javascript , apenas o uso do npm é necessário, ou seja, não usar o bower, apenas o npm para qualquer biblioteca js. Vale a pena dar uma olhada.  
