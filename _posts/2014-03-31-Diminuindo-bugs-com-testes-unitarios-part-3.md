---
comments: true
layout: default
category : lessons
title: "Diminuindo bugs com testes unitários - part 3"
summary: "Antes de dar continuidade a saga dos testes unitários, vou precisar mencionar que... Não existem apenas..."
date: 2014-03-31 11:15:00
date_str: "31 mar 2014"
month_str: "Março"
previous_month_str: "Março"
---

Antes de dar continuidade a saga dos testes unitários, vou precisar mencionar que... Não existem apenas testes unitários para realizarmos na nossa aplicação...

 - Pergunta: **O QUE????** 
 - Resposta: Isso mesmo... Além de você fazer testes unitários, você também tem testes de integração, testes de aceitação, testes de carga e outros. 
 
Não vou listar todos os tipos de teste que conheço e nem descrever cada um deles. O foco agora são os **unitários**.

Eu sei também que deve estar aparecendo outra pulga na orelha agora que provavelmente é:
 - Pergunta: Vou precisar fazer todos eles???
 - Resposta: Depende da sua aplicação. 
 
Mas esse assunto fica pra outro artigo.


###Mas porque mencionar isso?

Bem, o motivo da menção de outros tipos de testes é para explicar uns princípios que aplicamos quando vamos fazer testes. Chamamos de F.I.R.S.T.

F.I.R.S.T. como da para ler é um acrônimo de 5 princípios, que são:
 - Fast
 - Isolated
 - Repeatable
 - Self-verifying
 - Timely.

Os princípios F.I.R.S.T. se aplicam diretamente a testes unitários. Pois logo o primeiro dos princípios (Fast) não é possível você realizar com testes de integração (já que testes de integração envolvem chamadas externas da sua aplicação como - Bancos de dados, Serviços e outros)



###Descrevendo melhor:

####Fast

Rápidos, são o que testes unitários tem de ser! Imagina se você tem um teste que leva 4 segundos para rodar, Uoa! Bem rápido. 
Agora imagina 100 que levam 4 segundos! mais de 6 minutos, imagina 1000 que levam 4 segundos! Mais de 1 hora. 
O correto é que seus testes sejam MAIS rápidos o possível. O ideal é milisegundos para todos.

####Isolated

Todos os testes devem ser isolados, ou seja, eles não dependem de outros testes para rodar e também testam uma única decisão do SUT (System Under Test). Deixando assim  sua aplicação respeitando mais ao S.R.P. (Single Responsability Principle). 
Uma boa observação é ver se no seu teste na parte da estrutura de "Assert", você está fazendo apenas um "Assert", mais de um indica que você está validando coisas diferente no seu teste.

####Repeatable

O teste tem de poder ser repetido inúmeras vezes sem alterar seu comportamento. O que quero dizer é que se você rodou seu teste segunda e funcionou. Na terça, sem fazer nenhuma modificação no código, quando você rodar o teste novamente, ele tem que funcionar!  

####Self-verifying

Os testes também devem se auto-validar, isto é, ou eles passam ou não passam! Sem nenhuma intervenção externa.

####Timely

Escreva seus teste ANTES do código! Simples não? Bem, neste último princípio vamos acabar entrando em TFD - Test First Development. Para testes unitários este tipo de princípio é mais conhecido como TDD - Test Driven Development. Timely vou me aprofundar mais em outro artigo.



No próximo darei exemplos de testes unitários com os princípios F.I.R.S.T.
