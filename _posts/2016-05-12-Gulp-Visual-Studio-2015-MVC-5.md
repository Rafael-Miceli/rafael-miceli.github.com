---
author: Rafael Miceli
comments: true
layout: default 
category : gulp 
title: "Usando Gulp com MVC 5 no Visual Studio 2015" 
date: 2016-05-12 20:00:00
---

Hoje em dia nossas aplicações web estão ficando cada vez mais ricas no Front-End, com isso alguns projetos adotam uma arquitetura de __Clients__ apontando para __APIs__.

Neste caso, __Clients__ não são clientes finais (usuários) efetivamente, estes __Clients__ vem do mesmo conceito de __Clients__ do [Oauth2](http://oauth.net/).

Uma forma de criar uma solução com MVC 5 nessa arquitetura é tendo um WebApplication Client e um WebApplication API

Nosso WebApplication API obviamente vai possuir diferentes ambientes, vamos supor que ele possua 2: __Dev__ e __Prod__.

Quando publicamos nossa API, publicamos a versão de __Dev__ no endereço http://localhost:5000 e a de __Prod__ no http://localhost:5009

Mas para o Javascript de nosso __Client__ como mudamos a url de acordo com cada build que damos? Ai que vamos conhecer o __Gulp__.

### Gulp

Gulp é um automatizador de tarefas, com ele podemos realizar tarefas que precisamos quando vamos dar um build (e outras coisas).

O Gulp usa uma forma de escrever essas tarefas bem fluente, o que fica fácil de entender o que estamos automatizando.

Agora vamos preparar o Gulp em uma aplicação nossa.

### Instalando e preparando o Gulp

1 - Para usarmos o Gulp em nossa aplicação primeiramente precisamos instalar o [Node](https://nodejs.org/en/).

2 - Após instalar o node, vamos em nossa aplicação Web, e na raiz do projeto vamos clicar com o botão direito do mouse e selecionamos Add... -> New Item... -> E vamos selecionar o __NPM Configuration File__.

![passo1](http://rafael-miceli.com.br/ico/Gulp-Visual-Studio-2015-MVC-5/passo1.png)

Caso você não possua este item de template pode adicionar apenas um arquivo do tipo json e nomea-lo como __package.json__.

3 - Agora vamos adicionar nosso arquivo gulp, muito similar ao passo anterior Add... -> New Item... -> __Gulp Configuration File__.

![passo2](http://rafael-miceli.com.br/ico/Gulp-Visual-Studio-2015-MVC-5/passo2.png)

Mesma coisa no passo anterior, caso você não possua este item de template pode adicionar um arquivo do tipo javascript com o nome __gulpfile.js__.

Excelente! Agora temos o gulp instalado em nossa aplicação!

Para fazer um teste vamos digitar o seguinte código em nosso __gulpfile__:

```javascript
var gulp = require('gulp'),
    file = require('gulp-file');

// define tasks here
gulp.task('default', function () {
    // run tasks here
    var str = "Say my name...";

    return file('name.js', str, { src: true })
    .pipe(gulp.dest('dist'));
});
```

Neste código estamos pedindo ao Gulp para criar um arquivo contendo o texto da variável _str_.

Para fazermos nossa task default executar abrimos nossa __Task Runner Explorer Window__. Perceba que nele está nosso Gulpfile.js com nossa task __default__.

Para fazermos ele executar em nosso build nós damos um _bind_ do mesmo em um dos nossos eventos:

![passo3](http://rafael-miceli.com.br/ico/Gulp-Visual-Studio-2015-MVC-5/passo3.png)

Excelente! Feito isso, podemos dar um build em nossa aplicação que nosso gulp vai criar um arquivo com o texto que passamos a variável dentro de uma pasta chamada _dist_.

Em um próximo post vamos fazer o gulp criar o arquivo de forma diferente para cada ambiente.
