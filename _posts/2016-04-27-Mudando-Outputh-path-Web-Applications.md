---
author: Rafael Miceli
comments: true
layout: default 
category : aspnet 
title: "Mudando o Outputh path de Web Applications" 
date: 2016-04-27 09:00:00
---

Esse post é sobre um problema que passei.

Em um projeto que trabalhei utilizando Asp.Net 4.5 mudamos o Output path da nossa aplicação.

O que antes era "bin" mudamos para "bin/Debug".

### O problema

Nós encontravamos o seguinte erro:

![passo1](http://rafael-miceli.com.br/ico/Mudando-Outputh-path-Web-Applications/passo1.png)

    The CodeDom provider type “Microsoft.CodeDom.Providers.DotNetCompilerPlatform.CSharpCodeProvider” could not be located

Como estávamos usando C# 6 nosso projeto possuía a dependência a __Microsoft.CodeDom.Providers.DotNetCompilerPlatform__. Por esse motivo nosso __CodeDom__ sempre procura por uma pasta chamada _roslyn_ no __bin__.

Como havíamos mudado o Output path encontramos esse erro.

### Possíveis soluções

Nós encontramos 3 seguintes possíveis soluções:

1 - Podemos remover a dependencia do __Microsoft.CodeDom.Providers.DotNetCompilerPlatform__, mas com isso vamos ficar impossibilitados de usar funcionalidades do C# 6

2 - Podemos criar um comando de __Post Build__:

    xcopy /Q /Y "$(TargetDir)roslyn\*.*" "$(TargetDir)..\roslyn\"

3 - Mudar o Output path para bin, como era antes.

Se encontrar esse problema, ai estão 3 possíveis soluções para o mesmo.
