---
author: Rafael Miceli
comments: true
layout: default 
category : unit test 
title: "Melhorando manutenção em testes com AutoFixture" 
date: 2016-04-06 12:00:00
--- 

Quem nunca precisou refatorar um código que quebrasse os testes que cobria o mesmo? 
 
Ok, dependendo da refatoracão é porque você realmente quebrou algo, mas tem situações em que dia classe precisa de uma nova dependência e isso quebra o __Arrange__ do seu teste, vamos imaginar a seguinte situação. 
 
### Problema no Arrange 
 
Digamos que você possui o seguinte teste para garantir que ai criar um aluno o mesmo não possa ser cadastrado com uma matricula já existente (Todo o código inserido aqui pode ser encontrado no meu [github](https://github.com/Rafael-Miceli/Blog-Codes/tree/master/IdeiasComAzeite/AutoFixtureSample)): 

__AlunoServiceTest.cs__
 
![passo1](http://rafael-miceli.com.br/ico/Melhorando-Testes-Autofixture/passo1.png)

__Aluno.cs__ 
 
![passo2](http://rafael-miceli.com.br/ico/Melhorando-Testes-Autofixture/passo2.png) 
 
__AlunoService.cs__

![passo3](http://rafael-miceli.com.br/ico/Melhorando-Testes-Autofixture/passo3.png) 
 
Excelente, sempre ao criar um aluno garantimos que ele tenha uma matricula única. 
 
Mas, o usuário do sistema disse que quando cadastrar um aluno, também tem que vincular as matérias que ele vai fazer. 
 
Então, vamos criar um teste para garantir que quando o aluno se cadastrar, vincule as matérias que ele selecionou. 
 
 __AlunoServiceTest.cs__
 
![passo4](http://rafael-miceli.com.br/ico/Melhorando-Testes-Autofixture/passo4.png)

__AlunoService.cs__
    
![passo5](http://rafael-miceli.com.br/ico/Melhorando-Testes-Autofixture/passo5.png) 
 
Perceba que nessa modificação, no Service de criar aluno inserimos uma nova dependência, como só tínhamos três unitários isso é bem fácil de lidar, mas imagina que tivéssemos dezenas (o que em alguns casos não é difícil), isso seria um bom trabalho, e repetitivo até. Imagina futuramente se tivermos mais uma nova dependência? Vai ficar complicada a coisa. Que tal melhorarmos o __Arrange__ do nosso teste então? 
 
Uma forma que podemos fazer isso é com a design pattern __SUTFactory__, muito bem explicada por [Mark Seemann](http://blog.ploeh.dk/2009/02/13/SUTFactory/). 
 
### Autofixture 
 
Agora, para um sistema maior imagina ter de criar uma classe __SUTFactory__ para cada classe que você precisa gerenciar as dependências? Bastante código certo? 
 
Com o __Autofixture__ facilitamos muito nosso trabalho. 
 
Para usarmos o __Autofixture__ instalamos o mesmo com nosso package manager no nosso projeto de teste 
 
![passo6](http://rafael-miceli.com.br/ico/Melhorando-Testes-Autofixture/passo6.png) 
 
Agora, vamos refatorar nosso teste usando o autofixture 
 
![passo7](http://rafael-miceli.com.br/ico/Melhorando-Testes-Autofixture/passo7.png) 
 
Legal, mas nem tanto ainda. Pois estamos tendo um erro com a interface dos services. 
 
### AutoFixture.AutoMoq 
 
Para facilitar mais podemos instalar também um plugin para o Autofixture com moq. Vamos instalar o mesmo no nosso projeto de teste. 
 
![passo8](http://rafael-miceli.com.br/ico/Melhorando-Testes-Autofixture/passo8.png) 
 
Tendo Instalado, agora vamos refatorar um pouco mais nosso teste. 
 
Preciso adicionar o seguinte trecho para que o Autofixture faça seu trabalho com o moq: 
 
![passo9](http://rafael-miceli.com.br/ico/Melhorando-Testes-Autofixture/passo9.png) 
 
Se perceber, o Autofixture cria dados randômicos para as propriedades de nossa classe, e com o AutoFixture.AutoMoq ele gera setups com returns randômicos também, o que é um problema para nossos testes de verificar a matricula, precisamos garantir que o retorno do método ___alunoRepo.ExisteMatricula__ seja verdadeiro para o cenário da matricula já existir.  
 
Além disso vamos melhorar nosso teste garantindo que ele não tente buscar as matérias no cenário de matrícula repetida. Pois uma vez indo buscar as matérias, significa que a matricula não era repetida. 
 
Para ajustarmos isso vamos refatorar mais uma vez nosso teste para o seguinte: 
 
```csharp
[TestClass]
public class AlunoServiceTests
{
    [TestMethod]
    [ExpectedException(typeof(Exception))]        
    public void Dado_Um_Aluno_Quando_Criar_O_Mesmo_Se_A_Matricula_Ja_Existir_Deve_Levantar_Uma_Excecao()
    {
        //Arrange
        var fixture = new Fixture();
        fixture.Customize(new AutoMoqCustomization());

        var materiaServiceMock = fixture.Freeze<Mock<IMateriaService>>();
        var alunoRepoMock = fixture.Freeze<Mock<IAlunoRepo>>();

        alunoRepoMock.Setup(x => x.ExisteMatricula(It.IsAny<string>())).Returns(true);

        var aluno = fixture.Build<Aluno>().Without(a => a.Materias).Create();
        var sut = fixture.Create<AlunoService>();
        
        //Act
        sut.AdcionarAluno(aluno, null);

        //Assert
        materiaServiceMock.Verify(x => x.BuscarMateriaisSelecionados(It.IsAny<List<int>>()), Times.Never);
    }

    [TestMethod]
    public void Dado_Um_Aluno_Quando_Criar_O_Mesmo_Se_A_Matricula_Nao_Existir_Deve_Criar_Com_Sucesso()
    {
        //Arrange
        var fixture = new Fixture();
        fixture.Customize(new AutoMoqCustomization());

        var materiaServiceMock = fixture.Freeze<Mock<IMateriaService>>();
        var alunoRepoMock = fixture.Freeze<Mock<IAlunoRepo>>();

        alunoRepoMock.Setup(x => x.ExisteMatricula(It.IsAny<string>())).Returns(false);

        var aluno = fixture.Build<Aluno>().Without(a => a.Materias).Create();
        var materiasIdsSelecionadas = fixture.CreateMany<int>();
        var sut = fixture.Create<AlunoService>();
        
        //Act
        sut.AdcionarAluno(aluno, materiasIdsSelecionadas.ToList());

        //Assert
        materiaServiceMock.Verify(x => x.BuscarMateriaisSelecionados(It.IsAny<List<int>>()), Times.Once);
        alunoRepoMock.Verify(x => x.CriarAluno(aluno), Times.Once);
    }

    [TestMethod]    
    public void Dado_Um_Aluno_Quando_Criar_Se_O_Mesmo_Selecionou_Materia_Deve_Criar_Com_Sucesso()
    {
        //Arrange
        var fixture = new Fixture();
        fixture.Customize(new AutoMoqCustomization());

        var materiaServiceMock = fixture.Freeze<Mock<IMateriaService>>();
        var alunoRepoMock = fixture.Freeze<Mock<IAlunoRepo>>();

        alunoRepoMock.Setup(x => x.ExisteMatricula(It.IsAny<string>())).Returns(false);
        materiaServiceMock.Setup(x => x.BuscarMateriaisSelecionados(It.IsAny<List<int>>())).Returns(new List<Materia>
        {
            new Materia()
        });

        var aluno = fixture.Build<Aluno>().Without(a => a.Materias).Create();
        var materiasIdsSelecionadas = fixture.CreateMany<int>();
        var sut = fixture.Create<AlunoService>();
        
        //Act
        sut.AdcionarAluno(aluno, materiasIdsSelecionadas.ToList());

        //Assert
        Assert.IsNotNull(aluno.Materias);
        Assert.IsTrue(aluno.Materias.Count > 0);
        materiaServiceMock.Verify(x => x.BuscarMateriaisSelecionados(It.IsAny<List<int>>()), Times.Once);
        alunoRepoMock.Verify(x => x.CriarAluno(aluno), Times.Once);
    }

}
``` 
 
Agora vamos triangularizar nosso cenário de aluno selecionar matérias criando um teste para a situação que o aluno não escolha nenhuma matéria: 
 
```csharp
[TestClass]
public class AlunoServiceTests
{
    [TestMethod]
    [ExpectedException(typeof(Exception))]        
    public void Dado_Um_Aluno_Quando_Criar_O_Mesmo_Se_A_Matricula_Ja_Existir_Deve_Levantar_Uma_Excecao()
    {
        //Arrange
        var fixture = new Fixture();
        fixture.Customize(new AutoMoqCustomization());

        var materiaServiceMock = fixture.Freeze<Mock<IMateriaService>>();
        var alunoRepoMock = fixture.Freeze<Mock<IAlunoRepo>>();

        alunoRepoMock.Setup(x => x.ExisteMatricula(It.IsAny<string>())).Returns(true);

        var aluno = fixture.Build<Aluno>().Without(a => a.Materias).Create();
        var sut = fixture.Create<AlunoService>();
        
        //Act
        sut.AdcionarAluno(aluno, null);

        //Assert
        materiaServiceMock.Verify(x => x.BuscarMateriaisSelecionados(It.IsAny<List<int>>()), Times.Never);
    }

    [TestMethod]
    public void Dado_Um_Aluno_Quando_Criar_O_Mesmo_Se_A_Matricula_Nao_Existir_Deve_Criar_Com_Sucesso()
    {
        //Arrange
        var fixture = new Fixture();
        fixture.Customize(new AutoMoqCustomization());

        var materiaServiceMock = fixture.Freeze<Mock<IMateriaService>>();
        var alunoRepoMock = fixture.Freeze<Mock<IAlunoRepo>>();

        alunoRepoMock.Setup(x => x.ExisteMatricula(It.IsAny<string>())).Returns(false);

        var aluno = fixture.Build<Aluno>().Without(a => a.Materias).Create();
        var materiasIdsSelecionadas = fixture.CreateMany<int>();
        var sut = fixture.Create<AlunoService>();
        
        //Act
        sut.AdcionarAluno(aluno, materiasIdsSelecionadas.ToList());

        //Assert
        materiaServiceMock.Verify(x => x.BuscarMateriaisSelecionados(It.IsAny<List<int>>()), Times.Once);
        alunoRepoMock.Verify(x => x.CriarAluno(aluno), Times.Once);
    }

    [TestMethod]    
    public void Dado_Um_Aluno_Quando_Criar_Se_O_Mesmo_Selecionou_Materia_Deve_Criar_Com_Sucesso()
    {
        //Arrange
        var fixture = new Fixture();
        fixture.Customize(new AutoMoqCustomization());

        var materiaServiceMock = fixture.Freeze<Mock<IMateriaService>>();
        var alunoRepoMock = fixture.Freeze<Mock<IAlunoRepo>>();

        alunoRepoMock.Setup(x => x.ExisteMatricula(It.IsAny<string>())).Returns(false);
        materiaServiceMock.Setup(x => x.BuscarMateriaisSelecionados(It.IsAny<List<int>>())).Returns(new List<Materia>
        {
            new Materia()
        });

        var aluno = fixture.Build<Aluno>().Without(a => a.Materias).Create();
        var materiasIdsSelecionadas = fixture.CreateMany<int>();
        var sut = fixture.Create<AlunoService>();
        
        //Act
        sut.AdcionarAluno(aluno, materiasIdsSelecionadas.ToList());

        //Assert
        Assert.IsNotNull(aluno.Materias);
        Assert.IsTrue(aluno.Materias.Count > 0);
        materiaServiceMock.Verify(x => x.BuscarMateriaisSelecionados(It.IsAny<List<int>>()), Times.Once);
        alunoRepoMock.Verify(x => x.CriarAluno(aluno), Times.Once);
    }

    [TestMethod]
    [ExpectedException(typeof(Exception))]
    public void Dado_Um_Aluno_Quando_Criar_Se_O_Mesmo_Nao_Selecionou_Materia_Deve_Levantar_Uma_Excecao()
    {
        //Arrange
        var fixture = new Fixture();
        fixture.Customize(new AutoMoqCustomization());

        var materiaServiceMock = fixture.Freeze<Mock<IMateriaService>>();
        var alunoRepoMock = fixture.Freeze<Mock<IAlunoRepo>>();

        alunoRepoMock.Setup(x => x.ExisteMatricula(It.IsAny<string>())).Returns(false);

        var aluno = fixture.Build<Aluno>().Without(a => a.Materias).Create();
        var sut = fixture.Create<AlunoService>();

        //Act
        sut.AdcionarAluno(aluno, new List<int>());

        //Assert
        materiaServiceMock.Verify(x => x.BuscarMateriaisSelecionados(It.IsAny<List<int>>()), Times.Never);
        alunoRepoMock.Verify(x => x.CriarAluno(aluno), Times.Never);
    }
}
``` 
 
Com isso podemos se quiser adicionar mais uma dependência que não vamos quebrar nossos testes. Digamos que o usuário nos pediu para que após cadastrar o aluno em suas matérias, envie um e-mail para o mesmo. Fazemos a seguinte modificação: 
 
![passo10](http://rafael-miceli.com.br/ico/Melhorando-Testes-Autofixture/passo10.png) 
 
Adicionando a dependência IEmailService não quebramos os testes anteriores, nem em compile time. 
 
Fica ai um _sample_ de __Autofixture__ para situações que geralmente passamos no dia a dia. 
 
Lembrando que todo o código para conferir (a versão sem o Autofixture e com o Autofixture) estão em meu [github](https://github.com/Rafael-Miceli/Blog-Codes/tree/master/IdeiasComAzeite/AutoFixtureSample) 