---
author: Rafael Miceli
comments: true
layout: default 
category : Angular2
title: "Angular2 - Criando uma aplicação no Asp.Net 4.5" 
date: 2016-07-28 11:00:00
---

Minha recomendação quando a este post é __cautela__. 
Pois caso você vá criar uma aplicação [Green Field](https://en.wikipedia.org/wiki/Greenfield_project) com AspNet recomendo que use o [AspNet Core](https://docs.asp.net/en/latest/).

Se você tiver uma [aplicação legada](https://en.wikipedia.org/wiki/Brownfield_(software_development)), ou não quer migrar para o novo AspNet Core por algum motivo neste post vou mostrar como você criar uma aplicação Angular2 e serve a mesma com AspNet. Com essa aplicação vamos evoluindo o código e vamos fazendo coisas bem diferentes nela.

### Configurando Angular 2

Vamos criar uma aplicação que nos mostre as matérias de escola que temos para cursar, nada muito fancy. Também vou fazendo comparações com o Angular 1 mostrando o que mudou de lá para cá.

Lembrando que o repositório deste exemplo vai estar disponível no [Github](https://github.com/Rafael-Miceli/Blog-Codes/tree/Angular2)

Passo 1

Primeiro crie uma aplicação web empty e na raiz da mesma crie 4 arquivos, esse são: 

__index.html__

```html
<!DOCTYPE html>
<html>

<head>
  <base href="/">

  <meta name="viewport" content="width=device-width, initial-scale=1.0">  
</head>

<body>
  <h1>Ola!</h1>
</body>

</html>s
```

__tsconfig.json__

```json
{
  "compilerOptions": {
    "target": "es5",
    "module": "system",
    "moduleResolution": "node",
    "sourceMap": true,
    "emitDecoratorMetadata": true,
    "experimentalDecorators": true,
    "removeComments": false,
    "noImplicitAny": false
  },
  "compileOnSave": true,
  "exclude": [
    "node_modules",
    "typings/main",
    "typings/main.d.ts"
  ]
}
```

__typings.json__

```json
{
  "ambientDependencies": {
    "es6-shim": "github:DefinitelyTyped/DefinitelyTyped/es6-shim/es6-shim.d.ts#7de6c3dd94feaeb21f20054b9f30d5dabc5efabd"
  }
}
```

__package.json__

```json
{
    "name": "IdeasWihOlive",
    "version": "0.0.1",
    "author": "Rafael Miceli",
    "description": "Package for Blog Test",
    "scripts": {
        "start": "concurrently \"npm run tsc:w\" \"npm run lite\" ",
        "tsc": "tsc",
        "tsc:w": "tsc -w",
        "lite": "lite-server",
        "typings": "typings",
        "postinstall": "typings install"
    },
    "license": "ISC",
  "dependencies": {
    "angular2": "2.0.0-beta.15",
    "systemjs": "0.19.26",
    "es6-shim": "^0.35.0",
    "reflect-metadata": "0.1.2",
    "rxjs": "5.0.0-beta.2",
    "zone.js": "0.6.10",
    "bootstrap": "^3.3.6"
  },
    "devDependencies": {
        "concurrently": "^2.0.0",
        "lite-server": "^2.2.0",
        "typescript": "^1.8.10",
        "typings": "^0.7.12"
    },
    "repository": {}
}
```

O __index.html__ é aonde nossa aplicação vai inicializar

Mais detalhes sobre o tsconfig.json e o typings.json no post [futuro](https://en.wikipedia.org/wiki/Future) de [TypeScript](http://rafael-miceli.com.br/categories.html#TypeScript-ref).

O package.json é o arquivo aonde gerenciamos os pacotes que nossa aplicação [front-end](https://en.wikipedia.org/wiki/Front_and_back_ends) que no caso é nosso Angular 2

Passo 2

Vamos inicializar o Angular 2 em nosso index.html, para isso nosso index.html vai fica da seguinte maneira:

__index.html__

```html
<!DOCTYPE html>
<html>

<head>
  <base href="/">

  <meta name="viewport" content="width=device-width, initial-scale=1.0">

  <!-- IE required polyfills, in this exact order -->
  <script src="node_modules/es6-shim/es6-shim.min.js"></script>
  <script src="node_modules/systemjs/dist/system-polyfills.js"></script>
  <script src="node_modules/angular2/es6/dev/src/testing/shims_for_IE.js"></script>

  <!-- Angular polyfill required everywhere -->
  <script src="node_modules/angular2/bundles/angular2-polyfills.js"></script>

  <script src="node_modules/systemjs/dist/system.src.js"></script>
  <script src="node_modules/rxjs/bundles/Rx.js"></script>
  <script src="node_modules/angular2/bundles/angular2.dev.js"></script>
  <script src="node_modules/angular2/bundles/router.dev.js"></script>
  <script src="node_modules/angular2/bundles/http.dev.js"></script>

  <script>
    System.config({packages:{
      api: {format: 'register',defaultExtension: 'js'},
      app: {format: 'register',defaultExtension: 'js'}
    }});
    System.import('app/main')
          .then(null, console.error.bind(console));
  </script>

</head>

<body>
  <h1>Olá</h1>
</body>

</html>
``` 

Anteriormente com Angular 1 você precisava utilizar __ng-app__ para indicar que sua aplicação estava usando o Angular e também inicializar o mesmo. Havia uma forma de [inicializar o Angular 1 diretamente pelo Javascript](https://docs.angularjs.org/guide/bootstrap#manual-initialization).

Com o Angular 2 existe apenas a forma de inicializar pelo Javascript.

Passo 3

Vamos criar nosso ponto de entrada da nossa aplicação Angular 2, ou seja, nosso "Main". Para isso criamos uma pasta chamada __app__ na raiz do nosso projeto e os seguintes arquivos dentro dessa pasta:

__main.ts__

```typescript
import {bootstrap}    from 'angular2/platform/browser';
import {AppComponent} from './app.component';

bootstrap(AppComponent);
```

__app.component.ts__

```typescript
import {Component} from 'angular2/core';

@Component({
    selector: 'materias',
    template: '<h1>Matérias para cursar</h1>',
})
export class AppComponent {     
}
```

Dentro do main.ts importamos a função __bootstrap__ que vem de __angular2/platform/browser__, e também nosso __Appcomponent__ que está no caminho __./app.component__, que vai ser nosso __Root Component__

Após importar ambos, chamamos a função __bootstrap__ para inicializarmos nosso __AppComponent__.

Para finalizar precisamos atualiza nosso __index.html__, aonde estava a tag: `<h1>Olá</h1>` vamos substituir pela tag:

    <materias>Iniciando...</materias>

Pronto, uma aplicação Angular 2 rodando.

No próximo Post vamos entender melhor o AppComponent e os Components do Angular.







