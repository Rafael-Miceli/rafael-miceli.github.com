---
author: Rafael Miceli
comments: true
layout: default 
category : dependency-injection
title: "Diferentes bancos de dados com Dependency Injection" 
date: 2017-06-11 21:00:00
---

Neste post vamos imaginar o seguinte cenário: E se tenho um serviço WCF em que quero apontar para diferentes bases de dados dependendo do cliente que está realizando uma chamada ao meu serviço?

### Pensando o cenário

Como precisamos acessar uma base de dados para cada Client uma ideia seria nosso header trafegar um token de identificação do Client para que possamos garantir que o mesmo acesse a sua base de dados.

### Pensando no quesito segurança

Para que cada Client acesse corretamente sua base e não haja algum tipo de Injection para acessar a base de outro cliente uma forma de fazermos isso seria gerarmos um token para este cliente, armazenar o mesmo e no header que recebermos esse token apontamos para a base que temos armazenada deste Client, assim não trafegamos informações sensíveis de nossos clients.

Para fins de simplicidade deste cenário e não fugirmos do escopo vamos criar em memória um token fictício __Hard Code__ (não faça isso em PROD) para cada Client. 

Em um [post futuro](https://en.wikipedia.org/wiki/Future) podemos evoluir este cenário utilizando um centralizador de identidade, como [IdentityServer](https://github.com/IdentityServer) para que o mesmo gerencie estes tokens de acesso.

### Construir a Connection

Definidor uma forma de identificarmos nosso Client outra questão é aonde e como vamos construir essa conexão com a Base de Dados específica do Client.

Um lugar que podemos fazer isso seria na regra de construção da nossa dependência com o Repositório, vamos ver com mais detalhes isso.

### Escrevendo o cenário

Vamos reaproveitar já boa parte do código de **Injeção de dependência no WCF com AutoFac** de um [post anterior](http://rafael-miceli.com.br/wcf/2017/04/28/WCF-Dependency-Injection.html)

Antes de entrarmos no código do WCF, em um banco de dados (Estou usando sql server) crie os dois databases abaixo:

```sql
USE [Master]
GO

CREATE DATABASE AlunosClient1
GO

USE [AlunosClient1]
GO

CREATE TABLE [dbo].[Alunos]
(   
 [Id] [uniqueidentifier] DEFAULT NEWID() NOT NULL,
 [Name] [varchar](50) NULL
) 
GO

INSERT INTO ALUNOS (NAME) VALUES ('Aluno do Cliente 1')
GO

USE [Master]
GO

CREATE DATABASE AlunosClient2
GO

USE [AlunosClient2]
GO

CREATE TABLE [dbo].[Alunos]
(   
 [Id] [uniqueidentifier] DEFAULT NEWID() NOT NULL,
 [Name] [varchar](50) NULL
) 
GO

INSERT INTO ALUNOS (NAME) VALUES ('Aluno do Cliente 2')
GO
```

Agora, em nosso WCF vamos adicionar um `AlunoRepoToAnyDb`:

```csharp
public class AlunoRepoToAnyDb : IAlunoRepoToAnyDb
{
    private string _databaseServer;

    public AlunoRepoToAnyDb(string databaseServer)
    {
        _databaseServer = databaseServer;
    }

    public string GetAlunos()
    {
        using (SqlConnection conn = new SqlConnection(_databaseServer))
        {
            SqlCommand cmd = new SqlCommand("Select Name from Alunos", conn);

            conn.Open();

            using (SqlDataReader reader = cmd.ExecuteReader())
            {
                string nomesDoAlunos = null;

                while (reader.Read())
                    nomesDoAlunos += reader[0].ToString().Trim() + ", ";

                return nomesDoAlunos ?? "Não foi encontrado nenhum aluno";
            }
        }
    }
}
```

Após, Na classe Bootstrapper vamos criar os métodos, **BuildDbRepo** e **MontarConn**:

```csharp
private static AlunoRepoToAnyDb BuildDbRepo()
{
    IncomingWebRequestContext request = WebOperationContext.Current.IncomingRequest;
    WebHeaderCollection headers = request.Headers;
                
    var clientToken = headers["clientToken"];       
    
    var connStringDb = MontarConn(clientToken);

    return new AlunoRepoToAnyDb(connStringDb);
}

private static string MontarConn(string clientToken)
{
    if (clientToken == "Token1")
    {
        return "Data Source=(localDb);Initial Catalog=AlunosClient1;";
    }

    if (clientToken == "Token2")
    {
        return "Data Source=(localDb);Initial Catalog=AlunosClient2;";
    }

    throw new Exception("Not a valid client token");
}
```

Em nosso método de BuildContainer vamos fazer nosso repositório ser registrado usando esse Builder dele:

```csharp
public static IContainer BuildContainer()
{
    var builder = new ContainerBuilder();

    builder.Register(r => BuildDbRepo()).As<IAlunoRepoToAnyDb>();

    // build container
    return builder.Build();
}
```

Para testarmos se nosso WCF está montando corretamente a string de conexão vamos chamar ele através [Postman](https://chrome.google.com/webstore/detail/postman/fhbjgbiflinjbdggehcddcbncdddomop). Caso você não conheça o Postman é uma ferramenta para realizar requests para um endpoint da nossa escolha. 

Em nosso Postman montamos o mesmo com os seguintes Headers:

```
Connection: Keep-Alive
Content-Length: 214
Content-Type: text/xml; charset=utf-8
Accept-Encoding: gzip, deflate
Expect: 100-continue
Host: localhost:porta
SOAPAction: "http://tempuri.org/IAlunoServiceContract/BuscarNomesDosAlunos"
clientToken: Token1
```

Também o seguinte Body em `text/xml`:

```xml
<soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/" xmlns:tem="http://tempuri.org/">
   <soapenv:Header/>
   <soapenv:Body>
      <tem:BuscarNomesDosAlunos/>
   </soapenv:Body>
</soapenv:Envelope>
```

Excelente! Se testarmos mudando nosso `clientToken` (que está abstraido) vamos ver que construímos nosso repositório com diferentes "apontamentos para databases".

Esse é o ponto principal, com isso nossa aplicação pode construir Connections para diferentes bancos. Para ver o exemplo completo pegue o mesmo no [repositório deste exemplo](https://github.com/Rafael-Miceli/Blog-Codes)