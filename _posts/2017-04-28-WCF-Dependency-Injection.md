---
author: Rafael Miceli
comments: true
layout: default 
category : wcf
title: "Dependency Injection com WCF" 
date: 2017-04-28 23:50:00
---

Recentemente tive de criar um projeto WCF para trabalhar, e me deparei com a necessidade de fazer injeção de dependência no WCF e aqui vai o que descobri.

Encontrei um post do [Jimmy Bogard](https://lostechies.com/jimmybogard/2008/07/30/integrating-structuremap-with-wcf/) para realizar DI com Strucuture Map, mas achei um tanto complicado.

Então encontrei o do [Chsakell](https://chsakell.com/2015/07/04/dependency-injection-in-wcf/) que demonstrou como fazer com Autofac e que apesar do post ser um tanto denso, o sample code estava excelente!!!  Estava Clean e fácil de entender.

Para aplicarmos ele seguimos os seguintes passos:

1 - Crie seu projeto WCF com o template do Visual Studio (Lembrando que o [Asp.Net Core ainda não possui WCF Server](https://github.com/dotnet/wcf/issues/1200))

2 - Instale o pacote do AutoFac para WCF `install-package Autofac.Wcf`

3 - Crie uma classe chamada Bootstrapper na raiz do seu projeto

4 - Implemente a classe Bootstrapper da seguinte maneira
Nesta classe o método estático `BuildCotainer`vai criar o container do Autofac e dentro dele vamos mapear as instâncias que queremos injetar. E vamos retornar este Container.

5 - Adicione um Global.asax na sua aplicação e implemente o seguinte código em seu `Application_Start`:

    IContainer container = Bootstrapper.BuildContainer();
    AutofacHostFactory.Container = container;

6 - Agora expanda seu `.svc` e exclua o seu code behind.

7 - Após, crie uma nova classe `AlunosService`, implemente nela o `IService1` já existente

8 - Agora dentro seu `.svc` (que está sozinho), remova a propriedade `CodeBehind` e na propriedade `Service` insira sua Interface, `IService1`, com o namespace completo dela mais o namespace apenas, e depois adicione a proriedade factory apontando para o `AutofacServiceHostFactory`. Vai ficar tipo assim:

```
<%@ ServiceHost 
    Language="C#" 
    Debug="true"     
    Service="namespace.IService1, namespace"
    Factory="Autofac.Integration.Wcf.AutofacServiceHostFactory, Autofac.Integration.Wcf"%>

```

9 - Vamos voltar em nosso Bootstrapper e em seu `BuildContainer` adicione a injeção do aluno, da seguinte forma:

```csharp
public static IContainer BuildContainer()
{
    var builder = new ContainerBuilder();    
    builder.RegisterType<IService1>().As<AlunoService>();    
    return builder.Build();
}
```

Pronto! Temos nossa injeção de dependencia com Autofac no WCF. Lembrando que este código esta em meu [repo do Github](https://github.com/Rafael-Miceli/Blog-Codes)
