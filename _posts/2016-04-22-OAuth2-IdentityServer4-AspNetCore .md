---
author: Rafael Miceli
comments: true
layout: default 
category : OAuth 
title: "OAuth2 - IdentityServer4 com AspNetCore" 
date: 2016-04-22 11:10:00
---

O Beta 1 do [_IdentityServer 4_](https://github.com/IdentityServer/IdentityServer4) já está disponível para uso com __AspNet Core 1__, e que tal vermos como criar nosso AuthorizationServer usando o novo IdentityServer?

### O que mudou?

> Primeiramente, [Dominick Baier](https://twitter.com/leastprivilege) possui um artigo muito melhor que o meu explicando as [mudanças do Id4](https://leastprivilege.com/2016/01/11/announcing-identityserver-for-asp-net-5-and-net-core/)

O que vou passar aqui vai ser uma pincelada.

No novo _IndentityServer_ você tem total controle para customizar a UI de uma forma que antes não era possível, e nesta versão beta ainda precisamos configurar a UI, mesmo que seja com os valores padrões, creio eu que em futuramente ainda vai haver um template no [Yo](http://yeoman.io/) para isso!

Depois das novidades na UI, é só isso.... 

Sim, o resto das funcionalidades continua exatamente o mesmo. Então agora vamos ver como criamos nosso AuthorizationServer.

### Criando o Authorization Server no AspNet Core

> Na criação desse Authorization Server apesar de estar em um Windows vou criar o mesmo de forma que possamos também usar em um Linux.

Passo 1 - Vamos usar nosso Yo e criar uma aplicação web vazia! (Para caso de duvidas recomendo [este post meu](http://rafael-miceli.com.br/aspnetcore/2016/03/31/Criando-Projeto-AspNetCore-No-Ubuntu.html))

Passo 2 - Se fosse pelo antigo Asp.Net usariamos o Console Package Manager, mas no Asp.Net Core vamos adicionar as seguintes dependências em nosso project.json:

```javascript
"dependencies": {
    "IdentityServer4": "1.0.0-beta1-update1",
    "Microsoft.ApplicationInsights.AspNet": "1.0.0-rc1",
    "Microsoft.AspNet.IISPlatformHandler": "1.0.0-rc1-final",
    "Microsoft.AspNet.Mvc": "6.0.0-rc1-final",
    "Microsoft.AspNet.Mvc.TagHelpers": "6.0.0-rc1-final",
    "Microsoft.AspNet.Server.Kestrel": "1.0.0-rc1-final",
    "Microsoft.AspNet.StaticFiles": "1.0.0-rc1-final",
    "Microsoft.AspNet.Tooling.Razor": "1.0.0-rc1-final",
    "Microsoft.Extensions.Configuration": "1.0.0-rc1-final",
    "Microsoft.Extensions.Configuration.FileProviderExtensions": "1.0.0-rc1-final",
    "Microsoft.Extensions.Configuration.Json": "1.0.0-rc1-final",
  },
```

Após isso damos um __dnu restore__

Passo 3 - Vamos começar a configurar nosso IdentityServer em nosso _Startup.cs_.([neste post](http://rafael-miceli.com.br/oauth/2016/04/20/OAuth2-Implementando-AuthorizationServer-IdentityServer.html) explico com mais detalhes o IdentityServer) 

Vamos em nosso método __ConfigureServices__ e nele faremos como era no IndentityServer3:

Pegamos nosso certificado para assinatura de nosso token.
Adicionamos nosso certificado no _option_ do nosso IdentityServer
Adicionamos em memória nossos Scopes, Clients e Users 

Scopes

```csharp
public class Scopes
{
    public static IEnumerable<Scope> Get()
    {
        return new []
        {
            StandardScopes.OpenId,
            StandardScopes.Profile,
            StandardScopes.OfflineAccess,

            new Scope
            {
                Name = "read",
                DisplayName = "Leitura",
                Type = ScopeType.Resource,
                Emphasize = false,
                AllowUnrestrictedIntrospection = true,
                ScopeSecrets = new List<Secret> { new Secret("secret".Sha256()) }
            }
        };
    }
}
```

Clients 

```csharp
public class Clients
{
    public static IEnumerable<Client> Get()
    {
        return new []
        {
            new Client
            {
                ClientId = "teste",
                ClientName = "Teste OAuth2",
                ClientSecrets = new List<Secret>
                {
                    new Secret("secret".Sha256())
                },
                Flow = Flows.ResourceOwner,
                //Pequena mudança no nome dos Scopes comparado ao IdentityServe3
                AllowedScopes  = new List<string> { StandardScopes.OpenId.Name },
                Enabled = true
            }             
        };
    }
}
```

Users

```csharp
static class Users
{
    public static List<InMemoryUser> Get()
    {
        var users = new List<InMemoryUser>
        {
            new InMemoryUser
            {
                Subject = "GUIDS",
                Username = "rafael.miceli@oauth.com",
                Password = "123456",
                //Pequena mudança no nome do Claims comparado ao IdentityServe3
                Claims = new []
                {
                    new Claim(JwtClaimTypes.Name, "Rafael Miceli")
                }
            }                
        };

        return users;
    }
}
```


E a diferença vem aqui. Ao configurarmos o MVC do nosso Asp.Net Core adicionamos um _RazorOptions_. Nosso _ConfigureServices_ fica da seguinte maneira:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    var certificado = Convert.FromBase64String("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");
    
    var cert = new X509Certificate2(certificado, "password");
    
    var identityServerBuilder = services.AddIdentityServer(options =>
    {
        options.SigningCertificate = cert;
    });
    
    identityServerBuilder.AddInMemoryClients(Clients.Get());
    identityServerBuilder.AddInMemoryScopes(Scopes.Get());
    identityServerBuilder.AddInMemoryUsers(Users.Get());
    
    services.AddMvc()
    .AddRazorOptions(razor =>
        {
            razor.ViewLocationExpanders.Add(new AuthorizationServer.UI.CustomViewLocationExpander());
        });
}
```

Repare que adicionamos em nosso _RazorOptions_ a classe __CustomViewLocationExpander__. Esta classe é aonde está o código para criação das Views Padrões do IdentityServer4. Não vou postar esse código aqui por ser um conjunto de muitas classes, mas ele está em meu [Github](https://github.com/Rafael-Miceli/Blog-Codes/tree/master/IdeiasComAzeite/IdentityServer4-Sample/AuthorizationServer)

Para finalizar _usamos_ nossas configurações no método _Configure_:

```csharp
public void Configure(IApplicationBuilder app)
{
    app.UseIISPlatformHandler();

    app.UseIdentityServer();
    app.UseStaticFiles();
    app.UseMvcWithDefaultRoute();
}
```

Agora vamos executar nossa aplicação:

![passo1](http://rafael-miceli.com.br/ico/OAuth2-IdentityServer4-AspNetCore/passo1.png)

Percebam que o layout está faltando. Como disse, nesta versão do IdentityServer ainda temos de configurar a UI nós mesmos, ou seja, peguem neste [link](https://github.com/Rafael-Miceli/Blog-Codes/tree/master/IdeiasComAzeite/IdentityServer4-Sample/AuthorizationServer) o __wwwroot__ com o css para ajustar o layout.

No final temos nosso IdentityServer:

![passo2](http://rafael-miceli.com.br/ico/OAuth2-IdentityServer4-AspNetCore/passo2.png)

Este IdentityServer está configurado exatamente como do [post](http://rafael-miceli.com.br/oauth/2016/04/20/OAuth2-Implementando-AuthorizationServer-IdentityServer.html) anterior. Vamos realizar o mesmo teste com postman:

![passo3](http://rafael-miceli.com.br/ico/OAuth2-IdentityServer4-AspNetCore/passo3.png)

E ai esta nosso Access Token.

Próximo passo é termos nossa aplicação _Client_ se logando através do nosso _AuthorizationServer_.
