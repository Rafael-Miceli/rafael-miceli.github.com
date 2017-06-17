---
author: Rafael Miceli
comments: true
layout: default 
category : dotnetcore
title: "Resolvendo Dotnet restore para mais de um PackageSource" 
date: 2017-06-17 14:00:00
---

Imagine que você vai começar a fazer uma aplicação com .NetCore, e ao realizar o comando `dotnet restore` você percebe que o DotNet está tentando fazer o restore de um local que não é o _Source_ padrão do Nuget e começa a tomar diversos 404 (_Not Found_). Mais ou menos assim:

`Restoring packages for C:\Users\you\My.csproj...
  Retrying 'FindPackagesByIdAsyncCore' for source 'http://YourSource/FindPackagesById()?id='Package.YouAre.LookingFor''.
  Response status code does not indicate success: 404 (Not Found).`

### Pensando o cenário

Imaginando que nunca adicionamos um novo PackageSource para o .Net Core precisamos descobrir porque nossa aplicação esta indo até este Source.

Mas, e se já configuramos outros sources usando o Visual Studio para aplicações que não eram .NetCore? Será se isso impactaria? E porque impactaria?

Bem, quando executamos o `dotnet restore` perceba que o .Net nos avisa de quais lugares ele esta procurando um `NuGet.config`:

```
NuGet Config files used:
      C:\Users\You\AppData\Roaming\NuGet\NuGet.Config
      C:\Program Files (x86)\NuGet\Config\Microsoft.VisualStudio.Offline.config

  Feeds used:
      https://api.nuget.org/v2/index.json
      C:\Program Files (x86)\Microsoft SDKs\NuGetPackages\
```

Repare que ele está tentando pegar um `NuGet.config` em nosso `AppData`, que por padrão nosso Visual Studio* também lê de lá.

> *Testado nos VS 2013, 2015 (ainda confirmar no 2017)

### Resolvendo o cenário

Para resolvermos esse cenário umas das formas que podemos fazer é criar um arquivo NuGet.config na raiz do seu projeto com o seguinte conteudo:

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <packageSources>
    <clear/>
    <add key="myget.org dotnet-buildtools" value="https://dotnet.myget.org/F/dotnet-buildtools/api/v3/index.json" />
    <add key="myget.org dotnet-core" value="https://dotnet.myget.org/F/dotnet-core/api/v3/index.json" />
    <add key="nuget.org" value="https://api.nuget.org/v3/index.json" />
  </packageSources>
  <config>
    <add key="repositoryPath" value="..\packages" />
  </config>
  <packageRestore>
    <add key="automatic" value="false" />
  </packageRestore>
</configuration>
```

Para saber mais detalhes dos comportamentos do **Nuget.config** recomendo as seguintes referências:

https://docs.microsoft.com/en-us/nuget/consume-packages/configuring-nuget-behavior

https://docs.microsoft.com/en-us/nuget/schema/nuget-config-file

