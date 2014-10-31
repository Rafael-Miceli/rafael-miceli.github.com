---
comments: true
layout: default
category : lessons
title: "Repeatable e..."
summary: "O princípio Repeatable da nossa saga (F.I.R.S.T.) indica que todo o..."
date: 2014-05-16 16:15:00
date_str: "16 mai 2014"
month_str: "Maio"
previous_month_str: "Abril"
---

O princípio Repeatable da nossa saga (F.I.R.S.T.) indica que todo o teste tem de poder ser repetido SEMPRE! 

Isto independente de ordem que você execute os testes. 

Eles não podem:
 - Ser dependentes de nenhum estado inicial para rodar (Ex: Variavel booleana para saber se pode fazer Asserts ou mesmo inicializar o teste)
 - Deixar resíduos para trás de forma que na próxima vez que for executar os testes, os mesmos não funcione (Ex: em bancos de dados - Connection strings abertas, Transactions não finalizadas - , isso também vale para acesso a arquivos ou serviços externos)
 - Depender de situações de ambiente ou mesmo bibliotecas externas.

Ou seja, eles devem rodar independente de qualquer ambiente externo que possa influência-los, mesmo sem internet, mesmo sem rede local, mesmo sem bibliotecas externas que estejam linkadas a sua aplicação (como entity framework, Hibernate, ou qualquer outra que não esteja dentro do escopo do contexto do seu teste)

### Mas qual motivo disto?

Com seus testes sendo independentes (unitários) nesse nível, você os roda com *MUITO* mais frequência, já que eles não lhe causam problemas e pelo contrário, te mostram quando aparece um.

### E como posso atingir isto?

Como mostrado no artigo [Fast] (http://rafael-miceli.github.io/lessons/2014/04/16/Fast/) os [Test Doubles] (http://www.martinfowler.com/bliki/TestDouble.html) são excelentes para isto. Fakes e Mocks são os caras que você mais vai usar para garantir uma boa [Isolação] (http://rafael-miceli.github.io/lessons/2014/04/29/Isolated/) dos seus testes.

Nó próximo artigo: Self-Validating.

Não, espera.... Self-Validating é um princípio bem mais simples, vai aqui mesmo.

 

## SELF-VALIDATING

Self-Validating é um princípio que já é praticamente implicito em testes unitários, mas que mesmo assim deve ser lembrado para não criarmos azeites em nossos testes.

Self-Validating é para lembrarmos que os testes são Pass-Fail, ou seja, eles se auto-validam se passam ou não. Nenhum outro teste ou código pode fazer isto por ele. Ele mesmo deve Faze-lo.

Sim é isto. É praticamente - "os testes validam si mesmo, não mande outro fazer o trabalho dele"!

Agora sim, no próximo artigo Timely.




