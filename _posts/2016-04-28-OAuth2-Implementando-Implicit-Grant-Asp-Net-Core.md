---
author: Rafael Miceli
comments: true
layout: default 
category : OAuth 
title: "OAuth2 - Implict Code Flow no Asp.Net Core" 
date: 2016-04-28 18:30:00
---

Em um post anterior, fizemos autorização da nossa aplicação com Implicit Code Flow no Asp.Net 4.5, agora, vamos trazer o mesmo para o Asp.Net Core 1.

### Preparando o Authoriztion Server para o novo Client

Da mesma forma que no anterior, vamos na nosso InMemoryClient e lá vamos criar um novo Client para o Implict Flow:

```csharp
new Client
{
    ClientId = "teste_implict",
    ClientName = "Teste OAuth2 Implicit",
    ClientSecrets = new List<Secret>
    {
        new Secret("secret".Sha256())
    },
    Flow = Flows.Implicit,
    
    AllowedScopes  = new List<string> 
    {
            StandardScopes.OpenId.Name,
            StandardScopes.Profile.Name,
            "read" 
    },
    RedirectUris = new List<string>
    {
        "http://localhost:5001/signin-oidc"
        "http://localhost:5001"
    },
    Enabled = true
}
```

E pronto! Nosso Authorization Server está pronto para o novo Client.

### Preparando o Client

Vamos preparar nosso _Client_ para autorização no asp.net Core 1.

Para isso criei uma aplicação Web Sem Authorization com o Yo.

O próximo passo é colocarmos nosso atributo __Authorize__ em nossa View Contact no HomeController:

![passo1](http://rafael-miceli.com.br/ico/OAuth2-Implementando-Implicit-Grant-Asp-Net-Core/passo1.png)

Se tentarmos chamar a pagina _Contact_ vamos receber um _401_.

![passo2](http://rafael-miceli.com.br/ico/OAuth2-Implementando-Implicit-Grant-Asp-Net-Core/passo2.png)

Excelente! Temos agora que configurar para usarmos nosso Authorization Server para autorizar nosso Client.

Para isso vamos precisar adicionar as seguintes dependências em nosso __project.json__:

    "Microsoft.AspNet.Authentication.Cookies": "1.0.0-rc1-final",
    "Microsoft.AspNet.Authentication.OpenIdConnect": "1.0.0-rc1-final",
    "IdentityModel": "2.0.0-alpha2"

Após termos adicionado as dependências precisamos adicionar o seguinte código no método __Configure__ do nosso __Startup.cs__:

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env, ILoggerFactory loggerFactory)
{
    loggerFactory.AddConsole(Configuration.GetSection("Logging"));
    loggerFactory.AddDebug();

    if (env.IsDevelopment())
    {
        app.UseDeveloperExceptionPage();
    }
    else
    {
        app.UseExceptionHandler("/Home/Error");
    }

    app.UseIISPlatformHandler();

    app.UseStaticFiles();
    
    app.UseCookieAuthentication(options =>
    {
        options.AuthenticationScheme = "cookies";
        options.AutomaticAuthenticate = true;
    });

    JwtSecurityTokenHandler.DefaultInboundClaimTypeMap.Clear();
    app.UseOpenIdConnectAuthentication(options =>
    {
        options.AuthenticationScheme = "oidc";
        options.SignInScheme = "cookies";
        options.AutomaticChallenge = true;

        options.Authority = "http://localhost:5000/";                
        options.RequireHttpsMetadata = false;

        options.ClientId = "teste_implict";
        options.ResponseType = "id_token token";

        options.Scope.Add("profile");
        options.Scope.Add("openid");
        options.Scope.Add("read");
    });

    app.UseMvc(routes =>
    {
        routes.MapRoute(
            name: "default",
            template: "{controller=Home}/{action=Index}/{id?}");
    });
}
```
Com essa configuração já podemos tentar acessar nossa página Contact que seremos redirecionados para nosso Login:

![passo3](http://rafael-miceli.com.br/ico/OAuth2-Implementando-Implicit-Grant-Asp-Net-Core/passo3.png)

![passo4](http://rafael-miceli.com.br/ico/OAuth2-Implementando-Implicit-Grant-Asp-Net-Core/passo4.png)

E ai está um Implicit Flow básico com Asp.Net Core.


