---
author: Rafael Miceli
comments: true
layout: default 
category : Oauth
title: "OAuth2 - Refresh Token" 
date: 2016-06-18 10:50:00
---

No [post anterior](http://rafael-miceli.com.br/oauth/2016/06/17/OAuth2-Hybrid-Flow.html) implementamos o Hybrid Flow, agora que estamos em nosso Hybrid Flow podemos realizar um refresh em nosso token _(Authorization Code Grant também pode)_, dessa forma nosso Client não vai ter problemas de expiração de token e o usuário vai poder ter um **access_token** "permanente". Vamos implementar como podemos dar um refresh em nosso token.

### Recebendo Refresh Token no Claims

Vamos começar configurando nosso Authorization Server:

Primeiro temos de ir em nossa classe InMemoryScopes. Nela vamos adicionar para ela retornar o _StandarScopes.OfflineAccess_.

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

Segundo, temos que ir em nossa classe InMemoryClients. 

Em nosso Client **teste_code** vamos adicionar um novo _AllowedScopes_ que vai ser o _Constants.StandardScopes.OfflineAccess_.

```csharp
new Client
{
    ClientId = "teste_code",
    ClientName = "Teste OAuth2 Authorization Code",
    ClientSecrets = new List<Secret>
    {
        new Secret("secret".Sha256())
    },
    Flow = Flows.Hybrid,

    AllowedScopes  = new List<string>
    {
            Constants.StandardScopes.OpenId,
            Constants.StandardScopes.Profile,
            Constants.StandardScopes.OfflineAccess,
            "read"
    },
    RedirectUris = new List<string>
    {
        "http://localhost:60758/",
        "http://localhost:60758/Home/AuthCallBack"
    },
    Enabled = true
}
```

E isso é o que precisamos configurar em nosso Authorization Server. Agora em nosso Client:

Vamos no Startup.cs. Na propriedade _Scope_ do nosso __OpenIdConnectAuthenticationOptions__ vamos adicionar o  *offline_access*.

Também na propriedade _AuthorizationCodeReceived_ do nosso __OpenIdConnectAuthenticationNotifications__ vamos adicionar o Claim do nosso *refresh_token*.

```csharp
app.UseOpenIdConnectAuthentication(new OpenIdConnectAuthenticationOptions
{
    ClientId = "teste_code",
    Authority = "http://localhost:54734",
    RedirectUri = "http://localhost:60758/",
    ResponseType = "code id_token",
    Scope = "openid profile offline_access",
    PostLogoutRedirectUri = "http://localhost:60758/",
    SignInAsAuthenticationType = "cookies",


    Notifications = new OpenIdConnectAuthenticationNotifications
    {
        AuthorizationCodeReceived = async notification =>
        {
            var requestResponse = await OidcClient.CallTokenEndpointAsync(
                new Uri("http://localhost:54734/connect/token"),
                new Uri("http://localhost:60758/"),
                notification.Code,
                "teste_code",
                "secret");

            var identity = notification.AuthenticationTicket.Identity;

            identity.AddClaim(new Claim("id_token", requestResponse.IdentityToken));
            identity.AddClaim(new Claim("access_token", requestResponse.AccessToken));
            identity.AddClaim(new Claim("refresh_token", requestResponse.RefreshToken));

            notification.AuthenticationTicket = new AuthenticationTicket(identity, notification.AuthenticationTicket.Properties);
        }
```

Excelente!

### Implementando o Refresh do Token

Agora vamos em nosso HomeController do nosso _Hybrid_ Client e vamos colocar esta Action:

```csharp
public ActionResult RefreshAccessToken()
{
    var claimsPrincipal = User as ClaimsPrincipal;

    //client para fazr request do Novo token com o refresh_toke atual
    var client = new OAuth2Client(new Uri("http://localhost:54734/connect/token"),
        "teste_code", "secret");

    var requestResponse = client.RequestAccessTokenRefreshToken(
        claimsPrincipal.FindFirst("refresh_token").Value);

    //Usar AuthenticationManager para substituir antigo access_token pelo novo access_token
    var manager = HttpContext.GetOwinContext().Authentication;

    var refreshedIdentity = new ClaimsIdentity(User.Identity);

    refreshedIdentity.RemoveClaim(refreshedIdentity.FindFirst("access_token"));
    refreshedIdentity.RemoveClaim(refreshedIdentity.FindFirst("refresh_token"));

    refreshedIdentity.AddClaim(new Claim("access_token",
        requestResponse.AccessToken));

    refreshedIdentity.AddClaim(new Claim("refresh_token",
        requestResponse.RefreshToken));

    //Atualizar o Cookie
    manager.AuthenticationResponseGrant =
        new AuthenticationResponseGrant(new ClaimsPrincipal(refreshedIdentity),
        new AuthenticationProperties { IsPersistent = true });

    return Redirect("/Index");
}
```

Nesta Action estamos realizando o Refresh Manual do nosso token, explicando melhor.

Primeiro vamos buscar nosso **ClaimsPrincipal** para termos acesso ao nosso **refresh_token**

Após isso, vamos realizar um request com nosso **refresh_token** para pedir um novo **access_token** usando o __OAuth2Client__.

Próximo passo vamos pegar nosso __AuthenticationManager__ e com ele vamos atualizar nossos __Claims __ do **access_token** e **refresh_token**.

E por fim atualizamos o Cookie.

Após isso temos nossa Action de refresh completa.

Agora temos implementado o Hybrid Flow podendo realizar refresh do seu access_token.


