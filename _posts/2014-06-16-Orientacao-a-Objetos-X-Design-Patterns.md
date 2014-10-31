---
comments: true
layout: default
category : lessons
title: "Orientação a Objeto X Design Patterns"
summary: "Um amigo foi a uma entrevista e se deparou com as seguintes perguntas..."
date: 2014-06-16 11:45:00
date_str: "16 jun 2014"
month_str: "Junho"
previous_month_str: "Junho"
---
{% include JB/setup %}

Um amigo foi a uma entrevista e se deparou com as seguintes perguntas:

	Entrevistador: Você conhece padrões de projeto? (Design Patterns)

	Amigo: Não.

	Entrevistador: Você conhece orientação a objetos? (OO)

	Amigo: Tenho um conhecimento avançado em OO

	Entrevistador: É contraditório você possuir conhecimento avançado em OO e não possuir em design patterns.

Logo nesse momento a dúvida que bateu foi: O que OO tem a ver com design patterns?

Certo antes de qualquer coisa vamos dar uma olhada em uma básica definição do que vamos abordar:

###Desgin Patterns

[Design Patterns] (http://en.wikipedia.org/wiki/Design_Patterns) ou padrões de projeto em nossa língua, é a observação de soluções reutilizáveis dentro de um contexto para problemas recorrentes. É como se fosse uma fórmula para resolver um determinado problema.
Para tentar facilitar a explicação, imagine que você quer descobrir a soma dos número de 1 a 100, você pode fazer a soma um a um, o que vai dar um p*** trabalho ou pode usar o padrão matemático que é a P.A. (Progressão Aritmética).

###Orientação a Objeto
A Orientação a objeto como diz no [wikipedia] (http://en.wikipedia.org/wiki/Object-oriented_programming) é um [paradigma de programação] (http://en.wikipedia.org/wiki/Programming_paradigm), ou seja, uma forma de você "programar" falando de forma muito mais simples.

###Relação

Beleza e agora qual a relação entre os dois? 

Bem, infelizmente a maioria em OO lembra somente de sobrecarga, herança e polimorfismo. E ponto. Mas esse é somente o início da OO. Na OO além dos citados acima, você possui os princípios S.O.L.I.D. também.

E para você aplicar estes princípios, você acaba fazendo o uso design patterns e muito.

Ou seja, se você possui conhecimentos avançados em Orientação a Objetos você acaba conhecendo as Design Patterns, por que você aplica muitas delas, para estar de acordo com os conceitos da Orientação a Objeto

###SOLID

O [S.O.L.I.D.][1], vem de:

- Single Responsibility Principle (SRP)
- Open Closed Principle (OCP)
- Liskov Substitution Principle (LSP)
- Interface Segregation Principle (ISP)
- Dependency Inversion Principle (DIP)

Estes são cinco princípios extremamente importantes para a OO. Robert C. Martin cita eles em seu livro [Agile principle, patterns and practices in C#] (http://www.amazon.com/Agile-Principles-Patterns-Practices-C/dp/0131857258/ref=sr_1_3?ie=UTF8&qid=1402929321&sr=8-3&keywords=robert+c+martin). Eu o recomendo MUITO.

Dando um breve exemplo da relação entre OO e design patterns, se vc está querendo obedecer ao DIP, muito provavelmente você irá usar repository pattern na sua aplicação. Para explicar o por que, vou fazer uma série explicando os básicos do S.O.L.I.D..

Mas lembrem-se, Oreintação a Objetos avançado acaba fazendo o uso de Design Patterns.


[1]: http://en.wikipedia.org/wiki/Solid_(object-oriented_design)


