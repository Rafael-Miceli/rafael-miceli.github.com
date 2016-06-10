---
author: Rafael Miceli
comments: true
layout: default 
category : Oauth
title: "Implementando Authorization Code Grant com IdentityServer 3" 
date: 2016-06-09 23:40:00
---

Continuando com a (saga) implementação dos flows de nossa aplicação utilizando Oauth. O último que implementamos foi o [implicit flow](link) que é muito recomendado para __Clients__ que você não quer que possam dar um refresh no token (geralmente aplicações mobile).

Mas e quando precisamos dar um refresh em nosso **token**?

### Implementando Authorization Code Grant

Já expliquei em um post anterior quais as diferenças entre os redirect flows: implicit e acg, então vamos logo com a mão na massa.

Vamos no passa a passo para implementar nosso Authorization Code Grant.

Passo 1.

Vá na classe InMemoryClients do AuthorizationServer e adicione um novo Client com o ClientId **teste_code**, e com o fluxo __AuthorizationCode__.

```csharp
new Client
{
    ClientId = "teste_code",
    ClientName = "Teste OAuth2 Authorization Code",
    ClientSecrets = new List<Secret>
    {
        new Secret("secret".Sha256())
    },
    Flow = Flows.AuthorizationCode,

    AllowedScopes  = new List<string>
    {
            Constants.StandardScopes.OpenId,
            Constants.StandardScopes.Profile,
            Constants.StandardScopes.OfflineAccess,
            "read"
    },
    Enabled = true
}
```

Passo 2 

Crie uma nova aplicação MVC sem autenticação e no HomeController na action __Contact__ coloque o atributo __Authorize__.

Se rodar a nova aplicação e tentar acessar a página contact vamos ter um 401!

Passo 3

Adicione o seguinte código na action __Index__:

```csharp
public ActionResult Index()
{
    if (User.Identity.IsAuthenticated)
    {
        return View();
    }

    var url = "http://localhost:54734/connect/authorize" +
        "?client_id=teste_code" +
        "&redirect_uri=http://localhost:60758/Home/AuthCallBack" +
        "&response_type=code" +
        "&scope=openid+profile" +
        "&response_mode=form_post";

    return Redirect(url);

}
```

Passo 4

Como viram, para nosso **redirect_uri** da url que vamos chamar precisamos criar uma nova action chamada __AuthCallBack__, e ela fica da seguinte maneira:

```csharp
public ActionResult AuthCallBack(string code, string state, string error)
{
    var tokenUrl = "http://localhost:54734/connect/token";

    var client = new OAuth2Client(new Uri(tokenUrl), "teste_code", "secret");

    var requestResult = client.RequestAccessTokenCode(code, new Uri("http://localhost:60758/Home/AuthCallBack"));

    var claims = new[]
    {
        new Claim("access_token", requestResult.AccessToken)
    };

    var identity = new ClaimsIdentity(claims, "cookies");
    
    Request.GetOwinContext().Authentication.SignIn(identity); 

    return Redirect("/");
}
```

Explicando melhor o que estamos fazendo aqui. 

Por convenção quando chamamos o Endpoint __authorize__ de nosso AuthorizationServer recebemos um Callback com os parâmetros: _code, state e error_.

Neste Callback pedimos nosso **access_token** pelo _tokenUrl_.

Criamos o objeto client (que instância a classe OAuth2Client), passando como parâmetro o _tokenUrl_, o clientId e o _secret_, e fazemos um request com o code que recebemos no Callback.

Com o resultado desse request recebemos nosso **access_token** e adicionamos o mesmo com um Identity em nosso OwinContext.

Passo 5

Vamos voltar em nosso novo Client e atualizar as __RedirectUris__ para a URL de nossa nova aplicação e o Callback também.

Passo 6 

Rodamos a aplicação e testamos.

No próximo post vamos configurar melhor nosso Authorizaion Code Grant

