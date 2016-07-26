---
author: Rafael Miceli
comments: true
layout: default 
category : TypeScript
title: "TypeScript - Nice to Meet" 
date: 2016-07-26 10:00:00
---

No post anterior falei um pouco das linguagens usadas para desenvolver [Angular 2](https://angular.io/) e citei o [TypeScript](https://www.typescriptlang.org) até também por ter sido a principal escolha do time do Angular.

Neste post vou explicar um pouco do TypeScript para usar não só com o Angular 2 mas como também para qualquer aplicação de Javascript.

A primeira característica que faz do TypeScript um dos [supersets de Javascript](https://en.wikipedia.org/wiki/JavaScript#Use_as_an_intermediate_language) mais aceitos é que a migração para ele é extremamente fácil, uma vez que ele aceita seu próprio Javascript já escrito anteriormente.

### Playground

Para testar isso o TypeScript possui um [Playground](https://www.typescriptlang.org/play/index.html) para "brincarmos".

Teste o seguinte código no Playground, ou se achar melhor, jogue um código de Javacript que você tenha e teste.

```typescript
function Greeter(greeting) {
    this.greeting = greeting;
}

Greeter.prototype.greet = function() {
    return "Hello, " + this.greeting.message;
}

let greeter = new Greeter({message: "world"});  

let button = document.createElement('button');
button.textContent = "Say Hello";
button.onclick = function() {
    alert(greeter.greet());
};

document.body.appendChild(button);
```

Este código acima é puro Javascript, não tem nada da "Type" ai ainda.

### Substituir pelo Javascript?

Mas porque usar o TypeScript, devo substituir ele pelo Javascript? Deixo de aprender Javascript?

Bem, a noticia ruim é que eu e muitos outros experts recomendam que você não deixe aprender Javascript, de maneira alguma. 

1 - __Porque Javascript ainda é e será por um bom tempo mais usado que TypeScript.__ Não estou dizendo que o TypeScript vai substituir o Javascript, mas o trabalho que a comunidade do TypeScript faz contribui bastante para influenciar decisões das próximas versões do EcmaScript.

2 - __Para você entender o que seu TypeScript está fazendo.__ o código de TypeScript como TypeScript não roda no seu browser, no fim ele sempre vira Javascript. Você saber o que seu código está virando, ajuda para o caso de você encontrar algo que possa melhorar ou ajustar e contribuir para essas melhorias/ajustes já que o TypeScript é [__Open Source!__](https://github.com/Microsoft/TypeScript).

Ou seja, não deixa de estudar o Javascript (ainda...)

### Então sempre devo usar TypeScript?

Se você estiver em um projeto aonde você prevê que o front-end vá se tornar algo complexo (o que não é algo dificil hoje em dia) recomendo que você considere o TypeScript como opção. Claro que fatores como o time conhecer um pouco da linguagem influencia, ou uso de muitas bibliotecas de terceiros também, mas isso vai depender do seu projeto.

Outro fator a se analisar é caso você usar node no Back-End que também é possível escrever com TypeScript (afinal é Javascript)

Com o Angular recomendando o TypeScript é provável que a linguagem ganhe um boost de adoção para prjetos, o que já vem acontecendo:

![TypeScript Adocao](http://whatpixel.com/images/2016/04/typescript-google-trends-screenshot.jpg)

Agora, se você vai fazer um projeto com pouco código Javascript eu pessoalmente __hoje__ não usaria TypeScript. Novamente por fatores do time já saber usar a linguagem ou uso de bibliotecas externas.

### Vantagens

Mas agora, como disse anteriormente se você vai ter um projeto que você prevê uma quantidade significativa de Javascript vou mostrar algumas vantagens que o TypeScript te dá.

- Ele reduz o número de código de Javascript que você vai escrever, e __MUITO__. Para você ter uma ideia disso, vá ao [Playground](https://www.typescriptlang.org/play/index.html) e teste a herança.

Você pode testar com o codigo abaixo ou selecionando __Using Inheritance__ no Select do Playgorund.

```typescript
class Animal {
    constructor(public name: string) { }
    move(distanceInMeters: number = 0) {
        console.log(`${this.name} moved ${distanceInMeters}m.`);
    }
}

class Snake extends Animal {
    constructor(name: string) { super(name); }
    move(distanceInMeters = 5) {
        console.log("Slithering...");
        super.move(distanceInMeters);
    }
}

class Horse extends Animal {
    constructor(name: string) { super(name); }
    move(distanceInMeters = 45) {
        console.log("Galloping...");
        super.move(distanceInMeters);
    }
}

let sam = new Snake("Sammy the Python");
let tom: Animal = new Horse("Tommy the Palomino");

sam.move();
tom.move(34);
```

Repare no Javascript ao lado que ele reduz praticamente pela __METADE__ a quantidade de código!

- Ele é [Strongly Typed](https://en.wikipedia.org/wiki/Strong_and_weak_typing), o que é excelente para encontrar erros pela própria [IDE](https://en.wikipedia.org/wiki/Integrated_development_environment) antes de executar o código. Também te da o beneficio do [Intellisense](https://en.wikipedia.org/wiki/Intelligent_code_completion).
- É Fácil migrar uma aplicação de Javacript para TypeScript, uma vez que o TypeScript aceita qualquer código Javascript
- A curva de aprendizagem é relativamente curta, ainda mais se você já trabalhou com o conceito de orientação objeto.
- TypeScript te faz escrever código mais robusto usando o conceito de orientação a objeto.
- TypeScript possui __MUITO__ suporte para editores de texto e IDE's

Essas são alguma das vantagens, ainda vamos explorar mais o TypeScript aqui e ver que você pode fazer uso de [Interfaces](https://en.wikipedia.org/wiki/Interface_(computing)), [Generics](https://en.wikipedia.org/wiki/Generic_programming).

Próximo post vamos explorar um pouco o compiler do TypeScript e como criar um projeto.