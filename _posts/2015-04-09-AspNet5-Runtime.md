---
author: Rafael Miceli
comments: true
layout: default
category : AspNet5
title: "AspNet 5 Runtime"
date: 2015-04-09 09:00:00
---

Se você leu o meu último artigo sobre o novo [AspNet](http://rafael-miceli.com.br/aspnet5/2015/03/24/AspNet5-project-json.html) deve lembrar que uma das coisas que nos chamou a atenção foi o fato de haver mais de um CLR abaixo do nosso references. 
 
![passo 1](http://rafael-miceli.com.br/ico/AspNet5-Runtime/passo1.png)
 
Um é nosso já conhecido .Net CLR, o é o novo CoreCLR, que foi feito otimizado para nuvem e para ser independente de plataforma. 
Olhando o nosso arquivo project.json, você vai ver dentro do objeto framework, para quais frameworks que estamos compilando nosso projeto.  
 
Se após a criação deste template você não modificou o project.json você vai ver que em nosso objeto frameworks vai estar o aspnet50 e o aspnetcore50.  
 
![passo 2](http://rafael-miceli.com.br/ico/AspNet5-Runtime/passo2.png)
 
Se você realizar um build neste momento, seu projeto vai ser compilado para ambos os frameworks. 
 
OK, vamos fazer um teste para ver a diferença entre os frameworks. 
 
Vamos criar um controller e vamos retornar o nome da nossa assembly. 
 
![passo 3](http://rafael-miceli.com.br/ico/AspNet5-Runtime/passo3.png)
 
Vamos rodar nosso projeto. 
 
![passo 4](http://rafael-miceli.com.br/ico/AspNet5-Runtime/passo4.png)
 
Legal, tudo OK.... Bem...  Não. 
 
Se voltarmos ao nosso output window para o build deste projeto, vai ver que tem um erro nele. O erro diz que não foi possível realizar o build da CoreCLR. 
 
Isto ocorre porque esta assembly não existe em nosso Core CLR. 
 
#####OK, mas aonde está o Core Clr para eu confirmar que a assembly não existe? 
 
Se você for a use-folder/.kre/packages vai encontrar nosso Core CLR, e também vai encontrar O CLR que sempre usamos também. Mas apenas uma fração do antigo modelo de CLR esta aqui, o resto continua como antes. No GAC(link). 
 
Já o CoreCLR está inteiramente aqui! Ele é muito leve mesmo, e você ainda pode instala-lo com seus projetos. 
 
Agora, sabemos que estamos compilando nosso projeto para ambos os CLRs e aonde o CoreCLR está, mas quando lançamos a aplicação, qual CLR estamos usando? 
 
Para descobrir isto, clique com o botão direito no seu projeto AspNet e selecione properties 
 
![passo 5](http://rafael-miceli.com.br/ico/AspNet5-Runtime/passo5.png)
 
Perceba que na opção “Application” estamos apontando para *KRE-CLR-x86.1.0.0.-beta1*. 

![passo 6](http://rafael-miceli.com.br/ico/AspNet5-Runtime/passo6.png)

Este é nosso CLR já conhecido, se expandirmos esta combobox teremos a opção de selecionar nosso CoreCLR

![passo 7](http://rafael-miceli.com.br/ico/AspNet5-Runtime/passo7.png)
 
Ou seja, estamos compilando para ambas as CLR, mas estamos lançando a aplicação usando o CLR já conhecido. 
 
###E os .Net packages que instalamos? 
 
Nas versões anteriores do AspNet quando instalavamos um pacote usando nuget ele era guardado em nossa pasta package na raiz do nosso projeto. Essa pasta não existe mais. 
 
Agora todos os pacotes (independente do projeto) estão todos centralizados em um mesmo lugar. 
 
Eles se encontram em user-folder/.kpm/packages 
 
E se você lembra, no último artigo sobre o novo AspNet, nós instalamos o signalR. Perceba que lá está ele. 

No próximo artigo vamos ver como ficou o novo MVC e a nova Web API. 
