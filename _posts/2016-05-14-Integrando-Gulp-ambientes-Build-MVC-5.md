---
author: Rafael Miceli
comments: true
layout: default 
category : gulp 
title: "Integrando Gulp com Ambientes de Build no MVC 5" 
date: 2016-05-14 18:00:00
---

No [post anterior](http://rafael-miceli.com.br/gulp/2016/05/12/Gulp-Visual-Studio-2015-MVC-5.html) fizemos nossa aplicação MVC 5 usar o gulp e executar tasks do mesmo no nosso Task Runner Explorer.

Agora, vamos rodar diferentes tasks para nossos diferentes builds de ambiente.

### Tasks Debug e Release no gulp

Se quisermos executar tasks especificas para cada build nosso em nosso __gulpfile.js__ vamos criar uma task de Debug e outra de Release:

```javascript
var gulp = require('gulp'),
    file = require('gulp-file');

// define tasks here
gulp.task('Release', function () {
    // run tasks here
    var str = "Say my name... in Release";
    
    return file('name.js', str, { src: true })
    .pipe(gulp.dest('dist'));
});

gulp.task('Debug', function () {
    // run tasks here
    var str = "Say my name... in Debug";
    
    return file('name.js', str, { src: true })
    .pipe(gulp.dest('dist'));
});
```

No código acima vamos escrever diferentes frases para diferentes configurations de build.

Caso você crie um novo configuration, basta criar uma task com o nome desse novo configuration.

### Executando gulp com o build

Para executar o gulp em cada build e também para que o build do gulp não fique preso ao visual Studio, vamos remover nossa task do gulp do nosso task runner explorer:

![passo1](http://rafael-miceli.com.br/ico/Integrando-Gulp-ambientes-Build-MVC-5/passo1.png)

Agora vamos adicionar o build do nosso gulp em nosso __Pre-build event command line__ clicando com o botão direito em cima de nosso projeto e selecionando a opção properties. Na opção __Build Events__ vamos adicionar o seguinte código em __Pre-build event command line__:

![passo2](http://rafael-miceli.com.br/ico/Integrando-Gulp-ambientes-Build-MVC-5/passo2.png)


Código do __Pre-build event command line__:

    cd $(ProjectDir)
    gulp $(Configuration)

Se dermos um build em nossa aplicação em debug, veremos o resultado aplicado pela task debug em nosso arquivo __name.js__.

Se mudar para release e der um nobo build, veremos o resultado da task de release.

Agora podemos definir tasks para cada build configuration em nosso gulp.
