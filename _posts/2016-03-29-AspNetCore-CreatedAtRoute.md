---
author: Rafael Miceli
comments: true
layout: default
category : aspnetcore
title: "Asp Net Core - problema com CreatedAtRoute em resposta de Post"
date: 2016-03-29 13:00:00
---

Se você esta usando a nova versão do asp.net e em algum momento decidiu fazer um scaffolding de um Controller para uma API com EntityFramework (lembrando que no asp.net Core, a classe Controller e ApiController foram unificadas) deve ter percebido que o template mudou algumas coisas do asp.net 4.5 para o core. 

###Novidades no Controller para API 

Agora na criação de um Scaffolding de Controller para API com EF o retorno é o padrão para todos os controllers cirados do Scaffold, __IActionResult__ e não mais __IHttpActionResult__ e, aonde quero me concentrar principalmente, um catch com um novo __HttpStatusCodeResult__. Novo porque neste __HttpStatusCodeResult__ não existe mais overloads do construtor como no anterior com StatusDescription. 

Para não deixar de lado, também houveram algumas mudanças como: 

- Injeção de dependência que vem nativo no asp.net Core 
- Melhoria na convenção de nome do private field do Context 
- Atributo __Produces__ no asp.net Core para indicar o content-type que vai ser retornardo pela classe 
- Atributo __Route__ no asp.net Core para já especificar a rota de sua API 

###CreatedAtRoute 

No asp.net Core como tudo novo, o método que era do __ApiController__ antigamente __CreatedAtRoute__, sofreu mudanças. Mas uma delas veio me incomodar 

###Mapeamento um para muitos em API 

Como comentei no topo, esse Scaffolding no asp.net Core foi usando o EF, ou seja, ao criar o novo Controller eu seleciono um Model e um Context e ele gera para mim os métodos read/write. 
Agora vamos criar o seguinte cenário para compreender o problema que quero demonstrar: 
Digamos que você vá criar duas entidades, profissão e pessoa, aonde uma pessoa pode ter uma profissão, mas uma profissão pode pertencer a várias pessoas. Um clássico um para muitos. 
Para nosso EF mapear isso a forma mais comum de se criar as entidades seria: 

    public class Profissao 
    { 
        public int Id { get; set; } 
        public string Nome { get; set; } 

        public List<Pessoa> Pessoas { get; set; } 
    } 

    public class Pessoa 
    { 
        public int Id { get; set; } 
        public string Nome { get; set; } 

        public int ProfissaoId { get; set; } 
        public Profissao Profissao { get; set; } 
    } 


Mel, certo? 

Mas ai que vem. Para retornar essa estrutura como Json pela API isso poderia nos dar uma referência cíclica com um seguinte exemplo: 

    'Pessoa': { 'Id': 1, 'Nome': 'Bruce', 'ProfissaoId': 1, 'Profissao': { 'Id': 1, 'Nome': 'Músico-Vocalista', 'Pessoas' [{ 'Id': 1, 'Nome': 'Bruce', 'ProfissaoId': 1, 'Profissao': 'Entenderam aonde quero chegar, certo?'] 

Mas por sorte nosso amigo asp.net já sabe lidar com isso, ele simplesmente faz isso: 

    { 'Id': 1, 'Nome': 'Bruce', 'ProfissaoId': 1, 'Profissao': null } 

Acabram os problemas! 

Bem, mais ou menos. Quando vamos dar um _Post_ para criar uma Pessoa temos o seguinte problema: 

![Erro](http://rafael-miceli.com.br/ico/AspNetCore-CreatedAtRoute/502_3.png)

Nós recebemos esse 502.3 quando estamos tentando realizar um Post de uma classe que é dependente de muitos! Mas isso não acontece ao salvar essa nova entidade, mas no __CreatedAtRoute__! 
Se você realizar um Get vai ver que esse problema não ocorre ( uma vez que não é chamado o __CreatedAtRoute__) 


###Como resolver isso? 

Por hora, a forma mais simples que encontrei para solucionar o problema em uma situação como essa é retornar o seguinte ao final do seu Post com sucesso: 
 
    return new HttpStatusCodeResult(StatusCodes.Status201Created); 

Quem passou pelo mesmo problema que eu, por favor comentem aqui as soluções que vocês encontraram para a situação! 

Vlw, e até a próxima. 






 


 
