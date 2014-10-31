---
author: Rafael Miceli
comments: true
layout: default
category : lessons
title: "Diminuindo bugs com testes unitários - part 2"
summary: "No artigo anterior expliquei um pouco sobre teste unitário e como..."
date: 2014-03-21 19:15:00
date_str: "21 mar 2014"
month_str: "Março"
previous_month_str: "Março"
---

No [artigo anterior] (http://rafael-miceli.github.io/lessons/2014/03/15/Diminuindo-bugs-com-testes-unitarios-part-1/) expliquei um pouco sobre teste unitário e como eles ajudam a diminuir bugs na sua aplicação.

Uma das vantagens que citei foi a do fato de você poder automatizar aquele seu velho e chato teste que você fazia rodando a aplicação.

Aqui vai mais duas vantagens:

Uma que citei pouco é a ajuda em **prevenir recorrência de bugs**. Você não precisa mais se preocupar em voltar a ter o mesmo bug de antes, já que ele agora está coberto por um teste unitário. Antes do bug aparecer na tela o teste vai acusar que ele vai voltar acontecer, assim podendo corrigir o problema antes de aparecer para seu cliente.

Outra é te deixar muito mais seguro ao fazer qualquer refactory no código, pois uma vez automatizados os testes, com um clique do botão você testa TUDO em questão de segundos (ou minutos dependendo de quantos testes você tiver e a forma que eles rodam)

Aqui vai uma prática que seguimos para nos ajudar a escrever bons testes.

### A-A-A

Arrange - Act - Assert é uma pattern para nos ajudar a subdividir melhor a estrutura dos nossos testes unitários

Revendo o teste anterior, eu reescrevi ele de forma para ficar mais fácil de ver esta estrutura:

####Teste:
```java
public class ClienteTest {

	@Test
	public void Test_Retornar_Nome_Mais_Sobrenome() {
		
		//Arrange
		String nome = "Meu";
		String sobrenome = "Nome";

		Cliente cliente = new Cliente();

		cliente.Nome = nome;
		cliente.Sobrenome = sobrenome;

		//Act
		String nomeMaisSobrenome = cliente.RetornarNomeMaisSobrenome();          

		//Assert
		Assert.assertEquals(nome + " " + sobrenome, nomeMaisSobrenome);
	}
}
```

Esta estrutura nos ajuda a montar o teste unitário.

### Explicando melhor

Arrange: é onde nós preparamos tudo que precisamos no teste. Neste caso nós precisamos instanciar a classe a qual vamos chamar o método "RetornarNomeMaisSobrenome()"  e que vai ser usada no "Act", (O objeto que trabalhamos no "Act" também chamamos de SUT - System Under Test) também precisamos criar duas variáveis - nome e sobrenome - que são os valores que esperamos que o método do SUT retorne (neste caso o método "RetornarNomeMaisSobrenome").

Act: é onde nós chamamos o método que queremos testar do SUT e guardamos o valor que ele retorna em alguma variável.

Assert: é onde nós verificamos se o valor retornado do SUT é igual ao nosso valor esperado (Neste caso estamos verificando se o valor esperado - nossas variáveis nome e sobrenome - são iguais ao valor retornado pelo SUT, mas, lembre-se que podemos testar também se o valor que o SUT está retornando "não é nulo", ou se "é verdadeiro").

Com essa estrutura já fica bem mais fácil de nos organizarmos. O melhor é tentar manter cada teste unitário pequeno e simples, para ele ser realmente unitário. 
Também observe que se seu teste unitário está muito grande e complexo ele está querendo te dizer que o código que você criou também está complexo, e que uma refatoração provavelmente seria bom.

Daremos continuidade em testes unitários no próximo artigo.
