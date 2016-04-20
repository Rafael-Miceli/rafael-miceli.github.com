---
author: Rafael Miceli
comments: true
layout: default 
category : OAuth 
title: "OAuth2 - Implementando AuthorizationServer com IdentityServer" 
date: 2016-04-20 09:30:00
---

Vamos criar nosso authorization server para gerenciar o acesso a áreas privadas em nosso sistema.

Para isso vamos usar o [__IdentityServer__](https://github.com/IdentityServer/IdentityServer3)

> Lembrando que todo código visto aqui está em meu [Github](https://github.com/Rafael-Miceli/Blog-Codes/tree/master/IdeiasComAzeite/AutoFixtureSample/Escola)

### IdentityServer 

O IndentityServer é um framework completo com as especificações do OAuth2 implementadas. Isso nos adianta muita coisa, além de ser um framework criado por experts da segurança.

### Criando Authorization Server

Vamos no passo a passo para termos nosso Authorization Server.

1 - Vamos criar um MVC 5 empty web application.

2 - Após isso vamos adicionar nossa _Startup.cs_ em nosso projeto, para isso clicamos com o botão direito em nosso projeto, Add New Item e selecionamos _OWIN Startup class_

![passo1](http://rafael-miceli.com.br/ico/OAuth2-Implementando-AuthorizationServer-IdentityServer/passo1.png)

3 - Agora, vamos abrir nosso _Package Manager Console_ e instalamos o Identity server.

    Install-Package IdentityServer3
   
Também temos de instalar o OWIN host:
 
    Install-Package Microsoft.Owin.Host.SystemWeb

4 - Próximo passo, vamos configurar o IdentityServer em nosso Startup.cs.

Para isso adicionamos o seguinte código no método configuration.

    var options = new IdentityServerOptions();

    app.UseIdentityServer(options);


5 - Com essa variavel options nós não temos mais erros de Build, mas, precisamos configurar algumas propriedades do nosso options para que funcione corretamente, e para isso começamos configurando nosso certificado de assinatura do nosso token.

Primeiro, use este certificado em seu web.config para assinatura.

> Lembrando, este certificado não deve ser usado em produção, é apenas para estudo

    <add key="SigningCertificate" value="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"/>
    <add key="SigningCertificatePassword" value="senha"/>

Aproveitando que estamos no web.config, o IndentityServer também precisa de mais um elemento para funcionar apropriadamente:

    <system.webServer>
        <modules runAllManagedModulesForAllRequests="true"/>
    </system.webServer> 

Agora, usamos este certificado em nosso options em nosso _Staturp.cs_

    var certificado = Convert.FromBase64String(ConfigurationManager.AppSettings["SigningCertificate"]);

    var options = new IdentityServerOptions
    {
        SigningCertificate = new X509Certificate2(certificado, ConfigurationManager.AppSettings["SigningCertificatePassword"]),
    };

    app.UseIdentityServer(options);


6 - Não vamos forçar usar SSL uma vez que vamos trabalhar local apenas

> Novamente, sempre use SSL em produção

![passo2](http://rafael-miceli.com.br/ico/OAuth2-Implementando-AuthorizationServer-IdentityServer/passo2.png)

7 - O próximo passo é configurar quais scopes nosso Authorization Server vai trabalhar. Podemos usar os scopes padrões do protocolo OAuth2 e podemos também criar scopes customizados. 

Vamos criar uma classe estática separada com a lista de nossos scopes

![passo3](http://rafael-miceli.com.br/ico/OAuth2-Implementando-AuthorizationServer-IdentityServer/passo3.png)

Vamos usar os seguintes scopes.

```csharp
public class InMemoryScopes
{
    public static IEnumerable<Scope> GetScopes()
    {
        return new[]
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
            },
        };
    }
}
```

Agora, vamos adicionar os scopes que acabamos de definir em nosso IdentityServer, e após adicionar nosso _Configuration_ fica dessa maneira:

```csharp
public void Configuration(IAppBuilder app)
{
    var factory = new IdentityServerServiceFactory().UseInMemoryScopes(InMemoryScopes.GetScopes());
    var certificado = Convert.FromBase64String(ConfigurationManager.AppSettings["SigningCertificate"]);

    var options = new IdentityServerOptions
    {
        SigningCertificate = new X509Certificate2(certificado, ConfigurationManager.AppSettings["SigningCertificatePassword"]),
        RequireSsl = false,
        Factory = factory
    };

    app.UseIdentityServer(options);
}
```

8 - Também precisamos configurar quais clients podem se autorizar com nosso IdentityServer, para isso criamos uma lista estática de clients, como em scopes.

```csharp
public class InMemoryClients
{
    public static IEnumerable<Client> GetClients()
    {
        return new[]
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
                AllowedScopes  = new List<string> { Constants.StandardScopes.OpenId },
                Enabled = true
            }
        };
    }
}
```

Atualizar Startup.cs

```csharp
var factory = new IdentityServerServiceFactory()
        .UseInMemoryScopes(InMemoryScopes.GetScopes())
        .UseInMemoryClients(InMemoryClients.GetClients());
```

Percecebam que na classe __InMemoryClients__ no nosso Cliente __teste__ estou usando o Flow ResourceOwner (explicação em um próximo post).

9 - Por fim, vamos criar um usuário em memoria para testar nosso login, similar a como criamos scopes e clients.

```csharp
public class InMemoryUsers
{
    public static List<InMemoryUser> GetUsers()
    {
        return new List<InMemoryUser>
        {
            new InMemoryUser
            {
                Subject = "GUIDS",
                Username = "rafael.miceli@oauth.com",
                Password = "123456",
                Claims = new []
                {
                    new Claim(Constants.ClaimTypes.Name, "Rafael Miceli")
                }
            }
        };
    }
}
```

Atualizar Startup.cs

```csharp
var factory = new IdentityServerServiceFactory()
        .UseInMemoryScopes(InMemoryScopes.GetScopes())
        .UseInMemoryClients(InMemoryClients.GetClients())
        .UseInMemoryUsers(InMemoryUsers.GetUsers());
```

10 - Apesar de nosso passo 9 ter dito, _Por fim_, no passo 10 precisamos testar para termos certeza de que está funcionando.

Para isso, vamos rodar nosso Authorization Server e vamos usar o Postman para realizar o seguinte post:

![passo4](http://rafael-miceli.com.br/ico/OAuth2-Implementando-AuthorizationServer-IdentityServer/passo4.png) 

O resultado que esperamos é nosso token no response:

![passo5](http://rafael-miceli.com.br/ico/OAuth2-Implementando-AuthorizationServer-IdentityServer/passo5.png)

E assim, fizemos uma configuração básica de um Authorization Server. 

Em um próximo post vamos começar a usar nosso Authorization Server Com nossa API. 
