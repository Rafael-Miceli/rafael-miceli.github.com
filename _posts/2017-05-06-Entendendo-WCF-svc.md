---
author: Rafael Miceli
comments: true
layout: default 
category : wcf
title: "Entendendo WCF ServiceHost .svc" 
date: 2017-05-06 19:30:00
---

### Etendendo um pouco melhor o arquivo *.svc

Falando mais um pouco de WCF, eu já tive muitas vezes dúvida sobre como funciona o arquivo \*.svc e neste post espero resolver as dúvidas que já tive, pois são poucas fontes com explicação fácil (pelo menos para mim) para entender o papel dele no WCF.

Quando criamos um template da nossa aplicação WCF o \*.svc ja vem de cara, mas ele não é obrigatório para que nossa aplicação WCF rode. Este arquivo é reconhecido apenas pelo IIS (que é **muito** provavelmente aonde você vai hospedar seu WCF) mas não é obrigatório. Ele serve para facilitar a inicialização do seu Endpoint. 

Caso você pretenda fazer um SelfHost este arquivo não é necessário (mais de self host em outro post)

Quando criamos o \*.svc de um template do AspNet ele vem com os seguintes atributos: 

- __Language__: Que especifica qual linguagem esse host vai compilar, geralmente vem preenchido C# ou VB
- __Debug__: Indica se ao compilar o projeto ele vai gerar o \*.pdb que é para _debugar_ o código. Resumindo "Vou debugar ou não?". Pode ser _true_ ou _false_, e é __case sensitive__!
- __Service__: Este atributo é um pouco mais complexo, podemos montar ele das seguintes maneiras:
    
    1 - Passamos o nome do Service que implemente uma Interface de `ServiceContract`, 
    exemplo: `Service="WcfService1.Service1"`

    2 - Passamos o nome do Service que implemente uma Interface de `ServiceContract` e o namespace que se encontra este serviço, 
    exemplo: `Service="WcfService1.Service1, namespace.service"`

   3 - Passamos o nome da Interface de `ServiceContract` e o namespace que se encontra esta interface, 
    exemplo: `Service="WcfService1.IService1, namespace.contract"` 
 > Neste caso vamos precisar também que o factory esteja implementado

- __CodeBehind__: Este atributo é opcional. Caso seu \*.svc possua abaixo dele um arquivo do tipo \*.svc.cs (ou \*.svc.vb) você informa o nome da sua classe no \*.svc.cs aqui (como já vem no template) caso você não tenha uma classe abaixo do \*.svc então este atributo não é necessário.
- __Factory__: Este atributo também é mais complexo, assim como no atributo __Service__ nele passamos a classe Factory e seu namespace que vamos usar para instânciar o __Service__. No template de WCF ele não implementa este atributo. Quando este atributo não é implementado ele usa o Factory default que é o [ServiceHostFactory](https://msdn.microsoft.com/fr-fr/library/system.servicemodel.activation.servicehostfactory(v=vs.100).aspx)

### Formas de implementar o WCF com *.svc

1 - Com CodeBehind.

Para ver o \*.svc com CodeBehind na forma padrão basta criar um project template do WCF que ele já cria com CodeBehind

2 - Sem CodeBehind e com Service namespace

O código de exemplo abaixo pode ser encontrado no [repositório deste blog](https://github.com/Rafael-Miceli/blog-codes/tree/WcfApp)

AlunoServiceEndpoint.svc

```
<%@ ServiceHost 
Language="C#"
Debug="true"
Service="WcfApp.Services.AlunoService, WcfApp.Services"
%>
```

AlunoService.cs

```csharp
namespace WcfApp.Services
{
    public class AlunoService: IAlunoServiceContract
    {
        public IEnumerable<string> BuscarNomesDosAlunos()
        {
            return new List<string> { "Aluno1", "Aluno2" };
        }
    }
}
```

IAlunoServiceContract.cs

```csharp
namespace WcfApp.Contracts
{
    [ServiceContract]
    public interface IAlunoServiceContract
    {
        [OperationContract]
        IEnumerable<string> BuscarNomesDosAlunos();
    }
}
```


3 - Utilizando Factory

Para ver um exemplo utilizando Factory veja meu [post anterior](http://rafael-miceli.com.br/wcf/2017/04/28/WCF-Dependency-Injection.html) aonde faço [Dependency Injection](https://martinfowler.com/articles/injection.html) com [AutoFac](https://autofac.org/) e com isso acabo utilizando o [AutofacServiceHostFactory](https://autofac.org/apidoc/html/CA546AD9.htm). 

> Fontes: https://msdn.microsoft.com/pt-br/library/aa967286(v=vs.110).aspx
