---
comments: true
layout: post
category : lessons
title: "Diminuindo bugs com testes unitários - part 1"
summary: "No último artigo compartilhei alguns passos que ajudam em momentos que empacamos quando estamos desenvolvendo nossas..."
date: 2014-03-15 19:15:00
date_str: "15 mar 2014"
month_str: "Março"
previous_month_str: "Dezembro"
tags : [intro, beginner, jekyll, tutorial]
---
{% include JB/setup %}

No último artigo compartilhei alguns passos que ajudam em momentos que empacamos quando estamos desenvolvendo nossas aplicações. Em um dos passos comentei a respeito de testes unitários, e vale a pena compartilhar como o uso de testes unitários ajuda muito a diminuirmos exponencialmente os bugs da nossa aplicação - além de ajudar em outras coisas mais ( vou falar mais disto em outro artigo )

### O Que é?

Bem antes de qualquer coisa a definição de teste unitário mais detalhada está no [wikipedia] (http://en.wikipedia.org/wiki/Unit_testing) . Em essência um teste unitário é uma forma de automatizar os testes que fazemos em nossa aplicação. 

Nós as vezes repetimos o mesmo teste referente a uma tarefa mais de 10 vezes (supondo que fizemos uma pequena correção em cada teste). Automatizando isso com o click de um botão e saber se tudo está funcionando seria excelente, não? Além do que, por termos automatizado o teste nos prevenimos contra a regressão de problemas. Algo muito comum em uma refatoração de código ou mudança de requisito.

