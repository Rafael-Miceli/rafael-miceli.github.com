---
author: Rafael Miceli
comments: true
layout: default 
category : aspnetcore 
title: "Https com Kestrel e IIS no Windows" 
date: 2016-04-07 19:00:00
---
 
Em muitas situações vamos precisar que nossa aplicação web esteja rodando em cima de https.

Neste artigo vamos ver como configurar isso no ambiente Windows para o __kestrel__ e o __iis__. Em um próximo veremos no Ubuntu.

### Gerando um certificado

Para usar https com kestrel precisamos gerar um .pfx (PKCS #12), ou seja, precisamos criar um certificado para a criptografia da comunicação das nosss páginas web _(em um outro artigo explico melhor sobre, PKCS #12, X.509...)_.

Para criarmos o certificado faremos os seguintes passos (estou fazendo isso em um Windows 10, então podem haver pequenas variações para outras versões):

Abra o seu __gerenciador de certificados e criptografia__

![passo1](http://rafael-miceli.com.br/ico/Https-Com-Kestrel-iis-Windows/passo1.png)

Avance

![passo2](http://rafael-miceli.com.br/ico/Https-Com-Kestrel-iis-Windows/passo2.png)

Selecione __Criar um novo certificado__

![passo3](http://rafael-miceli.com.br/ico/Https-Com-Kestrel-iis-Windows/passo3.png)

Selecione a primeira opção e Avance

![passo4](http://rafael-miceli.com.br/ico/Https-Com-Kestrel-iis-Windows/passo4.png)

Escolha __Fazer backup do certificado e chave agora__ e no __Local do backup__ coloque o endereço aonde vai ser criado seu .pfx e o nome do seu arquivo.

Também escolha uma senha __E NÃO SE ESQUEÇA__ para seu certificado.

![passo5](http://rafael-miceli.com.br/ico/Https-Com-Kestrel-iis-Windows/passo5.png)

Por fim escolha __Atualizar os arquivos criptografados mais tarde__

E pronto, temos nossa chave no local que você definiu.   

### Configurando a aplicação

Após gerarmos nosso certificado precisamos agora ler o mesmo em nossa aplicação.

Para isso, copie o certificado para a raiz do seu projeto (aonde fica o project.json)

Em nosso startup.cs, no método configure, adicione o seguinte código:

    var certFile = appEnv.ApplicationBasePath + "\\teste.pfx";
    var certificate = new X509Certificate2(certFile, "teste");

    app.Use(ChangeContextToHttps);   
    app.UseKestrelHttps(certificate);
    
Vamos também fazer que todos os requests em nossa aplicação caiam para o https adicionando um método chamado __ChangeContextToHttps__ que nossa __IApplicationBuilder__ esta usando

```csharp
private static RequestDelegate ChangeContextToHttps(RequestDelegate next)
{
    return async context =>
    {
        context.Request.Scheme = "https";
        await next(context);
    };
}
```

Vamos precisar também adicionar uma nova dependência ao kestrel.https e o comando __web__ em nosso project.json:       

![passo6](http://rafael-miceli.com.br/ico/Https-Com-Kestrel-iis-Windows/passo6.png)

> Reparem que não estamos usando o dnxcore50, até podemos, mas a dependência ao kestrel.https tem de ser no dnx451 uma vez que este pacote não é suportado pelo RC1, mas para o RC2 estará!

Excelente!

Vamos testar executando nosso:

    dnx web 

![passo7](http://rafael-miceli.com.br/ico/Https-Com-Kestrel-iis-Windows/passo7.png)

### Configurando IIS

Para configurar nossa aplicação para https no IIS é ainda mais simples.

Apenas precisamos ir no arquivo launchSettings.json dentro da pasta _Properties_ (Se você estiver no visual studio vai ter o simbolo de uma ferramenta)

Dentro do objeto listSettings em iisExpress mudamos a __applicationUrl__ inicial para uma url com uma porta 443 e a __sslPort__ para a mesma porta usada no __applicationUrl__

![passo8](http://rafael-miceli.com.br/ico/Https-Com-Kestrel-iis-Windows/passo8.png)

Dessa vez para testar, vamos clicar no botão IIS express em nosso Visual Studio Community.

![passo9](http://rafael-miceli.com.br/ico/Https-Com-Kestrel-iis-Windows/passo9.png)

Em um próximo artigo veremos o mesmo para Ubuntu.
