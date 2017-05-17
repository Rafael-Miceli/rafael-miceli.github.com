---
author: Rafael Miceli
comments: true
layout: default 
category : VSTS
title: "Package Sources NuGet no VSTS" 
date: 2017-05-16 22:40:00
---

O [VSTS](https://go.microsoft.com/fwlink/?LinkId=307137&clcid=0x409&wt.mc_id=o~msft~vscom~home-vsts-hero~27308&campaign=o~msft~vscom~home-vsts-hero~27308) é uma ferramenta de [C.I.](https://martinfowler.com/articles/continuousIntegration.html) excelente para seus projetos DotNet (além de outros). E neste post foi comentar de uma situação que passei configurando um projeto DotNet.

Quando vamos criar um Build Definition adicionamos cada Build Step que queremos que nosso C.I. execute para garantirmos que nossa aplicação está   funcionando corretamente.

Geralmente os dois primeiros steps são:

- Clonar o repo para o servior de build
- baixar as dependências do seu projeto

Este segundo step é bastante trivial quando todas as nossas dependências vem do Nuget (É bom evitar de subir Dlls para o repositório), mas e quando temos dependências de outro **Source (Feeder)** além do Nuget?

> Quando instalamos pacotes na nossa aplicação usando o Package Manager temos que informar de qual Source estamos buscando os pacotes e geralmente buscamos do [Source do Nuget](https://www.nuget.org/api/v2/) (Caso este Source mude postem no comentário)

### Configurando Nuget Sources

Temos uma forma bem simples de resolver isto. Na raiz da solution adicione a pasta `.nuget` e dentro dela adicione um arquivo `NuGet.config`

Dentro do arquivo implemente da seguinte maneira:

``` xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <solution>
    <add key="disableSourceControlIntegration" value="true" />
  </solution>
  <packageRestore>
        <add key="enabled" value="True" />
        <add key="automatic" value="True" />
    </packageRestore>
    <packageSources>        
        <add key="MySource" value="http://MuSourceAddress" />
        <add key="nuget.org" value="https://www.nuget.org/api/v2/" />
    </packageSources>
    <activePackageSource>
        <add key="All" value="(Aggregate source)" />
    </activePackageSource>
</configuration>
```

Dentro da tag `packageSources` colocamos todos os Sources que vamos baixar nossos packages

Podemos também criar um `NuGet.config` dentro da pasta de um ou mais projetos para que assim, os projetos que criamos o `NuGet.config` vão primeiro buscar dos Sources do config da pasta deles ao invés da raiz da Solution.

Para mais detalhes sobre o comportamento dos NuGet.config a [documentação do AspNet](https://docs.microsoft.com/en-us/nuget/consume-packages/configuring-nuget-behavior) sempre ajuda!