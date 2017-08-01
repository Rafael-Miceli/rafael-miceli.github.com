---
author: Rafael Miceli
comments: true
layout: default 
category : dotnetcore
title: "IdentityServer com múltiplos databases através de JWT e C.I." 
date: 2017-07-31 23:40:00
---

### Cenário

Dando continuidade ao cenário [proposto anteriormnte](http://rafael-miceli.com.br/dependency-injection/2017/06/11/Apontando-Multiplos-Databases-DI-WCF.html), aonde realizamos um injeção de dependência buscando no _Header_ qual _Client_ nosso esta chamando a aplicação para definirmos para qual base de dados este vai apontar, vamos agora dar uma leve melhorada relativo a segurança do mesmo.

Anteriormente nós estávamos passando no *Header* um **Token** fictício para identificarmos qual *Client* estava realizando a chamada, para assim construir a *Connection String* dele. Mas isso é muito inseguro, vamos melhorar isso.

Vamos então criarmos um **Authorization Server** e nele vamos configurar um **Client Credentials Flow** e para provarmos quão desacoplado está esse Cenário vamos criar nosso **Auth Server** utilizando [IdentityServer 4](http://docs.identityserver.io/en/release/) com [Asp.Net Core](https://docs.microsoft.com/en-us/aspnet/core/)

### Aplicando o cenário

Para este cenário estou usando a versão 1.1 do SDK e 1.1.2 do Runtime do DotNet Core.

O primeiro passo é criarmos uma aplicação AspNet Core Empty.

Após, vamos adicionar o package do IdentityServer. Basta adicionar o `IdenttiyServer4` versão `1.3.1` em seu `.csproj`

Vamos configurar o IdentityServer, deixando nosso **Startup.cs** da seguinte maneira:

```csharp
public class Startup
{
    public void ConfigureServices(IServiceCollection services)
    {
        services.AddIdentityServer()
            .AddTemporarySigningCredential()
            .AddInMemoryApiResources(Config.GetApiResources())
            .AddInMemoryClients(Config.GetClients());
    }

    public void Configure(IApplicationBuilder app, IHostingEnvironment env, ILoggerFactory loggerFactory)
    {
        loggerFactory.AddConsole();

        if (env.IsDevelopment())
        {
            app.UseDeveloperExceptionPage();
        }

        app.UseIdentityServer();
    }
}
```

Também temos que ajustar nosso **Program.cs** para ficar da seguinte maneira:

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        Console.Title = "IdentityServer";

        var host = new WebHostBuilder()
            .UseKestrel()
            .UseUrls("http://localhost:5000")
            .UseContentRoot(Directory.GetCurrentDirectory())
            .UseIISIntegration()
            .UseStartup<Startup>()
            .Build();

        host.Run();
    }
}
```

Além disso, nosso Startup.cs está fazendo referência a uma classe `Config` aonde configuramos nossos Clients para ele conseguir realizar sua autenticação. Nossa classe `Config` fica da seguinte maneira:

```csharp
public class Config
{
    // scopes define the API resources in your system
    public static IEnumerable<ApiResource> GetApiResources()
    {
        return new List<ApiResource>
        {
            new ApiResource("api1", "My API")
        };
    }

    // client want to access resources (aka scopes)
    public static IEnumerable<Client> GetClients()
    {
        return new List<Client>
        {
            new Client
            {
                ClientId = "Client1",
                AllowedGrantTypes = GrantTypes.ClientCredentials,

                ClientSecrets =
                {
                    new Secret("Client1Secret".Sha256())
                },
                AllowedScopes = { "api1" }
            },
            new Client
            {
                ClientId = "Client2",
                AllowedGrantTypes = GrantTypes.ClientCredentials,

                ClientSecrets = 
                {
                    new Secret("Client2Secret".Sha256())
                },
                AllowedScopes = { "api1" }
            }
        };
    }
}
```

Excelente! Para testarmos se estamos recebendo nosso token podemos rodar nossa aplicação e fazer uma chamada com o Postman em nosso endpoint **connect/token**

![imagem1](http://rafael-miceli.com.br/ico/IdentityServer-Com-Multiplos-Databases-DI-WCF/imagem1.png)

Agora nosso próximo passo é enviar esse token no header para nossa aplicação WCF aonde ela vai ler este token para conectarmos ao database.

Para isso vamos ter que atualizar nosso WcfApp um pouco.

Primeiro vamos precisar instalar um novo Package. No Package Manager Console vamos executar `install-package System.IdentityModel.Tokens.Jwt`.

Após, na classe `Bootstrap.cs` vamos atualizar ela para ficar da seguinte maneira:

```csharp
public class Bootstrapper
{
    public static IContainer BuildContainer()
    {
        var builder = new ContainerBuilder();
        builder.RegisterType<AlunoService>().As<IAlunoServiceContract>();
        builder.RegisterType<AlunoApplicationService>().As<IAlunoApplicationService>();

        builder.Register(r => BuildDbRepo()).As<IAlunoRepoToAnyDb>();

        return builder.Build();
    }

    private static AlunoRepoToAnyDb BuildDbRepo()
    {
        IncomingWebRequestContext request = WebOperationContext.Current.IncomingRequest;
        WebHeaderCollection headers = request.Headers;

        var clientToken = headers["clientToken"];

        var clientClaim = GetClientClaimFromToken(clientToken);

        var connStringDb = MontarConn(clientClaim);

        return new AlunoRepoToAnyDb(connStringDb);
    }

    private static string GetClientClaimFromToken(string clientToken)
    {
        var tokenHandler = new JwtSecurityTokenHandler();
        var principal = tokenHandler.ReadToken(clientToken) as JwtSecurityToken;

        return principal.Claims.FirstOrDefault(c => c.Type == "client_id").Value;
    }

    private static string MontarConn(string clientClaim)
    {
        if (clientClaim == "Client1")
        {
            return @"Data Source=(localdb)\MSSQLLocalDB;Initial Catalog=AlunosClient1;Integrated Security=True;Connect Timeout=30;Encrypt=False;TrustServerCertificate=True;ApplicationIntent=ReadWrite;MultiSubnetFailover=False";
        }

        if (clientClaim == "Client2")
        {
            return @"Data Source=(localdb)\MSSQLLocalDB;Initial Catalog=AlunosClient2;Integrated Security=True;Connect Timeout=30;Encrypt=False;TrustServerCertificate=True;ApplicationIntent=ReadWrite;MultiSubnetFailover=False"; ;
        }

        throw new Exception("Not a valid client token");
    }
}
```

No método `GetClientClaimFromToken` nós recbemos agora o Token gerado pelo nosso AuthServer e lemos o valor da **Claim** *client_id*.

Os outros métodos sofreram leves mudanças por conta do novo Token.

Para testar, pegamos um token que foi gerado na imagem anterior e passamos ele no header para a chamada do WCF:

![imagem2](http://rafael-miceli.com.br/ico/IdentityServer-Com-Multiplos-Databases-DI-WCF/imagem2.png)

### Conclusão

Até aqui conseguimos acessar nossa base de dados para o cliente que fez login, tendo um Token gerado por nosso Authorization Server.

Porém, ainda tem um outro ponto referente a segurança a ser abordado que vai ficar para o próximo capítulo.