---
author: Rafael Miceli
comments: true
layout: default 
category : aspnetcore
title: "Realizando testes com InMemory provider do EF Core" 
date: 2017-03-26 21:00:00
---

EF Core possui bastante [Providers](https://docs.microsoft.com/en-us/ef/core/providers/), e um deles que veremos em específico é o __InMemory__

Este provider é excelente para você que esta querendo realizar testes de integração com seu Context.

### Um pouco sobre InMemory

O InMemory provider como diz na própria documentação __não é relacional__, ou seja, caso você realize alguma operação que viola a integridade do seu modelo relacional (Como por exemplo excluir uma tabela que possui uma FK referenciando a mesma, ele não vai reclamar).

Além disso ele possui configurações para você atender a cenários específicos do comportamente do seu banco.

Vamos conhecer melhor o provider com um exemplo.

### Testando com InMemory

Criando um projeto do template do Aspnet Core web completo. Após isso vamos criar um projeto de testes e em nosso projeto de testes referenciar o projeto web.

Vamos fazer o seguinte cenário de caso, vamos imaginar que queremos cadastrar um __Cliente__ e que exista uma regra que se houver um cliente com o mesmo Cnpj no banco de dados lançamos uma exceção.

 Para isto, vamos criar a nossa classa __Client__:

```csharp
public class Client
{
    public int Id { get; set; }
    public string Name { get; set; }
    public string Cnpj { get; set; }
}
```

Agora vamos adicionar nossa class __Client__ como um __DbSet__ em nosso __ApplicationDbContext__:

![imagem1](http://rafael-miceli.com.br/ico/InMemoryDb-Test/Imagem1.png)

Vamos adicionar o mesmo no banco de dados com nosso migrations do Ef. Para isso execute o seguinte comando na raiz do projeto:

`dotnet ef migrations add "ClientTable"`

Próximo etapa agora é configurarmos nosso provider InMemory em nosso projeto de testes adcionando o mesmo como uma dependencia em nosso __project.json__.

![imagem4](http://rafael-miceli.com.br/ico/InMemoryDb-Test/Imagem4.png)

Não esqueça de dar um `dotnet restore` após ter adicionado a referência ao __InMemory__.

Agora vamos começar a fazer um TDD.

Vamos criar um teste para quando não houver um __Client__ com o Cnpj que definimos que ele adicione no banco. 

Com isso para inicio vamos fazer uma carapaça do mesmo:

![imagem2](http://rafael-miceli.com.br/ico/InMemoryDb-Test/Imagem2.png)

![imagem3](http://rafael-miceli.com.br/ico/InMemoryDb-Test/Imagem3.png)

Agora em nosso teste vamos configurar o nosso IneMemory provider. Criamos o seguinte método:

```csharp
private static DbContextOptions<ApplicationDbContext> CreateNewContextOptions()
{
    // Cria um novo service provider, criando assim
    // uma nova instância com InMemory.
    var serviceProvider = new ServiceCollection()
        .AddEntityFrameworkInMemoryDatabase()
        .BuildServiceProvider();

    // Cria uma nova instância de options informando o context para usar o
    // InMemory e o novo service provider.
    var builder = new DbContextOptionsBuilder<ApplicationDbContext>();
    builder.UseInMemoryDatabase()
            .UseInternalServiceProvider(serviceProvider);

    return builder.Options;
}
```

Também adicione o `using` do seu __ApplicationDbContext__ e os seguintes `using` para as dependências que adiconamos:

```csharp
using Microsoft.EntityFrameworkCore;
using Microsoft.EntityFrameworkCore.Infrastructure;
using Microsoft.Extensions.DependencyInjection;
```

Próximo passo atualizarmos nosso teste para o seguinte:

```csharp
public void Given_A_Client_With_A_Cnpj_That_Doesnt_Exists_When_Adding_To_Database_Then_Persist_It()
{
    //Arrange
    var options = CreateNewContextOptions();
    CriarMassaDeClient(options);            
    string result;

    //Act
    using (var context = new ApplicationDbContext(options))
    {
        var sut = new HomeController(context);
        result = sut.CreateClient(new Client {
            Name = "Cliente vindo do teste",
            Cnpj = "654321"
        });
    }            

    //Assert
    using (var context = new ApplicationDbContext(options))
    {
        Assert.Equal("Funfou", result);
        Assert.NotNull(context.Clients.FirstOrDefault(c => c.Cnpj == "654321"));                
    }            
}
```

E reparem que estamos usando um método chamado `CriarMassaDeClient` que consiste em:

```csharp
private static void CriarMassaDeClient(DbContextOptions<ApplicationDbContext> options)
{
    using (var context = new ApplicationDbContext(options))
    {
        var client = new Client {
            Name = "Cliente de Teste 1",
            Cnpj = "123456"
        };

        context.Clients.Add(client);
        context.SaveChanges();
    }
}
```

E vamos atualizar nosso Controller adicionando o `_context` como private field sendo injetado no construtor:

```csharp
private readonly ApplicationDbContext _context;

public HomeController(ApplicationDbContext context)
{
    _context = context;
}
```

E a Action de adicionar Clients:

```csharp
public string CreateClient(Client client)
{
    _context.Add(client);
    _context.SaveChanges();
    return "Funfou";            
}
```

Se rodar o teste pode conferir que workou! Essa timeline do código você vê [aqui](https://github.com/Rafael-Miceli/Blog-Codes/tree/ecccea1ff3ed746e7cc7a3ba912c2c51be93f3c6)

Excelente! Agora vamos criar o teste com a verificação caso exista já o client com Aquele CNPJ que devemos tomar um erro:

```csharp
[Fact]
public void Given_A_Client_With_A_Cnpj_That_Already_Exists_When_Adding_To_Database_Then_Return_Cnpj_Already_Exists_Message()
{
    //Arrange
    var options = CreateNewContextOptions();
    CriarMassaDeClient(options);            
    string result;

    //Act
    using (var context = new ApplicationDbContext(options))
    {
        var sut = new HomeController(context);
        result = sut.CreateClient(new Client {
            Name = "Cliente vindo do teste",
            Cnpj = "123456"
        });
    }            

    //Assert
    using (var context = new ApplicationDbContext(options))
    {
        Assert.Equal("Cnpj já existe!", result);
        Assert.Equal(1, context.Clients.Count(c => c.Cnpj == "123456"));                
    }            
}
```

A Atualização do Controller vou deixar para o desafio. E se quiser ver como ficou o fim do código está no [repositório em meu github](https://github.com/Rafael-Miceli/Blog-Codes/tree/InMemDb)
