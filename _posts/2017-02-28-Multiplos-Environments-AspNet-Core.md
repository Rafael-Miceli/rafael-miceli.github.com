---
author: Rafael Miceli
comments: true
layout: default 
category : aspnetcore
title: "Setando diferentes ambientes com AspNet Core" 
date: 2017-02-28 22:30:00
---

Se você estivesse fazendo uma aplicação com AspNet MVC 5 e quisesse ter seu código preparado para suportar diferentes ambientes (como dev, teste e produção) você faria isso com seu web.config. Tendo diferentes web.ambiente.config.

De padrão de inicio um template de aplicação MVC 5 ja vinha com web.debug.config e web.release.config.

![imagem1](http://rafael-miceli.com.br/ico/Multiplos-Environments-AspNet-Core/imagem1.PNG)

Para você alternar entre configs de diferentes ambientes bastava você selecionar o seu build configuration no Visual Studio

![imagem2](http://rafael-miceli.com.br/ico/Multiplos-Environments-AspNet-Core/imagem2.PNG)

Mas… Com AspNet Core isso deu uma mudada…

### Não mais "configs"

No Aspnet Core não definimos mais os diferentes ambientes com o web.config. 
> Apesar de ter um web.config  no AspNet Core ele é apenas para [configurações espcíficas com IIS e nada mais](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/configuration#the-webconfig-file). Configurações da sua aplicação agora vão em appsettings.json

### AppSettings.json

Agora utilizamos o __appsettings.json__ paara armazenarmos as configurações da nossa aplicação. E como era no antigo web.ambiente.config também temos o mesmo com appsettings.ambiente.json.

Se você criar uma aplicação AspNet Core com o template de Aplicação Web completa com Yo você pode ver seu appsettings.json

![imagem3](http://rafael-miceli.com.br/ico/Multiplos-Environments-AspNet-Core/imagem3.PNG)

Perceba que não temos ainda os ambientes… Mas vamos fazer isso. Crie uma copia do appsettings.json, mas com o nome __appsettings.Production.json__ e renomeie o outro para __appsettings.Development.json__.

![imagem4](http://rafael-miceli.com.br/ico/Multiplos-Environments-AspNet-Core/imagem4.PNG)

E neles crie uma key para testarmos chamada __"Teste"__ e no __appsettings.Development.json__ coloque o valor __Dev__ e no outro __Prod__.

![imagem5](http://rafael-miceli.com.br/ico/Multiplos-Environments-AspNet-Core/imagem5.PNG)

![imagem6](http://rafael-miceli.com.br/ico/Multiplos-Environments-AspNet-Core/imagem6.PNG)

### Preparando o Startup.cs

Em nosso Startup.cs nós temos nossa propriedade __Configuration__ que é um __IConfigurationRoot__ que define aonde estão as configurações da nossa aplicação! Isso definimos no __Construtor__ do nosso Startup.cs com um __ConfigurationBuilder__ 

![imagem7](http://rafael-miceli.com.br/ico/Multiplos-Environments-AspNet-Core/imagem7.PNG)

E lá temos nosso .AddJsonFile adicionando o __appSettings.ambiente.json__:

![imagem8](http://rafael-miceli.com.br/ico/Multiplos-Environments-AspNet-Core/imagem8.PNG)

O próximo passo é definirmos qual __Environment__ vai ser injetado no parâmetro do nosso construtor __IHostingEnvironment__. Se não dfinirmos o __Envronment__ por padrão ele vai setar o Production, o que não queremos para desenvolver localmente.

Para definirmos nosso __Environment__ podemos usar o powershell na raiz do projeto e executar o comando:

`$Env:ASPNETCORE_ENVIRONMENT = "Development"`

Agora quando executarmos `dotnet run ` vemos que nosso projeto rodou em ambiente de __Development__.

Para provar isso adicione o seguinte código após o `Configuration = builder.Build();` 

`Console.WriteLine($"ambiente = {Configuration["Teste"]}");`

Com este código vamos exibiri o valor da nossa key __Teste__.

