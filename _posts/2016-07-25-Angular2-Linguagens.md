---
author: Rafael Miceli
comments: true
layout: default 
category : Angular2
title: "Angular2 - Linguagens" 
date: 2016-07-25 10:00:00
---

O [Angular 2](https://angular.io/) esta em R.C.(Release Candidate) e daqui pouco vai ter sua versão de produção pronta.
Com isso já é bom ir vendo como está ficando. 

Para quem já passou pelo Angular 1, muita coisa mudou! Algumas coisas continuam as mesma, mas algumas mudaram bastante!

Neste post não vou entrar a fundo em código, apenas vou apontar a nova forma como ele trabalha e em um próximo post teremos um exemplo.

### Linguagens para usar com Angular 2

É possível programar Angular 2 com mais de uma linguagem?

Bem, sim...

O Angular 2 pode ser programado com [ES5](https://en.wikipedia.org/wiki/ECMAScript#5th_Edition), [ES6](https://en.wikipedia.org/wiki/ECMAScript#6th_Edition_-_ECMAScript_2015), [TypeScript](https://www.typescriptlang.org/) e [Dart](https://www.dartlang.org/).

Mas, em qual programar?

Antes, é preciso explicar um pouco a diferença entre as 4 linguagens acima.

__ES5__:  É provavelmente o que mais escrevemos no dia a dia, o bom e velho Javascript comum. Angular 1 foi feito com ele e foram usadas diversas Patterns do ES5 para organizar melhor o código. 
O time do Angular 2 não recomenda que use o ES5 uma vez que você vai escrever muito mais código, com muito menos vantagens, e também porque em "pouco" tempo vamos migrar para o ES6.

__ES6__: É uma extensão do ES5. É a nova versão do ES e nem todos os browsers ainda entendem ES6. Ele vem com algumas novidades como "classes", "let", "import", "export" e outras que nos ajudam muito a escrever um código melhor com ES. 
Você pode usar o ES6 para escrever em Angular2, mas para isso vai precisar do [Babel.Js](https://babeljs.io/), que é um [transpiler](https://en.wikipedia.org/wiki/Source-to-source_compiler) de ES6 para ES5.

__TypeScript__: É a linguagem que o time do Angular 2 escolheu como a principal. __TypeScript também é Javascript!__ Só que é mais que ES5 e ES6. TypeScript é um [superset](https://en.wikipedia.org/wiki/TypeScript) do ES6. É OpenSource e muito bem aceito pela comunidade. O que o TypeScript traz de diferente? Como o nome diz, ele é [strongly typed](https://en.wikipedia.org/wiki/Strong_and_weak_typing). Mas ele nao força você a ser Strongly Typed!

Em um momento que você está escrevendo uma aplicação com Javascript, seu front-end (ou back) começa a ficar muito grande! Isso é excelente! Mas as coisas começam a ficar mais complexas, e __Strong Type__ é uma excelente ajuda para evitar erros em [runtime](https://en.wikipedia.org/wiki/Run_time_(program_lifecycle_phase)).

Além disso, TypeScript também possui o conceito de Interfaces e Classes idêntico a como é da orientação a objeto e ele diminui mais ainda o código de como escreveríamos com ES6. 

E por fim, TypeScript também possui ferramentas para grande maioria das [IDE's](https://en.wikipedia.org/wiki/Integrated_development_environment) o que da a ele um grande diferencial comparado com os outros supersets de Javascript.

__Dart__: É um um mundo a parte, o melhor seria Dart (assim como TypeScript) possuir posts demonstrando um pouco suas características. Para apenas uma pequena explicação como nos outros Dart não é um "superset" de Javascript. Dart e TypeScript não são ["comparáveis"](http://programmers.stackexchange.com/questions/167070/differences-between-typescript-and-dart)link já que eles são bem distintos na abordagem para trabalhar com front-end

Um gráfico que demonstra bem o que tentei explicar acima seria esse:

![linguagens](http://alexander.holbreich.org/content/images/2016/01/typescript-es6-es5.png)

Mais um adendo, para saber como está a adoção das linguagens do Angular 2, eles disponibilizam um [gráfico](http://angularjs.blogspot.com.br/2015/09/angular-2-survey-results.html) como com essa informação, e TypeScript compõe 45% do total!

Nos exemplos aqui vamos de TypeScript. Pois além de ser a linguagem mais adotada (logo mais fácil de encontrar mais exemplos), também é a que o time do Angular 2 recomenda e realmente vale a pena usar ela.
