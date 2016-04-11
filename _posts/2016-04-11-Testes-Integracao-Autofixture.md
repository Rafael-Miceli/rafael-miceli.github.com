---
author: Rafael Miceli
comments: true
layout: default 
category : unit test 
title: "Testes de integração com AutoFixture" 
date: 2016-04-11 09:00:00
---
 
AutoFixture é uma excelente ferramenta para usarmos e nossos testes unitários, mas também é bom para testes de integração?

Primeiro vamos a uma rápida diferença entre testes unitários e testes de integração.

> Teste Unitário - Testa uma pequena porção do seu código, verifica se um método ou um pequeno grupo de métodos de um objeto está se comportando da maneira como é esperado.

> Teste de integração - Testa também o comportamento, mas testa se várias partes do seus sistema estão funcionando juntas corretamente. Caso seu teste precise realizar chamadas a serviços externos (Web Servcies por exemplo) ou banco de dados, você pode ou não mockar essas chamadas.


### Teste de integração sem AutoFixture

Imagina que fossemos realizar um teste de integração sem o autofixture em um webapi. Neste caso queremos garantir o seguinte:

Quando o aluno for registrado tem de vincular ele as matérias dele ao polo que ele esta fazendo as matérias, criar um login para ele e... Mandar um e-mail..._Caramba quanta coisa_.

Para isso temos a seguinte estrutura (todo o exemplo esta no meu [Github](https://github.com/Rafael-Miceli/Blog-Codes/tree/master/IdeiasComAzeite/AutoFixtureSample/Escola))

AlunoController.cs

```csharp
public class AlunoController : ApiController
{
    private IAlunoFacade _alunoFacade;

    public AlunoController(IAlunoFacade alunoFacade)
    {
        _alunoFacade = alunoFacade;
    }

    // POST api/values
    public HttpResponseMessage Post([FromBody]AlunoVm alunoVm)
    {
        try
        {
            _alunoFacade.RegistrarNovoAluno(alunoVm);

            return Request.CreateResponse(HttpStatusCode.Created);
        }
        catch (Exception ex)
        {
            return Request.CreateErrorResponse(HttpStatusCode.BadRequest, ex.Message);
        }
        
    }
    
}
```

AlunoVm.cs

```csharp
public class AlunoVm
{
    [Required]
    public string Matricula { get; set; }
    [Required]
    public string Nome { get; set; }
    public IEnumerable<int> MateriasSelecionadasId { get; set; }
    [Required]
    public int PoloSelecionadoId { get; set; }
    [Required]
    [EmailAddress]
    public string Email { get; set; }
}
```

AlunoFacade.cs

```csharp
public class AlunoFacade: IAlunoFacade
{
    private IAlunoService _alunoService;
    private IMateriaService _materiaService;
    private IPoloService _poloService;
    private IUsuarioService _usuarioService;
    private IEmailService _emailService;


    public AlunoFacade(IAlunoService alunoService, 
        IMateriaService materiaService, 
        IPoloService poloService, 
        IUsuarioService usuarioService, 
        IEmailService emailService)
    {
        _alunoService = alunoService;
        _materiaService = materiaService;
        _poloService = poloService;
        _usuarioService = usuarioService;
        _emailService = emailService;
    }

    public void RegistrarNovoAluno(AlunoVm alunoVm)
    {
        if (MatriculaJaExiste(alunoVm.Matricula))
            throw new Exception("Mattrícula já cadastrada");

        IEnumerable<Materia> materias = _materiaService.BuscarMateriasSelecionadas(alunoVm.MateriasSelecionadasId);
        Polo polo = _poloService.BuscarPoloSelecionado(alunoVm.PoloSelecionadoId);

        _alunoService.CriarAluno(MapearAlunoVmParaAluno(alunoVm), polo, materias);
        _usuarioService.CriarUsuarioParaAluno(alunoVm.Matricula);            
        _emailService.EnviarEmailParaAlunoCriado(alunoVm.Email);
    }

    private Aluno MapearAlunoVmParaAluno(AlunoVm alunoVm)
    {
        return new Aluno
        {
            Matricula = alunoVm.Matricula,
            Nome = alunoVm.Nome,
            Email = alunoVm.Email
        };
    }

    private bool MatriculaJaExiste(string matricula)
    {
        return _alunoService.BuscarPorMatricula(matricula) != null;
    }
}
```

AlunoService.cs [local no github](https://github.com/Rafael-Miceli/Blog-Codes/blob/master/IdeiasComAzeite/AutoFixtureSample/Escola/Escola.Domain/AlunoService.cs)  

MateriaService.cs [local no github](https://github.com/Rafael-Miceli/Blog-Codes/blob/master/IdeiasComAzeite/AutoFixtureSample/Escola/Escola.Domain/MateriaService.cs)

PoloService.cs [local no github](https://github.com/Rafael-Miceli/Blog-Codes/blob/master/IdeiasComAzeite/AutoFixtureSample/Escola/Escola.Domain/PoloService.cs)

UsuarioService.cs [local no github](https://github.com/Rafael-Miceli/Blog-Codes/blob/master/IdeiasComAzeite/AutoFixtureSample/Escola/Escola.Domain/UsuarioService.cs)

EmailService.cs [local no github](https://github.com/Rafael-Miceli/Blog-Codes/blob/master/IdeiasComAzeite/AutoFixtureSample/Escola/Escola.Domain/EmailService.cs)

Os repositórios são apenas __Interfaces__ no __Domain__ [local no github](https://github.com/Rafael-Miceli/Blog-Codes/tree/master/IdeiasComAzeite/AutoFixtureSample/Escola/Escola.Domain/Interfaces). 

Aqui vamos possuir um teste só para o exemplo, quando cadastrar um aluno aonde a matricula dele seja única, no formato correto, aonde as matérias escolhidas existam, seu polo exista deve retornar 201 created

AlunoControllerTeste.cs

```csharp
[Fact]
public void Ao_Cadastrar_Um_Aluno_Aonde_Sua_Matricula_Eh_Unica_Selecionou_Um_Polo_Valido_Selecionou_Materias_Validas_Criou_O_Usuario_Corretamente_Entao_Retorne_201()
{
    // Arrange
    AlunoVm alunoVm = new AlunoVm
    {
        Matricula = "12345678",
        Nome = "Rafael",
        Email = "rafael@teste.com",
        PoloSelecionadoId = 1,
        MateriasSelecionadasId = new List<int> { 1, 2, 3 }
    };

    var alunoRepoMock = new Mock<IAlunoRepo>();
    var materiaRepoMock = new Mock<IMateriaRepo>();
    var poloRepoMock = new Mock<IPoloRepo>();

    alunoRepoMock.Setup(x => x.BuscarPorMatricula(alunoVm.Matricula)).Returns((Aluno)null);
    materiaRepoMock.Setup(x => x.BuscarMateriasSelecionadas(It.IsAny<IEnumerable<int>>())).Returns(new List<Materia>
    {
        new Materia()
    });

    poloRepoMock.Setup(x => x.BuscarPoloSelecionado(alunoVm.PoloSelecionadoId)).Returns(new Polo());

    IAlunoService alunoService = new AlunoService(alunoRepoMock.Object);
    IMateriaService materiaService = new MateriaService(materiaRepoMock.Object);
    IPoloService poloService = new PoloService(poloRepoMock.Object);
    IUsuarioService usuarioService = new UsuarioService();
    IEmailService emailService = new EmailService();                        

    IAlunoFacade alunoFacade = new AlunoFacade(alunoService, materiaService, poloService, usuarioService, emailService);

    AlunoController sut = new AlunoController(alunoFacade);

    // Act
    var result = sut.Post(alunoVm);

    // Assert

    Assert.Equal(HttpStatusCode.Created, result.StatusCode);
}
```

O número de dependências que você tem de preparar são bastante, e se mudar uma dependência ai no meio, __quebra tudo__.
Agora, imagina mais testes desse?

### Teste de integração com AutoFixture

Com o Autofixture temos muito ganho.

1 - Reduz a manutenção do teste, uma vez que novas dependências não quebram mais o teste.

2 - Podemos inicializar apenas as dependências que precisamos para o teste (neste caso não precisamos nunca lidar com os repositórios ou com e-mail ou boleto service, caso não seja necessário)

3 - Diminui número de linhas de código para o teste, dependendo do teste podendo reduzir drasticamente o mesmo. 

Vamos ver como fica o teste com o AutoFixture:

AlunoControllerTeste.cs

```csharp
[Fact]
public void Fixture_Ao_Cadastrar_Um_Aluno_Aonde_Sua_Matricula_Eh_Unica_Selecionou_Um_Polo_Valido_Selecionou_Materias_Validas_Criou_O_Usuario_Corretamente_Entao_Retorne_201()
{
    // Arrange
    var fixture = new Fixture();
    fixture.Customize(new ApiControllerConventions());

    AlunoVm alunoVm = fixture.Create<AlunoVm>();

    var alunoRepoMock = fixture.Freeze<Mock<IAlunoRepo>>();

    alunoRepoMock.Setup(x => x.BuscarPorMatricula(alunoVm.Matricula)).Returns((Aluno)null);
    
    IAlunoService alunoService = fixture.Create<AlunoService>();
    fixture.Inject(alunoService);
    IMateriaService materiaService = fixture.Create<MateriaService>();
    fixture.Inject(materiaService);
    IPoloService poloService = fixture.Create <PoloService>();
    fixture.Inject(poloService);
    IUsuarioService usuarioService = fixture.Create <UsuarioService>();
    fixture.Inject(usuarioService);
    IEmailService emailService = fixture.Create<EmailService>();
    fixture.Inject(emailService);

    IAlunoFacade alunoFacade = fixture.Create<AlunoFacade>();
    fixture.Inject(alunoFacade);

    AlunoController sut = fixture.Create<AlunoController>();

    // Act
    var result = sut.Post(alunoVm);

    // Assert
    Assert.Equal(HttpStatusCode.Created, result.StatusCode);
}
```

Reparem que precisamos definir as nossas injeções em nosso fixture na seguinte ordem:

- Damos um __Freeze__ com __AutoFixture__ no repositório que queremos manipular seu setup.
- Criamos os Services e damos inject em cada um. 
- Em seguida criamos o façade e injetamos o mesmo
- E por fim criamos o Controller com o objeto fixture.

Mais um detalhe que vale apontar. Para testarmos WebApi 2 Controllers precisamos de um __Customize__ para o Autfixture que se encontra também no meu [Github](https://github.com/Rafael-Miceli/Blog-Codes/blob/master/IdeiasComAzeite/AutoFixtureSample/Escola/Escola.Api.Tests/Controllers/ApiControllerConventions.cs)

  

