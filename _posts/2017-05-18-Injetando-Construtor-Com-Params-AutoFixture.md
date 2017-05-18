---
author: Rafael Miceli
comments: true
layout: default 
category : Autofixture
title: "Injetando construtor em código legado com Autofixture" 
date: 2017-05-18 20:40:00
---

Quando trabalhando com projetos legados¹ e estamos refatorando algumas classes para cobrir com testes uma das coisas mais comuns de se encontrar são acoplamentos\dependências² com outras classes que fazem comunicação com Banco de Dados ou com algum serviço externo.

> 1 Projetos legados de acordo com a definição do [Michael C. Feathers](https://twitter.com/mfeathers) é de projeto que não está coberto com testes que ele cita em seu livro [Working Effectively With Legacy Code](https://www.amazon.com/Working-Effectively-Legacy-Michael-Feathers/dp/0131177052/ref=pd_lpo_sbs_14_t_0?_encoding=UTF8&psc=1&refRID=ZH158VAH29PE94H6E4TV). Já [David Bernstein](https://twitter.com/tobeagile) coloca em [seu livro](https://www.amazon.com/Beyond-Legacy-Code-Practices-Software/dp/1680500791) que Projeto Legado é aquele que você não tem confiança em realizar mudanças no mesmo

> 2 Acoplamentos ou dependência em software é geralmente quando criamos uma ligação entre classes aonde um depende da implementação da outra (não tendo uma interface no meio para dependermos da abstração). Lembrando que nem toda dependência é ruim, dependência entre classes do Domain são muito comuns

Muitas das vezes vamos ver classes com as dependências de uma dessas 3 formas:

Com a dependência sendo feita logo antes da chamada:

```csharp
public class MateriaEscolar
{

    public void AdicionarMateria(Materia materia)
    {
        //Regra de negócio

        //Dependencia
        MatriaEscolarRepository_Dao dao = new MatriaEscolarRepository_Dao();
        dao.Adicionar(materia);
    }
}
```

Com a dependência sendo feita no membro da classe:

```csharp
public class MateriaEscolar
{
    //Dependencia
    private MatriaEscolarRepository_Dao _dao = new MatriaEscolarRepository_Dao();

    public void AdicionarMateria(Materia materia)
    {
        //Regra de negócio

        _dao.Adicionar(materia);
    }
}
```

E em alguns casos melhores com a dependência no contrutor:

```csharp
public class MateriaEscolar
{
    private readonly MatriaEscolarRepository_Dao _dao;

    public MateriaEscolar()
    {
        //Dependencia
        _dao = new MatriaEscolarRepository_Dao();
    }

    public void AdicionarMateria(Materia materia)
    {
        //Regra de negócio

        _dao.Adicionar(materia);
    }
}
```

E nos dois últimos casos (o primeiro você vai precisar entender melhor o ciclo de vida da classe) para tornarmos nossa classe "testável" **Repository\Dao** nós extraímos uma Interface da **Repository\Dao**, trocamos nosso Field que era a classe pela Interface que acabamos de criar e criamos um novo **Construtor** injetando nosso **Repository\Dao**, ficando da seguinte maneira:

```csharp
public class MateriaEscolar
{
    private readonly IMatriaEscolarRepository_Dao _dao;

    public MateriaEscolar()
    {
        //Dependencia
        _dao = new MatriaEscolarRepository_Dao();
    }

    public MateriaEscolar(IMatriaEscolarRepository_Dao dao)
    {
        _dao = dao;
    }

    public void AdicionarMateria(Materia materia)
    {
        //Regra de negócio
        
        _dao.Adicionar(materia);
    }
}
```

Com isso não quebramos as classes que dependem da classe **MateriaEscolar** e temos um construtor aonde podemos Injetar nossos [Mocks](https://www.google.com.br/url?sa=t&rct=j&q=&esrc=s&source=web&cd=1&cad=rja&uact=8&ved=0ahUKEwjX46m2wvXTAhUIHJAKHcUMBVYQFggjMAA&url=https%3A%2F%2Fmartinfowler.com%2Farticles%2FmocksArentStubs.html&usg=AFQjCNH4utCc4SrD1s5GVGroA183renVJA&sig2=j1tNvLgrAf-R6SzxoUoiEg) para realizar nossos testes.

### Injetando dependência com AutoFixture

Já falei do [Autofixture aqui](https://www.google.com.br/url?sa=t&rct=j&q=&esrc=s&source=web&cd=1&cad=rja&uact=8&ved=0ahUKEwir_sGPwvXTAhXIl5AKHeQLBT0QFggjMAA&url=http%3A%2F%2Frafael-miceli.com.br%2Funit%2520test%2F2016%2F04%2F06%2FMelhorando-Testes-Autofixture.html&usg=AFQjCNGmPG8Ull4cc7HQXx1b23r1NP6qsQ&sig2=LULya0TxjrmwTFj06EQybQ) e ele é uma excelente ferramenta junto com seu [AutoMoq Customization](https://www.nuget.org/packages/AutoFixture.AutoMoq) para injetar as dependências automaticamente. Porém quando temos um construtor vazio e o um construtor com as dependências para injetarmos como no caso mostrado acima, ao criarmos nosso [System\Subject Under Test](https://en.wikipedia.org/wiki/System_under_test) da classe **MateriaEscolar** da seguinte maneira:

`var sut = fixture.Create<MateriaEscolar>();`

Por default o AutoFixture vai inicializar com o construtor sem parâmetros. Como podemos então chamar sempre o construtor com parâmetros que queremos injetar?

Para isso antes de criar nosso `sut`, adicionamos a seguinte customização:

`fixture.Customize<MateriaEscolar>(c => c.FromFactory(new MethodInvoker(new GreedyConstructorQuery())));`

Com isso nosso `sut` vai ser construído usando o construtor com as dependências que queremos injetar, e se estivermos usando AutoMoq elas vão ser injetadas dinamicamente.