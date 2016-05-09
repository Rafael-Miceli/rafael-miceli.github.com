---
author: Rafael Miceli
comments: true
layout: default 
category : OAuth 
title: "Identity Server 4 - Logout com Redirect" 
date: 2016-05-09 09:30:00
---

Em um post anterior mostrei como preparar um logout da sua aplicação com o IdentityServer 4.

Mas no post anterior o logout tinha um fluxo que não é legal para o usuário, vamos fazer que após clicar em logout em nossa aplicação __client__ o usuário não precise fazer mais um logout.

### Logout mais fácil

Para agora temos nosso logout retornando diretamente para nossa aplicação neste novo IdentityServer é muito mais simples.

No IdentityServer 3 para isso precisaríamos buscar o *id_token* para conseguir isso, além de implementar bastante código no startup.cs do nosso client.

No identityserver 4 é muito mais simples, uma vez que o id_token já vem no nosso request.

### Implementando melhoria no logout

Vamos em nossa classe Clients do nosso Authorization Server, e lá vamos adicionar nosso PostLogoutRedurectUri em nosso idclient:

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
    },
    PostLogoutRedirectUris = new List<string>
    {
        "http://localhost:5001",
    },
    Enabled = true
}    
```

Agora, vamos em nossa aplicação Client no nosso startup.cs, e vamos também adicionar o PostLogoutRedurectUri:

```csharp
app.UseOpenIdConnectAuthentication(options =>
{
    options.AuthenticationScheme = "oidc";
    options.SignInScheme = "cookies";
    options.AutomaticChallenge = true;

    options.Authority = "http://localhost:5000/";                
    options.RequireHttpsMetadata = false;

    options.ClientId = "teste_implict";
    options.ResponseType = "id_token token";
    
    options.PostLogoutRedirectUri = "http://localhost:5001/";

    options.Scope.Add("profile");
    options.Scope.Add("openid");
    options.Scope.Add("read");
});
```

Excelente! Próximo passo, em nosso Client, vamos adicionar nosso idclient quando chamar nossa url de logout

```csharp
[Authorize]
public async Task<IActionResult> Logout()
{
    await HttpContext.Authentication.SignOutAsync("cookies");
    return Redirect("http://localhost:5000/ui/logout?id=teste_implict");
}
```

Por fim, em nosso Authorization Server no _LogoutController_ na action _Submit_ vamos sobre escrever a action com o seguinte código

```csharp
[HttpPost(Constants.RoutePaths.Logout)]
[ValidateAntiForgeryToken]
public async Task<IActionResult> Submit(string signOutId)
{
    await HttpContext.Authentication.SignOutAsync(Constants.PrimaryAuthenticationType);

    HttpContext.User = new ClaimsPrincipal(new ClaimsIdentity());

    var vm = new LoggedOutViewModel();
    
    vm.ReturnInfo = GetClientReturnInfo(signOutId);
    
    return Redirect(vm.ReturnInfo.Uri);
}

private ClientReturnInfo GetClientReturnInfo(string signOutId)
{
    var client = Clients.Get().FirstOrDefault(c => c.ClientId == signOutId);

    var clientReturnInfo = new ClientReturnInfo
    {
        ClientId = client.ClientId,
        Uri = client.PostLogoutRedirectUris.First() 
    };

    return clientReturnInfo;
}
```

Se executar a aplicação vamos ter um fluxo de logout padrão.




