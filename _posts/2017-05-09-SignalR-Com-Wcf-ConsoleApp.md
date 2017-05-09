---
author: Rafael Miceli
comments: true
layout: default 
category : signalr
title: "Push Notification com SignalR e WCF" 
date: 2017-05-06 19:30:00
---

Imagine que você tem uma aplicação windows forms (Ou console app, ou WPF) e você queria receber notificações de um WCF que está recebendo dados de outro serviço? Para quem já teve a curiosidade ou necessidade de fazer isso, vou documentar como resolvi esta funcionalidade.

### SignalR

Para quem conhece o SignalR sabe que ele é uma ferramenta extremamente poderosa quando você precisa trabalhar com [push notification](https://en.wikipedia.org/wiki/Push_Notification) em Clients.

Para quem não conhece a [documentação da Microsoft](https://docs.microsoft.com/en-us/aspnet/signalr/overview/getting-started/introduction-to-signalr) é excelente. Na mesma explica o funcionamento do SignalR, o que ele faz por trás dos panos para realizar as notificações e diversos exemplos.

### Cenário SignarR com WCF

O cenário que vou preparar vai ser uma aplicação WCF que também é um Server Hub do SignalR e um Client em Windows Forms.

Em nosso cenário nosso WCF vai receber uma mensagem indicando que um professor foi associado a uma matéria, e um aluno que está vinculado a essa matéria recebe essa notificação.

### Criando o serviço WCF

Vamos criar um serviço WCF utilizando o template que já vem no Visual Studio 2015

![imagem1](http://rafael-miceli.com.br/ico/SignalR-Com-Wcf-ConsoleApp/imagem1.png)

Vamos adicionar no projeto um **Owin Startup.cs**.

### Adicionando SignalR

Feito isso, vamos adicionar o SignalR em nosso projeto:

`install-package Microsoft.AspNet.SignalR`

Após a instalação ter concluído precisamos adicionar a rota para o Endpoint **signalr** em nosso projeto no **startup.cs**

csharp
```
public void Configuration(IAppBuilder app)
{
    app.MapSignalR();
}
```

Após isso nosso próximo passo e criar nosso Hub, mas para organizar melhor, vamos colocar nosso hub em um projeto diferente, ou seja, vou criar uma class library e nela vamos criar nosso hub (também vamos precisar instalar mais uma vez o SignalR.

csharp
```
public class MyHub: Hub
{
    public void MyCall()
    {            
    }
}
```

> Inicialmente vamos ter um Hub simples apenas para testar uma chamada dele

Como nosso hub esta em um projeto diferente do que inicializa nosso signalR vamos precisar adicionar o seguinte codigo antes de mapear ele no startup.cs

`AppDomain.CurrentDomain.Load(typeof(MyHub).Assembly.FullName);`

Sendo assim nosso startup.cs fica da seguinte maneira

csharp
```
public void Configuration(IAppBuilder app)
{
    AppDomain.CurrentDomain.Load(typeof(MyHub).Assembly.FullName);
    app.MapSignalR();
}
```

E por fim vamos fazer nosso Service1 realizar um broadcast para os clients

csharp
```
public class Service1 : IService1
{

    public Service1()
    {
        Clients = GlobalHost.ConnectionManager.GetHubContext<MyHub>().Clients;
    }

    public Microsoft.AspNet.SignalR.Hubs.IHubConnectionContext<dynamic> Clients { get; private set; }

    public string GetData(int value)
    {
        Clients.All.notify("Mensagem");
        return string.Format("You entered: {0}", value);
    }

    public CompositeType GetDataUsingDataContract(CompositeType composite)
    {
        if (composite == null)
        {
            throw new ArgumentNullException("composite");
        }
        if (composite.BoolValue)
        {
            composite.StringValue += "Suffix";
        }
        return composite;
    }
}
```

### Criando um Client

Nosso WCF ja esta recebendo chamadas em seu endpoint e fazendo broadcast para os clients (que ainda nao existe). Agora vamos criar nosso client que vai receber essa chamada

Vamos adicionar um Console Application project em nossa solution.

Agora instalamos o signarR client:

`install-package Microsoft.AspNet.SignalR.Client`

No controller do nosso form, adicionamos o seguinte código:

csharp
```
var hubConnection = new HubConnection("http://localhost:61464/");
IHubProxy stockTickerHubProxy = hubConnection.CreateHubProxy("MyHub");
stockTickerHubProxy.On<string>("notify", message => Console.WriteLine("Do WCF {0}", message));
hubConnection.Start().Wait();

Console.ReadKey();
```

Este código também e extremamente simples para testarmos a comunicacao do nosso wcf fazendo push para nosso Console.

Este projeto esta no [repositório deste blog](https://github.com/Rafael-Miceli/Blog-Codes/tree/SignalR-With-Wcf) e se for executar o wcf e rodar o Console, podemos ver em nosso output a mensagem vindo dele.

![imagem2](http://rafael-miceli.com.br/ico/SignalR-Com-Wcf-ConsoleApp/imagem2.png)

No próximo post vamos fazer o push para apenas os alunos que estejam cadastrado nas matérias que o professor entrou e com o Windows Forms.
