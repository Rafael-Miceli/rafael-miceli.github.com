---
author: Rafael Miceli
comments: true
layout: default
category : lessons
title: "Empacou no código? Aqui vão algumas dicas..."
summary: "Doug Bradbury escreveu ha algum tempo um artigo sobre como resolver os problemas que empacam agente no código..."
date: 2013-12-02 18:50:00
date_str: "2 dez 2013"
month_str: "Dezembro"
previous_month_str: "Novembro"
---

[Doug Bradbury] (http://www.8thlight.com/our-team/doug-bradbury) escreveu há algum tempo um artigo sobre como resolver os problemas que empacam a gente no código.

Ele lista alguns passos para conseguir achar a solução para desempacar daquela situação que é difícil de sair. Mesmo às vezes sendo uma coisa simples ou aquele detalhe que fica esquecido.

Eu passo por essa situação DIVERSAS vezes. Vou listar os passos que ele partilhou, mais algumas coisas que eu também aprendi com os passos dele e com outras técnicas.


### 1 - Leia a mensagem de erro.


### 2 - SÉRIO, LEIA A MENSAGEM DE ERRO


Sim, são passos repetidos. Mas para enfatizar bem que geralmente o problema que está acontecendo está sendo exibido bem abaixo do nosso nariz! Não precisa ficar tentando adivinhar o que possa ser. É so ler a mensagem e o stack trace, pois, ele geralmente já te da o caminho.


### 3 - Verifique suas anotações sobre este erro


Anote os erros complicados de se resolver em algum lugar. Recomendo o evernote, e posso falar, AJUDA!

Eu venho anotando ultimamente os erros escabrosos que tomo no evernote e já passei por um deles. Mais de uma vez até! E, cara, como ajudou ter anotado! Infelizmente, não tenho memória de elefante como o Evernote.

Para quem quiser outro sem ser o Evernote, também tem o OneNote, além de outros.


### 4 - Crie um teste unitário


Se você está tendo um erro desses aparecendo no seu código em runtime, é porque, provavelmente, ele não foi coberto com um teste. E dependendo da sua aplicação, a maneira de reproduzir este erro usando o programa, pode ser trabalhosa. Geralmente, um teste unitário facilita muito, tanto na reprodução do erro quanto na verifição se funcionou as correções que foram aplicadas. Também ajuda a entender melhor o que está acontecendo no código. Sem contar que após isso, seu código estará coberto pelo teste, ou seja, se o erro aparecer novamente seu teste antes vai indicá-lo para você.


### 5 - Google It!


Busque no google a mensagem de erro que apareceu para você, lembrando de remover tudo que for específico ao seu código, como nomes de arquivo, nomes de métodos e outros.


### 6 - Leia o resulado da busca


### 7 - SÉRIO, LEIA O RESULTADO DA BUSCA


Parece que ler é a parte mais difícil em solucionar um problema. Já fiz buscas em que ao invés de eu parar para ler direito o que alguém havia comentando de como solucionou o problema, eu só dava uma rápida olhada. Depois buscava em outros lugares, e em outros lugares, e em outros lugares, até ficar saturado. Depois de saturado, que eu decidia ler com mais calma, ESTAVA TODO O TEMPO NO PRIMEIRO LUGAR QUE ACHEI.... Ou seja, busque no Google. Quando ver algum link que parece ter a resposta clique nele e LEIA O ARTIGO, não apenas role o scroll pela página. Leia!


### 8 - Pergunte ao seu pet


Sim, pergunte a um Pet que você tenha, talvez uma Capivara na sua mesa (né Katia)? Ou um pato, ou mesmo ao seu gato ou seu cachorro, mas pergunte em voz alta! Quando você pergunta em voz alta, seu cérebro compreende melhor o problema. Parece loucura, né? Mas pense nas vezes que você falou com alguém que não entendia nada do que você estava dizendo, e de você tentar explicar o assunto, já é o suficiente para te trazer aquele A-HA!


### 9 - Pergunte a um amigo


Pergunte a um amigo seu que conheça a mesma tecnologia que você ou que conheça o problema, de preferência até que já tenha passado por isso.


### 10 - Pergunte a um expert

Pergunte a algum expert que você conheça e, se ainda assim a pessoa não souber, vá ao [StackOverflow] (http://stackoverflow.com/). [Scott Hanselman] (https://twitter.com/shanselman) e [Rob Conery] (https://twitter.com/robconery) tem um ótimo [vídeo, entrevistando Jon Skeet] (http://www.pluralsight.com/training/Courses/TableOfContents/get-involved), sobre como fazer perguntas no StackOverflow.


### 11 - Anote sua solução


Lembre-se: Após achar a solução do seu problema, anote!
Use o evernote ou OneNote que sincronizam suas notas em qualquer lugar. Melhor ainda: BLOGUE! Escreva em um blog e compartilhe com as pessoas o problema que você passou. Você pode até receber soluções melhores e mais elegantes que as suas para seus problemas. E não leve isso como demonstração de fraqueza. Na realidade, é preciso ter forças é admitir suas fraquezas.

E se vocês tiverem algum comentário ou maneira de melhorar a solução para resolver seus empacamentos, compartilhem aqui, ou postem o link de seus artigos!
