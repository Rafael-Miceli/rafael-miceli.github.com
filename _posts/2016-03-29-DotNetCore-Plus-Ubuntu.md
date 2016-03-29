---
author: Rafael Miceli
comments: true
layout: default
category : dotnetcore
title: ".Net Core + Ubuntu"
date: 2016-03-29 20:00:00
---

Quem diria isto acontecer. Isso mesmo amigos, podemos ter um ambiente do asp net core no Ubuntu e __sem Mono__. 

Mas antes de irmos para o asp.net core, vou mostrar passo a passo como preparar inicialmente o .net core, aonde a partir daqui já podemos criar CLIs para brincar. 


### dotnet Feed 

Para começarmos a instalação abrimos o temrinal no ubuntu usando o atalho:

    Ctrl + Alt + t 

Antes de instalarmos o **dotnet CLI** propriamente dito precisamos adicionar o feed para o .net Core

> Um lembrete importante, estamos fazendo esta instalação do feed de desenvolvimento do time do .Net
  
No terminal execute o seguinte código para adicionar o feed:

Primeiro:

    sudo sh -c 'echo "deb [arch=amd64] http://apt-mo.trafficmanager.net/repos/dotnet/ trusty main" > /etc/apt/sources.list.d/dotnetdev.list' 

Em seguida:

    sudo apt-key adv --keyserver apt-mo.trafficmanager.net --recv-keys 417A0893

E por fim

    sudo apt-get update
 

### Instalando dotnet CLI

Próximo passo, precisamos instalar agora o .net Core, 
para isso mais uma vez no terminal executamos o seguinte código:

    sudo apt-get install dotnet=1.0.0.001598-1

 
Excelente! Neste momento devemos ter o .net core instalado em nossa maquina.

### Criando um Hello 

Vamos criar um CLI com um belo Hello para testar, para isso crie uma nova pasta com o nome __HelloCore__ 

Nesta pasta crie 2 arquivos (sim apenas dois): 

- Program.cs 
- Project.json  

No Program.cs adicione o seguinte código: 

```C#
using System;

namespace ConsoleApplication
{
    public class Program
    {
        public static void Main(string[] args)
        {
            Console.WriteLine("Hello");
        }    
    }
}
```

No project.json o seguinte: 

```javascript
{
    "version": "1.0.0-*",
    "compilationOptions": {
        "emitEntryPoint": true
    },

    "dependencies": {
        "NETStandard.Library": "1.0.0-rc2-23811"
    },

    "frameworks": {
        "dnxcore50": { }
    }
}

``` 

### Rodando o projeto 

Para testarmos aponte o terminal para o endereço desta pasta da seguinte maneira: 


Agora execute os comandos na ordem 

    dotnet restore
    
E então 

    dotnet run 


O primeiro comando baixa todas as dependências que o seu projeto precisa para rodar.  

Já o segundo compila e executa nosso projeto. O resultado deve ser o seguinte:

![Resultado](http://rafael-miceli.com.br/ico/DotNetCore-Plus-Ubuntu/resultado-dotnet-core-ubuntu.png)

Vamos continuar esse papo num próximo artigo folks! 


 


 

