---
comments: true
layout: post
category : lessons
title: "Bois dos testes unitários"
summary: "Beleza. Achei legal até agora testes unitários e decidi começar a praticar um pouco..."
date: 2014-06-04 15:15:00
date_str: "04 jun 2014"
month_str: "Junho"
previous_month_str: "Maio"
---
{% include JB/setup %}

Beleza. Achei legal até agora testes unitários e decidi começar a praticar um pouco deles.

Mas.... por onde começo?

Qual ferramenta uso? por que vejo diferenças em coisas que cada um escreve? isEqual, AreEqual.

Bem vou mostrar o nome dos bois a mais High Level, vou usar o C# como exemplo. Então para python, ruby, javascript ou outras ainda não conheço muito bem os frameworks e Ides de teste unitários. E se você poder me ensinar fico grato =].

###Bois

No C# usamos como ambiente de desenvolvimento o *Visual Studio*, nossa IDE.

Testes unitário em C# fazemos criando uma nova Class Library e nela rerenciando o *MSTests* da microsoft. Pera lá.... So existe o MSTESTS? Bem.... Não....

Nós temos o *Nunit*, o *xUnit* e outros além. Estes kras são *Test Frameworks*. Se você não quiser usar o MSTests você pode baixar outro framework e referenciar o mesmo. Dependendo de quem você usar seus Asserts vão ser diferentes.

Exemplo:

No MSTest o Assert para validar igualdade é "Assert.AreEqual(expected, actual)"

No xUnit é "Assert.Same(expected, actual)"

Já vi também "valor.Should.BeEqual(expected)"

Os frameworks possuem classes e functions para te ajudar fazer uma série de coisas, este é um exemplo de uma delas.

Tem também a IDE de teste unitário. o Visual Studio ja vem com uma, o Nunit tem a sua e até o Resharper tem uma (o que uso atualmente por sinal).

Você usa a que você se sentir confortável, tem bastante opção por ai.

Só para vocês terem uma idéia da quantidade aqui vai uma listinha amiga do [wikpedia] (http://en.wikipedia.org/wiki/List_of_unit_testing_frameworks#.NET_programming_languages)

Para comparar um pouco e você entender melhor os Frameworks no .Net também existem no Java. 

Um dos mais famosos é o JUnit, este é o que venho usando nos exemplos ultimamente. Nele o Assert para validar igualdade se vocês lembram é o "Assert.isEqual(expected, actual)"

São todos bem parecidos.

Compreendendo tudo isso.

Em testes unitários você tem as *IDEs* para analisar, rodar e outras coisas mais nos testes. E os *frameworks*, que já são "bibliotecas" já prontas para te auxiliar na criação dos seus testes.

Ainda tem também frameworks para Test Doubles, em C# por exemplos temos Moq, FakeitEasy e outros também.

Mas este tópico fica para outro artigo.

Nó próximo vou finalizar o principio do F.I.R.S.T. com o Timely.




