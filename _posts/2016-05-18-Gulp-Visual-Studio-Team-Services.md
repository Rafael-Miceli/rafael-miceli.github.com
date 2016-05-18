---
author: Rafael Miceli
comments: true
layout: default 
category : gulp 
title: "Gulp no Visual Studio Team Services" 
date: 2016-05-18 10:00:00
---

Como no [post anterior](http://rafael-miceli.com.br/gulp/2016/05/14/Integrando-Gulp-ambientes-Build-MVC-5.html), que temos nossa aplicação web com mvc 5 usando gulp em seu pre build, vamos agora ajustar nosso visual studio team services [VSTS] (antigo visual studio online) para rodar nossa task gulp.

### Configurando o VSTS

Passo 1 vamos criar um build definition cliando no icone __"+"__.

O VSTS tem um template de uma aplicação com visual studio, vamos selecionar ela.

![passo1](http://rafael-miceli.com.br/ico/Gulp-Visual-Studio-Team-Services/passo1.png)

E a próximo opção é em qual repositório sua aplicação está, no geral você deve estar usando ela no próprio VSTS

Nesse template vamos ter uns passos já prontos que são os seguintes:

![passo2](http://rafael-miceli.com.br/ico/Gulp-Visual-Studio-Team-Services/passo2.png)

Explicando resumidamente os passos:

1. O primeiro passo instala todas as dependências que seu Nuget possui.

2. O segundo vai construir nossa aplicação. 

3. O terceiro roda todos os testes em projetos que tenha a palavra *test* em seu nome.

4. O quarto passo a grosso modo é para caso você queira __debugar__  sua aplicação, pois neste passo ele publica seus _.pdbs_. 

> Detalhe que este passo funciona apenas se você esta usando o repositório do VSTS

5. O quinto passo vai copiar seus arquivos de um lugar para outro, neste caso do diretório fonte, para o diretório de staging de publish.

6. O sexto passo publica seu _build artifcats_ ( o resultado do seu build para publicação) no próprio VSTS

Agora que entendemos o que os passos atuais fazem vamos para termos nosso gulp aqui.

Vamos adicionar um build step do node.

![passo3](http://rafael-miceli.com.br/ico/Gulp-Visual-Studio-Team-Services/passo3.png)

Neste build step vamos configurar ele para instalar o gulp e o gulp-file localmente

![passo4](http://rafael-miceli.com.br/ico/Gulp-Visual-Studio-Team-Services/passo4.png)

> Este próximo passo não é necessário caso você já tenha o gulp rodando em seu pre build event.
	
Prefeito! Vamos adicionar mais um build step, dessa vez o gulp

![passo5](http://rafael-miceli.com.br/ico/Gulp-Visual-Studio-Team-Services/passo5.png)

E nele vamos configurar para rodar uma task que tenha o nome do nosso build configuration.

![passo6](http://rafael-miceli.com.br/ico/Gulp-Visual-Studio-Team-Services/passo6.png)

Excelente!

Com isso temos nosso VSTS rodando nossas tasks do gulp.