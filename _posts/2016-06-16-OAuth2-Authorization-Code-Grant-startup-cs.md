---
author: Rafael Miceli
comments: true
layout: default 
category : Oauth
title: "OAuth2 - Authorization Code Grant no startup.cs" 
date: 2016-06-16 10:00:00
---

No [post anterior](http://rafael-miceli.com.br/oauth/2016/06/09/Implementando-Authorization-Code-Grant-IdentityServer3.html) implementamos de forma crua o Authorization Code Grant.

Mas a forma que fizemos isso foi que ao acessar a Index da nossa aplicação (apenas nesse momento), seria feito o redirecionamento para o nosso Authorization Server.

Vamos fazer como uma aplicação padrão. Apenas quando acessar uma área não autorizada somos redirecionados. 

### Implementando Authorization Code Grant no startup.cs

Vamos no passo a passo

### Passo 1

Vamos configurar nosso __OpenIdConnectAuthenticationOptions__ para realizar nosso flow em nosso startup.cs

Para isso vamos deixar nosso startup.cs da seguinte forma:

```csharp
public class Startup
{
    public void Configuration(IAppBuilder app)
    {
        JwtSecurityTokenHandler.InboundClaimTypeMap = new Dictionary<string, string>();

        app.UseCookieAuthentication(new CookieAuthenticationOptions
        {
            AuthenticationType = "cookies"
        });

        app.UseOpenIdConnectAuthentication(new OpenIdConnectAuthenticationOptions
        {
            ClientId = "teste_code",
            Authority = "http://localhost:54734",
            RedirectUri = "http://localhost:60758/",
            ResponseType = "code",
            Scope = "openid profile",
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

                    notification.AuthenticationTicket = new AuthenticationTicket(identity, notification.AuthenticationTicket.Properties);
                },

                RedirectToIdentityProvider = notification =>
                {
                    if (notification.ProtocolMessage.RequestType != OpenIdConnectRequestType.LogoutRequest)
                    {
                        return Task.FromResult(0);
                    }

                    notification.ProtocolMessage.IdTokenHint =
                        notification.OwinContext.Authentication.User.FindFirst("id_token").Value;

                    return Task.FromResult(0);
                }
            }
        });

        
    }
}
```

Explicando o codigo:

A primeira etapa de configurar _ClientId_, _Authority_ até a propriedade _SignInAsAuthenticationType_ já expliquei em um [post anterior](http://rafael-miceli.com.br/oauth/2016/04/25/OAuth2-Implementando-Implicit-Grant-Asp-Net-4-5.html).

Nesta parte o principal é nosso _Notifications_, que vai instanciar __OpenIdConnectAuthenticationNotifications__ e vamos popular sua propriedade _AuthorizationCodeReceived_.

Em _AuthorizationCodeReceived_ nós estamos fazendo o equivalente a nosso __Callback__ do [post anterior](http://rafael-miceli.com.br/oauth/2016/06/09/Implementando-Authorization-Code-Grant-IdentityServer3.html).

No post anterior ao chamar o endpoint _Authorize_ passamos um endereço de __Callback__. Dessa vez não precisamos do __Callback__ que criamos anteriormente, uma vez que nosso _AuthorizationCodeReceived_ faz isso por nós.

Nele chamamos nosso endpoint _Token_ (assim como no anterior), e passamos o code que recebemos em nosso parâmetro _notification_.

Após, guardamos nosso Identity em uma variável _identity_ e adicionamos os Claims *id_token* e *access_token*.

Por fim setamos nosso novo _AuthenticationTicket_ como nosso _identity_.

### Passo 2

Removemos o __Redirect__ para nosso endpoint _Authorize_ do nosso Authorization Server da nossa action __Index__ e excluimos o método de __Callback__.

Pronto!

Temos nosso Authorization Code Grant configurado.

Mas... Se você for perspicaz vai perceber que ai possui um erro. No post seguinte tem a correção. 


