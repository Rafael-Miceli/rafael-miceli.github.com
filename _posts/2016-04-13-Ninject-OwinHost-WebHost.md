---
author: Rafael Miceli
comments: true
layout: default 
category : DI 
title: "Ninject - OwinHost e WebHost" 
date: 2016-04-13 09:00:00
---

Quando temos uma aplicação como a aplicação de escola que possuímos de exemplo 
(se quiser pode pegar no meu [Github](https://github.com/Rafael-Miceli/Blog-Codes/tree/master/IdeiasComAzeite/AutoFixtureSample/Escola)), 
imagina se queremos inicializar todas as dependências que temos? Vamos fazer isso no construtor dos Controllers? Imagina para cada Controller fazer isso? Imagina mais, imagina ter de inicializar um grafo de dependências bem grande? 
O tamanho do construtor e o problema em manutenções futuras que isso pode gerar?

Para isso usamos _D.I. Containers_. E neste post vamos ver como usar o Ninject em nossa aplicação. Lembrando que existem muitos outros, aqui alguns só para listar:

- __Ninject__
- AutoFac
- Castle Windsor
- Unity

E muitos outros...

### Ninject

O Ninject é um dos mais famosos frameworks de _D.I. Container_ e se queremos usar ele para orquestrar as instâncias de nosso sistema temos antes de definir qual biblioteca do Ninject usar em nossa aplicação.

Vamos preparar inicialmente para nossas instâncias funcionarem tanto para MVC quanto para WebApi.

Para isso instalamos os pacotes:

Ninject
Ninject.MVC5
Ninject.Web.Common
Ninject.Web.WebApi

Para instalar os pacotes, dentro do Visual Studio abra a janela _Package Manager Console_ dentro dessa janela no topo dela na opção _Default project_ selecione a aplicação Web e execute o commando:

    Install-Package Ninject

Faça esse comando para cada um dos packages listados acima.

Para finalizar a instalação do Ninject vamos depender de como estamos inicializando nossa aplicação Asp.Net.

### Ninject Webhost

Caso não estejamos usando _OWIN_ para inicializar nossa aplicação, e estamos usando o __IIS__ como host, então instalamos em nossa aplicação o pacote __Ninject.Web.Common.WebHost__.

Ao instalar vai ser criado um arquivo __NinjectWebCommon.cs__ no __App_Start__, aonde nele vamos mapear nossas instâncias da aplicação.

Para isso, dentro da classe __NinjectWebCommon.cs__, no método __RegisterServices__ vamos implementar o código: 

NinjectWebCommon.cs

```csharp
private static void RegisterServices(IKernel kernel)
{
    kernel.Bind<IMateriaRepo>().To<MateriaRepository>().InRequestScope();
    kernel.Bind<IMateriaService>().To<MateriaService>().InRequestScope();
}        
```

### Ninject OwinHost

Caso estejamos usando o owin, então instalamos o pacote __Ninject.Web,Common.OwinHost__.

Usando o _OwinHost_, no método __Configuration__ do nosso __Startup.cs__ precisamos do seguinte código:

```csharp
public void Configuration(IAppBuilder app)
{
    //HttpConfig
    var config = new HttpConfiguration();            

    //Web Api Configuration
    ConfigureWebApi(config);

    //Ninject Configuration
    app.UseNinjectMiddleware(() => NinjectBootstraper.CreateKernel.Value);
    app.UseNinjectWebApi(config);
}
```

E para termos separado o mapeamento de nossas dependências, vamos criar uma classe chamada __NinjectBootstraper.cs__ no __App_Start__ com a seguinte implementação:

```csharp
public static class NinjectBootstraper
{
    public static Lazy<IKernel> CreateKernel = new Lazy<IKernel>(() =>
    {
        StandardKernel kernel = new StandardKernel();
        kernel.Load(Assembly.GetExecutingAssembly());

        RegisterServices(kernel);

        return kernel;
    });

    private static void RegisterServices(KernelBase kernel)
    {
        kernel.Bind<IMateriaRepo>().To<MateriaRepository>().InRequestScope();
        kernel.Bind<IMateriaService>().To<MateriaService>().InRequestScope();
    }
}
```

Excelente!

Vimos duas formas de configurar nossa aplicação com o Ninject. Existe mais uma que é o __SelfHost__, mas não vou cobrir ela aqui.

Um detalhe bem importante.

Não misture os pacote __OwinHost__ e __WebHost__, tenha apenas referência a um em seu projeto. Pois quando você tem referência aos dois você recebe uma exceção.
