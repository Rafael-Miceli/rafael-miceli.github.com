---
author: Rafael Miceli
comments: true
layout: default 
category : OAuth 
title: "OAuth2 - Implementando Implicit Grant Asp.Net 4.5" 
date: 2016-04-25 09:30:00
---

Neste post vamos utilizar nosso Authorization Server que criamos para autorizar uma aplicação escola que está em meu [Github](https://github.com/Rafael-Miceli/Blog-Codes/tree/master/IdeiasComAzeite/AutoFixtureSample/Escola):

### Preparando o Authoriztion Server para o novo Client

Vamos preparar nosso Authoriztion Server para receber um novo _Client_ realizando o fluxo __Implicit Grant__ para autorização do mesmo.

Para configurar nosso Authorization Server é muito simples, temos apenas de adicionar mais um Client na nossa classe __InMemoryClients__:

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
        "http://localhost:50334"
    },
    Enabled = true
}
```

Neste novo __Client__ de Id **teste_implicit** vamos utilizar o _Implicit_ flow e permitir dois novos _Scopes_ um _Standard_: __Profile__, e outro _Custom_: __read__.

Excelente! Nosso Authorization Server está pronto para o novo Client.

### Preparando o Client

Vamos preparar nosso _Client_ para autorização.

Em nosso "MateriaContoller" vamos adicionar o atributo __Authorize__ em sua classe.

![passo1](http://rafael-miceli.com.br/ico/OAuth2-Implementando-Implicit-Grant-Asp-Net-4-5/passo1.png)

Se executarmos nossa aplicação neste momento e tentarmos chamar a API Materia vamos receber um código 401.

![passo2](http://rafael-miceli.com.br/ico/OAuth2-Implementando-Implicit-Grant-Asp-Net-4-5/passo2.png)

Excelente! Agora precisamos adicionar algumas dependências em nossa aplicação usando nosso _Package Manager Console_

As dependências são:

    Microsoft.Owin.Security.OpenIdConnect
    Microsoft.Owin.Security.Cookies

Próximo passo, precisamos configurar nosso _Startup.cs_:

	> Caso você não possua uma classe Startup.cs crie a mesma clicando com o botão direito em cima do projeto, escolhendo Add... Depois, New Item... E por fim Selecione OWIN Startup File.

Com o package _Cookies_ que instalamos vamos configura-lo da seguinte forma:

```csharp
app.UseCookieAuthentication(new CookieAuthenticationOptions
{
    AuthenticationType = "cookies"
});
``` 

Neste trecho estamos dizendo que queremos que o tipo de autenticação que vamos usar será _cookies_ (existem outros tipos como: )

Próximo passo vamos configurar nosso OpenIdConnect:

```csharp
app.UseOpenIdConnectAuthentication(new OpenIdConnectAuthenticationOptions
{
    ClientId = "teste_implict",
    Authority = "http://localhost:54733/",
    RedirectUri = "http://localhost:50334",
    ResponseType = "id_token token",
    Scope = "openid profile",
    SignInAsAuthenticationType = "cookies"               
});
```

Nele informamos que somos o _Client_ **teste_implicit** pelo __ClientId__,  nosso _Authority_ (Autorization Server) fica no endereço informado. Nosso _ResponseType_ são: *id_token  e token*. 

Definimos os Scopes que queremos e por fim que tipo de autenticação estamos usando em _SignInAsAuthenticationType_.

E pronto!

Temos o básico de nosso _Client_ configurado.

Se realizarmos uma chamada _Get_ ao __MateriaContoller__ neste momento seremos redirecionados para nosso Authorization Server para login:

![passo3](http://rafael-miceli.com.br/ico/OAuth2-Implementando-Implicit-Grant-Asp-Net-4-5/passo3.png)

E ao nos logarmos conseguimos acesso ao recurso.

Em um próximo post vamos melhorar nosso código para Login.
