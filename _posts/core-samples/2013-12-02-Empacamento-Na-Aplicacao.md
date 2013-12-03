---
comments: true
layout: post
category : lessons
title: "Empacou no codigo? Aqui vao algumas dicas..."
summary: "Doug Bradbury escreveu ha algum tempo um artigo sobre como resolver os problemas que empacam agente no codigo..."
date: 2013-12-02 18:50:00
date_str: "2 dez 2013"
month_str: "Dezembro"
previous_month_str: "Novembro"
tags : [intro, beginner, jekyll, tutorial]
---
{% include JB/setup %}

[Doug Bradbury] (http://www.8thlight.com/our-team/doug-bradbury) escreveu ha algum tempo um artigo sobre como resolver os problemas que empacam agente no código

Ele lista alguns passos para conseguir achar a solução para desempacar daquela situação que eh difícil sair. Mesmo as vezes sendo uma coisa simples ou aquele detalhe que fica esquecido.

Eu passo por essa situacao DIVERSAS vezes. Vou listar os passos que ele partilhou, mais algumas coisas que eu tambem aprendi com os passos dele e com outras tecnicas.


### 1 - Leia a mensagem de erro.


### 2 - SERIO, LEIA A MENSAGEM DE ERRO


Sim, sao passos repetidos. Mas para enfatizar bem que geralmente o problema que esta acontecendo esta sendo exibido bem abaixo do nosso nariz! Nao precisa ficar tentando adivinhar o que possa ser. Eh so ler a mensagem, e o stack trace, ele geralmente ja te da o caminho.


### 3 - Verifique suas anotacoes sobre este erro


Anote os erros complicados de se resolver em algum lugar. Recomendo o evernote, e posso falar, AJUDA!

Eu venho anotando ultimamente os erros escabrosos que tomo no evernote e ja passei por um deles mais de uma vez! Kra como ajudou ter anotado! Infelizmente não tenho memoria de elefante como o Evernote.

Para quem quiser outro sem ser o Evernote, tambem tem o OneNote, alem de outros.


### 4 - Crie um teste unitario


Se voce esta tendo um erro desses aparecendo no seu codigo em runtime, eh porque provavelmente ele nao foi coberto com um teste. E dependendo da sua aplicacao, a maneira de reproduzir este erro usando o programa, pode ser trabalhosa. Geralmente um teste unitario facilita muito, tanto a reproducao do erron quanto a verificao se funcionou as correcoes que foram aplicadas. Tambem ajuda a entender melhor o que esta acontecendo no codigo. Sem contar que apos isso, seu codigo estara coberto pelo teste, ou seja se o erro aparecer novamente, seu teste antes vai indica-lo para voce.


### 5 - Google It!


Busque no google a mensagem de erro que apareceu para voce, lembrando de remover tudo que for especifico ao seu codigo, como nomes de arquivo, nomes de metodos e outros.


### 6 - Leia o resulado da busca


### 7 - SERIO, LEIA O RESULTADO DA BUSCA


Parece que ler eh a parte mais dificil em solucionar um problema. Ja fiz buscas onde ao inves de eu parar para ler direito o que alguem havia comentando de como solucionou o problema, eu so dava uma rapida olhada, e buscava em outros lugares, e em outros lugares, e quando ja estava saturado e decidia ler com mais calma, ESTAVA TODO TEMPO NO PRIMEIRO LUGAR QUE ACHEI.... Ou seja, busque no Google. Quando ver algum link que parece ter a resposta clique nele e LEIA O ARTIGO, nao apenas role o scroll pela pagina. Leia!


### 8 - Pergunte ao seu pet


Sim, pergunte a um Pet que voce tenha, talvez uma Capivara na sua mesa ne Katia? Ou um pato, ou mesmo ao seu gato ou seu cachorro, mas pergunte em voz alta! Quando você pergunta em voz alta, seu cerebro compreende melhor o problema, parece loucura ne? Mas pense nas vezes que so de falar com alguem que nao entenda nada do que voce esta dizendo, ja eh o suficiente para te trazer aquele A-HA!


### 9 - Pergunte a um amigo


Pergunte a um amigo seu, que conheca a mesma tecnologia que voce, ou que conheca o problema, de preferencia ate que ja tenha passado por isso.


10 - Pergunte a um expert

Pergunte a algum expert que voce conheca, e se ainda assim a pessoa nao souber, va ao [StackOverflow] (http://stackoverflow.com/). [Scott Hanselman] (https://twitter.com/shanselman) e [Rob Conery] (https://twitter.com/robconery) tem um otimo [video entrevistando Jon Skeet] (http://www.pluralsight.com/training/Courses/TableOfContents/get-involved), sobre como fazer perguntas no StackOverflow.


### 11 - Anote sua solucao


Lembre-se de apos achar a solucao do seu problema, anote ela! Use o evernote ou OneNote que sincronizam suas notas em qualquer lugar. Melhor ainda BLOG! Escreva em um blog e compartilhe com as pessoas o problema que voce passou. Voce pode ate receber solucoes melhores e mais elegantes que as suas para seus problema. E nao leve isso como demonstracao de fraqueza. Na realidade a coisa que mais se tem que ter forcas eh admitir suas fraquezas.

E se vocês tiverem algum comentario, ou maneira de melhorar a solucao de resolver seus empacamentos, compartilhem aqui, ou postem o link de seus artigos!
