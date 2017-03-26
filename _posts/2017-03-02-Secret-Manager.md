---
author: Rafael Miceli
comments: true
layout: default 
category : aspnetcore
title: "Secret Manager no .Net Core" 
date: 2017-03-02 21:30:00
---

No [post anterior](http://rafael-miceli.com.br/aspnetcore/2017/02/28/Multiplos-Environments-AspNet-Core.html) vimos como preparar nossa aplicação para configurações de diferentes ambiente com o AppSettings.json

Mas e informações sensíveis em nosso código como podemos manusear ela? Como nossa Connection String para o Banco de dados.

Geralmente em ambiente de desenvolvimento o melhor é que seja sempre um banco de dados local e que todos os serviços que você necessite estejam local. 
Pois caso a rede caia seu trabalho não precisa parar. Mas sabemos que as vezes isso não é possível.

Caso você precise por exemplo fazer uma comunicação com um banco de dados na sua rede podemos usar o novo Secret Manager do .Net Core.

>Deixando bem claro que o Secret Manager não criptografa seus dados sensíveis, ele apenas armazena eles em um .json separado para que não haja o risco de você commitar dados sensíveis no seu controle de versionamento. Ele também não é recomendado para armazenar seus dados sensíveis em outros ambientes além de desenvolvimento, para isso dê preferência a variáveis de ambiente.

### Configurando o Secret Manager

Para isso primeiro precisamos adicionar o pacote do Secret Manager ao `tools` do nosso projeto 

```javascript
"tools":{
    "Microsoft.Extensions.SecretManager.Tools": "1.1.0-preview4-final"
}
```

Feito isso fazemos um `dotnet restore` e vemos se nossa tools foi instalada executando na raiz do nosso projeto o comando: 

`dotnet user-secrets -h`

Se funcionou a próxima etapa é adicionarmos em nosso projetc.json uma key __userSecretsId__.
 
```javascript
{
 "userSecretsId": "aspnet-MyProject-7e92740b-cc82-4aba-9a38-789758d9d8ef",

 "dependencies": {     
     "yadayada": ""
 }
```

O valor da key __userSecretsId__ é apenas uma string para identificar o seu projeto e os UserSecrets dele, por isso é recomendado que você use um guid para colocar como o Value.

Você pode gerar um guid em um site de [guid generator](https://www.guidgenerator.com/)

Após isso podemos executar o seguinte comando na raiz do projeto para adicionar nosso Secret de conexão de banco:

`dotnet user-secrets set ConnectionStrings:DefaultConnection "ConexaoDoBanco"` 

Para ver os Screts que setamos para esse projeto basta executar os seguinte comando:

`dotnet user-secrets list`

Agora precisamos adicionar o pacote para acessar o Secret em nossa aplicação:

```javascript
"dependencies": {    
    "Microsoft.Extensions.Configuration.UserSecrets": "1.1.0",
    "yadayada": ""
 }
```

Por fim adicionamos o seguinte codigo no construtor do nosso Startup.cs após instanciar o __ConfigurationBuilder()__:

```csharp
if (env.IsDevelopment())
    builder.AddUserSecrets();
```

Mais uma vez de um __restore__ e __run__.

Para garantir que você vai ver a conexão do seu user-secrets adicione o seguinte código após o __Build()__ do __ConfigurationBuilder__:

`Console.WriteLine($"Connection = {Configuration.GetConnectionString("DefaultConnection")}");`

Com isso temos nosso Secret Manager configurado.

Caso você queira saber aonde seu `seret.json`se encontra, no windows a localização é:

`%APPDATA%\Roaming\microsoft\UserSecrets\<userSecretsId>\secrets.json`

