---
author: Rafael Miceli
comments: true
layout: default 
category : dotnet 
title: "Configuration Transformation em app.config com visual studio 2015" 
date: 2016-04-26 09:00:00
---

Quando criamos uma aplicação web com asp.net 4.5 podemos ter diferentes web.configs para cada configuration environment. 
Por default, quando criamos uma nova aplicação temos um Web.debug e um web.release 
e esses diferentes configs são muito poderosos para temros diferentes valores em diferentes ambientes (Como por exemplo ConnectionString), 
como podemos aproveitar isso para o __app.config__?

### Xml transformation

Para aproveitar essa funcionalidade para nossos app.config's precisamos acessar o __csproj__ da nossa aplicação.

No final dele vamos encontrar um seguinte trecho para tasks.

```xml
<Import Project="$(MSBuildToolsPath)\Microsoft.CSharp.targets" />
<!-- To modify your build process, add your task inside one of the targets below and uncomment it. 
    Other similar extension points exist, see Microsoft.Common.targets.
<Target Name="BeforeBuild">
</Target>
<Target Name="AfterBuild">
</Target>
-->
```

O que vamos fazer aqui, vai ser usar a funcionalidade do web.config de __Xml transformation__ no __csproj__ de um console app, afinal, ambos são Xml.

> Lembrando que o projeto com este código está em meu [Github](https://github.com/Rafael-Miceli/Blog-Codes/tree/master/IdeiasComAzeite)

Para podermos fazer a transformação, se estiver no visual studio 2015, precisamos adicionar o seguinte trecho de código no __csproj__ logo abaixo do elemtno _Import_:

```xml
<UsingTask TaskName="TransformXml" AssemblyFile="$(MSBuildExtensionsPath32)\Microsoft\VisualStudio\v$(VisualStudioVersion)\Web\Microsoft.Web.Publishing.Tasks.dll" />
<Target Name="AfterCompile" Condition="Exists('App.$(Configuration).config')">
<!--Generate transformed app config in the intermediate directory-->
<TransformXml Source="App.config" Destination="$(IntermediateOutputPath)$(TargetFileName).config" Transform="App.$(Configuration).config" />
<!--Force build process to use the transformed configuration file from now on.-->
<ItemGroup>
    <AppConfigWithTargetPath Remove="App.config" />
    <AppConfigWithTargetPath Include="$(IntermediateOutputPath)$(TargetFileName).config">
    <TargetPath>$(TargetFileName).config</TargetPath>
    </AppConfigWithTargetPath>
</ItemGroup>
</Target>
<!--Override After Publish to support ClickOnce AfterPublish. Target replaces the untransformed config file copied to the deployment directory with the transformed one.-->
<Target Name="AfterPublish">
<PropertyGroup>
    <DeployedConfig>$(_DeploymentApplicationDir)$(TargetName)$(TargetExt).config$(_DeploymentFileMappingExtension)</DeployedConfig>
</PropertyGroup>
<!--Publish copies the untransformed App.config to deployment directory so overwrite it-->
<Copy Condition="Exists('$(DeployedConfig)')" SourceFiles="$(IntermediateOutputPath)$(TargetFileName).config" DestinationFiles="$(DeployedConfig)" />
</Target>
```

Agora, precisamos criar __manualmente__ nosso _app.Debug.config_ e _app.Release.config_ e adicionar ao projeto:

![passo1](http://rafael-miceli.com.br/ico/Configuration-Transformation-app-config-visual-studio-2015/passo1.png)

Para adicionar em nosso projeto clique em Add... Existing Item... Selecione All Files e adicione os config's. 

![passo2](http://rafael-miceli.com.br/ico/Configuration-Transformation-app-config-visual-studio-2015/passo2.png)

E por fim, para ficar bonito, vamos novamente em nosso csproj e vamos agrupar nossos diferentes configs abaixo de app.config:

```xml
<ItemGroup>
    <None Include="App.config" />
    <None Include="App.Debug.config">
        <DependentUpon>App.config</DependentUpon>
    </None>
    <None Include="App.Release.config">
        <DependentUpon>App.config</DependentUpon>
    </None>
</ItemGroup>
```

Pronto! Temos nosso Xml Transformation funcionando

### Facilitando um pouco

Agora que entendemos como funciona s transformação do Xml, 
ao invés de termos de ficar digitando todo esse código, 
existe um plugin do Visual Studio que facilita muito nossa vida, que se chama [__Configuration Transform__](https://visualstudiogallery.msdn.microsoft.com/579d3a78-3bdd-497c-bc21-aa6e6abbc859).

Basicamente com ele podemos adicionar todo o código para o __Xml Transformation__ com poucos clicks.

### atenção ao caminho da dll e a task

Uma Atenção que tem que ser observada, é que os exemplos mais famosos na internet são com o visual Studio 2010, 
então o path da dll __Microsoft.Web.Publishing.Tasks.dll__ muda drasticamente.

Outro ponto importante, é estar atento que apenas o ultimo elemento __Target__ __AfterCompile__ dentro do __csproj__ que vai executar.
Então fique de olho, pois caso a transformação não esteja funcionando, verifique o __csproj__.

