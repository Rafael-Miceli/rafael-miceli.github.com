---
comments: true
layout: post
category : lessons
title: "Diminuindo bugs com testes unitarios - part 1"
summary: "No ultimo artigo compartilhei alguns passos que ajudam em momentos que empacamos quando estamos desenvolvendo nossas..."
date: 2014-03-13 19:15:00
date_str: "13 mar 2014"
month_str: "Março"
previous_month_str: "Dezembro"
tags : [intro, beginner, jekyll, tutorial]
---
{% include JB/setup %}

No último artigo compartilhei alguns passos que ajudam em momentos que empacamos quando estamos desenvolvendo nossas aplicações. Em um dos passos comentei a respeito de testes unitários, e vale a pena compartilhar como o uso de testes unitários ajuda muito a diminuirmos exponencialmente os bugs da nossa aplicação - além de ajudar em outras coisas mais ( vou falar mais disto em outro artigo )

### O Que é?

Bem antes de qualquer coisa a definição de teste unitário mais detalhada está no [wikipedia] (http://en.wikipedia.org/wiki/Unit_testing) . Em essência um teste unitário é uma forma de automatizar os testes que fazemos em nossa aplicação. 

Nós as vezes repetimos o mesmo teste referente a uma tarefa mais de 10 vezes (supondo que fizemos uma pequena correção em cada teste). Automatizando isso com o click de um botão e saber se tudo esta funcionando seria excelente, não? Além do que, por termos automatizado o teste nos prevenimos contra a regressão de problemas. Algo muito comum em uma refatoração de código ou mudança de requisito.
 
### Uma das vantagens:
 
quando você esta fazendo sua aplicação o fluxo comum é:
 
analisar o problema, criar o código para soluciona-lo, e testa a aplicação. 
se o código criado não gerar nenhum erro em runtime ou não agir de forma esperada (o que não é difícil) então você tenta novamente e novamente e assim vai.
A parte "chata" é no momento de testar a sua aplicação, e principalmente dependendo de como for para testa-la.

Ex: Digamos que você vá testar uma aplicação, e sempre que você executa ela, você precisa se logar primeiro. Logo após vc precisa acessar a página necessária e preencher um formulário com dados. Após preencher os dados, vc clica em um botão e precisa testar se gravou tudo correto. Agora imagina isto repetido 10 vezes?

Além de consumir bastante seu tempo torna o trabalho mais estressante, podendo levar você a procurar a solução mais rápido o possível (Mas que pode dar uma temperada com azeite no seu código)
 
### Exemplos de teste unitário

Um exemplo clássico para mostrar seria caso você quisesse que na sua aplicação, você pudesse exibir o "nome" e o "sobrenome" do seu cliente juntos, mas sendo "nome" e o "sobrenome" fields diferentes na sua aplicação (por favor atenha-se a abstração de eu não ter criado getters e setters):

Aplicação:

public class Principal {

    public static void main(String args[]){

        Cliente cliente = new Cliente();

        cliente.Nome = "Ash";
        cliente.Sobrenome = "Ketchun";

        System.out.println(cliente.RetornarNomeMaisSobrenome());
    }
}

class Cliente{
    public String Nome;
    public String Sobrenome;

    public String RetornarNomeMaisSobrenome(){
        return Nome + " " + Sobrenome;
    }
}

==================================================================

Teste:

public class ClienteTest {

    @Test
    public void Test_Retornar_Nome_Mais_Sobrenome() {

        String nome = "Meu";
        String sobrenome = "Nome";

        Cliente cliente = new Cliente();

        cliente.Nome = nome;
        cliente.Sobrenome = sobrenome;

        Assert.assertEquals(nome + " " + sobrenome, cliente.RetornarNomeMaisSobrenome());
    }
}


No próximo artigo vou abordar mais a pratica de testes unitários.
