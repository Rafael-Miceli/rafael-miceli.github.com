---
author: Rafael Miceli
comments: true
layout: default
category : aspnetcore
title: ".Net Core + Ubuntu"
date: 2016-03-30 23:30:00
---

Da ultima vez instalamos o dotnet CLI no Ubuntu para rodarmos nosso primeiro console app de .Net no Linux. 
 
Vamos preparar nosso ambiente agora para o asp.net core 
 
### DNVM 
 
Para começar facilitando, toda documentação de como preparar seu ambiente esta em http://get.asp.net 
  
DNVM significa Dot Net Version Manager. Se você possui um background de ruby vai lembrar do RVM (Ruby Version Manager). 
 
Com o DNVM podemos gerenciar qual versão do DNX(vamos chegar lá) vamos usar em nosso ambiente de trabalho. Entramos em mais detalhes do dnvm em um próximo artigo.

No primeiro passo precisamos instalar o DNVM em nossa maquina.  
 
### Instalando o DNVM 
 
Para instalar o DNVM no seu ubuntu abra o terminal usando o atalho:

    Ctrl + alt + t 
 
Antes de instalar o DNVM, caso você não possua, instale __unzip e curl__ 
 
Execute o seguinte código no terminal: 
 
    sudo apt-get install unzip curl 
 
Após isso instalamos o DNVM com o seguinte código no terminal: 
 
    curl -sSL https://raw.githubusercontent.com/aspnet/Home/dev/dnvminstall.sh | DNX_BRANCH=dev sh && source ~/.dnx/dnvm/dnvm.sh 
 
Após a instalação, digite no terminal: 
 
    DNVM  
 
Deve aparecer o seguinte: 
 
![Resultado](http://rafael-miceli.com.br/ico/AspNetCore-Plus-Ubuntu/dnvm.png) 
 
### DNX 
 
O DNX significa Dot Net Execution Environment. Usamos o DNX para executar nossos projetos do asp.net core. Com ele compilamos e rodamos nosso projeto. 
 
Utilizamos o DNVM para instalar o DNX, mas antes temos que instalar uns pré-requisitos para o DNX.  

Instalamos com o seguinte código no terminal: 
 
    sudo apt-get install libunwind8 gettext libssl-dev libcurl4-openssl-dev zlib1g libicu-dev uuid-dev
 
Agora, usamos nosso DNVM para instalamos nosso DNX com o código: 
 
    dnvm upgrade -r coreclr
 
 
### Instalando libuv 
 
Para finalizar, precisamos instalar o libuv. O libuv é usado pêlo kerstel.  
 
Kerstel é um servidor multi plataforma que sobe o .net no Linux. Sim, não é o IIS. 
 
Execute a seguencia de código abaixo para instalar o libuv 
 
Primeiro:

    sudo apt-get install make automake libtool curl
Segundo:

    curl -sSL https://github.com/libuv/libuv/archive/v1.8.0.tar.gz | sudo tar zxfv - -C /usr/local/src
Terceiro:

    cd /usr/local/src/libuv-1.8.0
Quarto:

    sudo sh autogen.sh
Quinto:

    sudo ./configure
E assim para cada linha:

    sudo make
    sudo make install
    sudo rm -rf /usr/local/src/libuv-1.8.0 && cd ~/
    sudo ldconfig 
 
 
Excelente! Com o ambiente preparado, no próximo post criamos nossa aplicação e exploramos um pouco ela.  