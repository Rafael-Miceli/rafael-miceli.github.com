---
author: Rafael Miceli
comments: true
layout: default
category : AspNet5
title: "Asp Net 5 project.json"
date: 2015-03-24 14:00:00
---


Vamos falar sobre o novo projecto.json do AspNet 5 
 
É neste arquivo que gerenciamos as dependências .Net do nosso projeto. É exatamente o que estão pensando, não usamos mais um arquivo project em C#. Agora usamos NuGet para gerenciar nossas dependências, e guardamos elas neste novo project.json.  
Percebam também que mais acima comentei explicitamente "dependências .Net", isto porque o project.json vai conter (quase sempre) apenas referências a assemblys .Net. 
 
###OK, mas porque esta mudança? 
 
Bem, um dos motivos é para simplificar o gerenciamento das dependências.  
Outro motivo é que a Microsoft esta tornando o AspNet independente do Windows e do Visual Studio. Como mencionado no artigo anterior o novo AspNet vai poder rodar em Linux e OSX. 
 
###Gerenciando as dependências  
 
Vamos ver a nova tela para gerenciar essas dependências. 
 
Na raiz do nosso projeto clique com o botão direito e selecione "Manage NuGet  Packages...". 
 
![passo 1](http://rafael-miceli.com.br/ico/AspNet5-project-json/passo1.png)
 
Esta é nossa nova interface para gerenciar dependências .Net, nela podemos atualizar nossas dependências, listar as já instaladas ou buscar novas. 
 
![passo 2](http://rafael-miceli.com.br/ico/AspNet5-project-json/passo2.png)
 
Podemos gerenciar nossas dependências também diretamente do arquivo, vamos abrir o project.json e tentar adicionar o SignalR. 
Dentro do objeto "dependencies" podemos começar a digitar o nome de uma nova dependência e note que o intelisense vem nos salvar!  
 
![passo 3](http://rafael-miceli.com.br/ico/AspNet5-project-json/passo3.png) 
 
Após selecionar a dependência, quando selecionar a versão note que o intelisense esta lá novamente! 
 
![passo 4](http://rafael-miceli.com.br/ico/AspNet5-project-json/passo4.png)
 
####Isso é incrível!  
 
Se você deixar a versão em branco você vai estar usando sempre a última versão deste pacote. 
 
Após isto, quando você salvar as mudanças note que em "References" vai aparecer ao lado "Restoring". 
Neste momento o AspNet está fazendo download, se necessário, das referências. 
 
![passo 5](http://rafael-miceli.com.br/ico/AspNet5-project-json/passo5.png)
 
Vamos agora visualizar nossas referências, e você vai ver dois AspNets abaixo de "References" 
 
![passo 6](http://rafael-miceli.com.br/ico/AspNet5-project-json/passo6.png)
 
Eu sei que provavelmente você deve estar neste momento:
 
![What?](http://rafael-miceli.com.br/ico/AspNet5-project-json/catwhat.jpg)
 
Mas isto é tema para outro artigo. Expandindo um dos AspNets você vai ver a nossa nova referência. 
 
![passo 7](http://rafael-miceli.com.br/ico/AspNet5-project-json/passo7.png)
 
Com isto vemos uma das grandes mudanças na forma de referências que o novo AspNet sofreu, e ainda vamos ver mais! 







