---
author: Rafael Miceli
comments: true
layout: default 
category : Lean
title: "Algoritmando - Encontrar a soma dos números" 
date: 2017-02-09 17:00:00
---

Eis um problema que peguei no [CodeFight](https://codefights.com) para resolver e achei divertido.

O nome do problema é __Count Sum Of Two Representations__ e ele consiste no seguinte:

>Dado três números n, l  e r encontre os inteiros que representem a soma de A e B aonde l <= A <= B <= r. Por exemplo para n = 6, l = 2 e r = 4 o resultado da função countSumOfTwoRepresentations(n, l, r) = 2.

A explicação é a seguinte que para os números entre 2 e 4 as somas possíveis que podem ser igual a 6 são: 2 + 4 e 3 + 3

### Analisando o problema

Uma forma que podemos resolver esse problema como alguns programadores resolveria seria da seguinte maneira:

```python
tot = 0
for num in range(l, r + 1):        
    for nu in range(num, r + 1):
        if num + nu == n:
            tot += 1
return tot
```
O problema é que essa resposta nos da péssima performance por nos dar __O(n²)__, o que é um interação dentro de um iteração.

Uma outra forma que podemos calcular é com o seguinte código:

```python
tot = 0
for num in range(l, r + 1):                
    if num <= n - num <= r:
        tot += 1
return tot
```

Neste caso agora, bem melhor, temos __O(n)__. 

No algoritmo acima fazemos de l a r e caso o numero que estamos analisando seja menor ou igual a (n - o numero) e o mesmo seja menor ou igual a r adicionamos ele.

Em nosso exemplo de n = 6, l = 2 e r = 4, na primeira iteração vamos ter o `num = 2` e nosso teriamos um `if 2 <= 6 - 2 <= 4` então soma.

Mas ainda assim podemos ter algo melhor!

### Encontrando uma formula

Vamos queimar um pouco a mufa, se não tivessemos a regra de que a somas dos números para ser 6 deveria estar entre l (2) e r (4) nós temos a fórmula:

somasDeN = n/2 + 1, essa formula vem de observarmos que as somas de n = 6 seriam:

(0,6)(1,5)(2,4)(3,3)

Ok, temos uma formula `n/2 + 1`, agora precisamos saber como acrescentar as regras.

Vamos por partes, primeiro vamos com a regra de que tem que ser acima do l aonde l = 2

Neste caso queremos queseja incluido na soma apenas o (2,4) e o (3,3)

(0,6)(1,5)__(2,4)(3,3)__

Para isso usamos a formula `n/2 + 1 - l`, ou seja, sempre que definirmos que um número vai começar a partir de l, subtraimos a formulá da soma de l.

Agora falta para o r. Neste caso não podemos passar do valor de r.

Se novamente formos por partes e procurarmos uma forma para as somas com o limite de r encontramos `r - n/2 + 1`. 

Aplicando novamente em nosso caso aonde r = 4 vamos ter também o (2,4) e o (3,3).

Ok, agora precisamos unir esses dois caras. Repare que o que precisamos é encontrar quem desses o `n/2 - l + 1` ou `r - n/2 + 1` vai ser o menor para sabermos quantas somas podemos fazer de n no intervalo entre l e r!

Então temos o seguinte algoritmo:

```python
result = min((n/2 - l + 1), (r - n/2 + 1))    
return 0 if result < 0 else int(result)
```

E este nosso algoritmo agora é __O(1)__!!! 

Podem comparar quem é o mais performático! Sem loop não tem dúvidas. Se quiserem o código deste exemplo esta em meu [github](https://github.com/Rafael-Miceli/AlgorithStudy/blob/master/Python/Count%20Sum%20of%20Two/problem.py)





