---
author: Rafael Miceli
comments: true
layout: default 
category : OAuth 
title: "Identity Server 4 - Logout" 
date: 2016-05-03 09:30:00
---

Uma vez que temos nossa aplicação usando nosso Authorization Server para acessar áreas privadas da nossa aplicação, vamos implementar a funcionalidade para realizar logout do mesmo.

Lembrando que todos os código de exemplo deste post estão em meu [Github](https://github.com/Rafael-Miceli/Blog-Codes/tree/master/IdeiasComAzeite/IdentityServer4-Sample/AuthorizationServer)

### Logout no Client

Em nossa aplicação Client vamos criar um botão de logout no menu que se encontra no caminho *Views/Shared/_Layout.cshtml*:

![passo1](http://rafael-miceli.com.br/ico/Identity-Server-4-Logout/passo1.png)

Excelente! Agora nos logamos nosso botão de logout aparece, mas ainda não faz nada. Vamos criar uma Action para nosso Logout em nosso __HomeController__, e nessa Action vamos implementar o seguinte código:

```
[Authorize]
public async Task<IActionResult> Logout()
{
    await HttpContext.Authentication.SignOutAsync("cookies");
    return Redirect("~/");
}
```

Muito bom! Mas... Se notar, ao realizarmos o logout, tudo parece ter ocorrido ok. Nossa opção de Logout do menu some, tudo certo... Porém, quando clicamos na área restrita (Contact nesse caso) repare que não nos pede o login novamente! E a opção Logout volta aparecer.

Explicando melhor, o trecho do código:

```csharp
await HttpContext.Authentication.SignOutAsync("cookies");
```

Apenas realiza Logout em nossa aplicação _CLIENT_, mas em nosso Authorization Server nós continuamos logado!

Então, como resolver isso? Vou mostrar uma maneira, mas vou melhorar ela depois.

### Logout no Authorization Server

Se quisermos que nossa aplicação ao realizar Logout nela, também realize em nosso Auth Server a forma mais rápida é no _Redirect_ da nossa Action de Logout apontarmos para o logout em nosso Authorization Server (Esse era o comportamento padrão no Identity Server 3).

Vamos fazer isso mudando nossa Action de Logout da seguinte maneira:

```csharp
[Authorize]
public async Task<IActionResult> Logout()
{
    await HttpContext.Authentication.SignOutAsync("cookies");
    return Redirect("http://localhost:5000/ui/logout");
}
```

Excelente! Nossa aplicação realiza Logout, e depois somo redirecionados a realizar o Logout no Indetity Server, mas ainda não somos redirecionados a nossa aplicação. Em nosso próximo post vamos melhorar isso mais.




