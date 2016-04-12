---
author: Rafael Miceli
comments: true
layout: default 
category : solid 
title: "S.O.L.I.D. DIP (Dependency Inversion Principle)" 
date: 2016-04-11 22:00:00
---
 
Neste post vou explicar um pouco qual a ideia por trás de um princípio extremamente importante, o principio da __Inversão de Dependência__ (_Dependency Inversion Principle_).

Para explicar melhor, nada como um bom exemplo.

### Divisão em camadas

Imagine a seguinte situação.

Digamos que temos uma aplicação e que a mesma sabemos que vai se tornar um sistema grande. No geral usamos o conceito de divisão de 3 camadas para ficar mais simples organizarmos as responsabilidades de nossa aplicação.

A ideia é que nosso sistema é dividido em 3 camadas: 

> Lembrando que essa é uma representação moderada. Para arquiteturas de sistemas maiores existem outras abordagens para a divisão das camadas. 

1 - _Presentation_ (Apresentação)

2 - _Domain_ ou _Business Logic_ (Domínio)

3 - _Data_ (Dados)

As responsabilidades de cada uma são respectivamente:

_Presentation_ - Possui todo seu código de apresentação, para fácil entendimento, em um projeto usando .Net C# essa seria nossa aplicação MVC

_Domain_ - É uma class library aonde fica a responsabilidade de toda lógica de regra de negócios do nosso sistema. 

_Data_ - Nesta camada, que também é uma class library, implementamos qualquer lógica para acesso a dados que persistimos.

### Dependências entre camadas

É quase natural que se fossemos imaginar a distribuição de dependência entre as camadas teríamos esse desenho:

![fluxo-data](http://rafael-miceli.com.br/ico/Inversao-De-Dependencia/fluxo-data.png)

O problema com está imagem é que neste cenário nossa camada de __regra de negócios__ (_Domain_) depende da camada de __dados__. 
Mas o coração de todo software se encontra na sua __regra de negócio__, pois como o nome da camada ja diz, a mesma define o _business_ do nosso sistema.

Este de longe é cenário mais comum aonde é aplicado o princípio da Inversão de Dependência

### DIP (_Dependency Inversion Principle_)

Ao aplicarmos o DIP no cenário acima a imagem muda para a seguinte:

![fluxo-data](http://rafael-miceli.com.br/ico/Inversao-De-Dependencia/fluxo-domain.png)

Mas na prática como fazemos isso?

Eis um pedaço (vertical) de nossa aplicação usando DIP, lembrando que este código pode ser encontrado no meu [Github](https://github.com/Rafael-Miceli/Blog-Codes/tree/master/IdeiasComAzeite/AutoFixtureSample/Escola):

MateriaController.cs

```csharp
public class MateriaController : ApiController
{
    private IMateriaService _materiaService;

    public MateriaController()
    {
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

MateriaService.cs

```csharp
public class MateriaService : IMateriaService
{
    private IMateriaRepo _materiaRepo;

    public MateriaService(IMateriaRepo materiaRepo)
    {
        _materiaRepo = materiaRepo;
    }

    public Materia BuscarMateriaPorNome(string nomeMateria)
    {
        return _materiaRepo.BuscarMateriaPorNome(nomeMateria);
    }

    public IEnumerable<Materia> BuscarMateriasSelecionadas(IEnumerable<int> materiasSelecionadasId)
    {
        return _materiaRepo.BuscarMateriasSelecionadas(materiasSelecionadasId);
    }

    public void CriarNovaMateria(Materia materia)
    {
        if (BuscarMateriaPorNome(materia.Nome) != null)
            throw new Exception("Matéria com esse nome já existe");

        _materiaRepo.CadastrarMateria(materia);
    }
}
```

MateriaRepository
```csharp
public class MateriaRepository : IMateriaRepo
{
    public Materia BuscarMateriaPorNome(string nomeMateria)
    {
        //Buscar materia no banco de dados
        return null;
    }

    public IEnumerable<Materia> BuscarMateriasSelecionadas(IEnumerable<int> materiasSelecionadasId)
    {
        //Buscar materias no banco de dados
        return null;
    }

    public void CadastrarMateria(Materia materia)
    {
        //Cadastrar materia no banco de dados
    }
}
```

Percebam que, a nossa classe __MateriaRepository__ da nossa camada de dados depende da implementação de nossa interface __IMateriaRepo__ que se encontra em nossa regra de negócios, ou seja, nós adicionamos uma referência do nosso projeto __Domain__ em nosso _Data_ e não o oposto.

Outra coisa que muda é que nossa camada de regra de negócios não mais instância nossa camada de dados (como resolver as instâncias veremos em um próximo post)

### Domain

Porque aplicamos o DIP no cenário anterior?

Porque queremos que o centro de nossa aplicação seja sempre nosso __Domain__, uma vez que ele possui o conjunto de regras necessárias para o funcionamento correto de nosso sistema. Podemos ver uma excelente referência sobre __Domain__ no livro [Domain Driven Design](http://www.amazon.com/Domain-Driven-Design-Tackling-Complexity-Software/dp/0321125215) do [Eric Evans](https://twitter.com/ericevans0).

Em um próximo post vamos ver como iniciar as instâncias de nossas classes após termos feito a inversão das dependências.
