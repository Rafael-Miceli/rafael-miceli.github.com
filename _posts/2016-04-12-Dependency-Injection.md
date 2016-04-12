---
author: Rafael Miceli
comments: true
layout: default 
category : solid 
title: "Dependency Injection" 
date: 2016-04-12 09:00:00
---
 
Uma vez que aplicamos DIP em nosso sistema, vamos iniciar o grafo de dependências do mesmo.

### Quando criamos uma dependência?

Dependência em uma aplicação .Net C# é quando uma classe __A__ precisa de alguma funcionalidade que esta em outra classe __B__, para isso a classe __A__ vai precisar criar uma instancia da classe __B__, a criação dessa instância com a palavra-chave _new_ cria uma ligação de dependência direta de A para B. Em outras palavras, __A__ depende de __B__.

Fora as classes primitivas da nossa linguagem como _int_, _string_, _bool_ e outras, todo o resto podemos considerar como dependência (_DateTime_ também é uma dependência)

Uma forma simples de encontrar uma dependência em nossa classe é sempre quando vemos uma nova instância sendo feita com a palavra-chave _new_.

### Dependency Injection (D.I.)

Se notaram no código do post anterior, toda dependência de nossas classes ficavam em seus construtores. E pediam as mesmas por parâmetros do construtor.

![MateriaService](http://rafael-miceli.com.br/ico/Dependency-Injection/MateriaService.png)

Exemplo em nosso __MateriaService__ que temos um dependência com __IMateriaRepo__, mas pedimos a mesma através do construtor, repare também que temos dependência na maioria dos casos em uma Interface e não em uma classe, mas essa explicação do porque disso vem em outro post.

Pedir dependências pelo construtor é uma forma de injetarmos dependência em nossa classe. Chamamos essa forma de injetar dependência de __Constructor Injection__. Essa é a forma mais comum de injeção de dependência, e uma muito boa, pois assim, sabemos todas as dependência que nossa classe vai ter olhando apenas seu construtor.

### Composition Root

Mas aonde começamos a instanciar nossas dependências? Em algum lugar temos que começar as instancias dessas classes. Nós fazemos isso em nosso __Composition Root__.

__Composition Root__, como o nome já diz, é a raiz de composição de nossa aplicação. Para cada tipo de aplicação o __Composition Root__ vai estar em um lugar diferente.

Por exemplo em uma aplicação MVC, se estiver usando OWIN, vai ser no método __Configuration__ no  __Startup.cs__. Caso não esteja usando OWIN vai ser no **Application_Start** do __Global.asax__.

### Poor Man's Dependency Injection

Para apenas executar a aplicação com Dependency Injection uma forma fácil é fazer um _Poor Man's Dependency Injection_.

Para aplicações de mundo real do dia a dia, __EVITE FAZER ISSO__. Essa é uma técnica que usamos apenas em situações bem específicas de aplicações do mundo real, mas uma aplicação _Green Field_ pode se beneficiar de Dependency Injection com um Framework de __D.I. Container__ (veremos em um próximo post).

Para fazer o Poor Man's em um aplicação MVC, inicie as instância do seu grafo de dependências no construtor de seu controller. Exemplo:

MateriaController.cs

```csharp
public class MateriaController : ApiController
{
    private IMateriaService _materiaService;

    public MateriaController()
    {
        var materiaRepo = new MateriaRepository();
        _materiaService = new MateriaService(materiaRepo);
    }

    // POST api/values
    public HttpResponseMessage Post([FromBody]Materia materia)
    {
        try
        {
            _materiaService.CriarNovaMateria(materia);

            return Request.CreateResponse(HttpStatusCode.Created);
        }
        catch (Exception ex)
        {
            return Request.CreateErrorResponse(HttpStatusCode.BadRequest, ex.Message);
        }
        
    }
    
}
```

Com isso temos nossa aplicação com Dependency Injection rodando, mas como disse, __EVITE FAZER ISSO__. No próximo post vamos ver uns __D.I. Containers__ para .Net.
