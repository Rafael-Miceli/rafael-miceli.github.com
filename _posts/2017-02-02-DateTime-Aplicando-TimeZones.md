---
author: Rafael Miceli
comments: true
layout: default 
category : DateTime
title: "DateTime - Aplicando Time Zones" 
date: 2017-02-02 15:00:00
---

Vamos ver uma forma de lidarmos com data e hora de clientes em diferentes TimeZones.

### Cenário

Vamos criar um cenário aonde você quer armazenar o horário que um cliente clica em um botão na Argentina e quer exibir para você aqui no Brasil como se a pessoa tivesse clicado ao lado de você.

### UTC

Em C# a primeira coisa a fazer é quando a pessoa clicar no botão você deve salvar a data dele como UTC da seguinte maneira:

`var horarioQueClicouBotao =  DateTime.UtcNow;`

UTC significa Coordinated Universal Time, ele é um fuso horário de referência para se calcular todas as outras zonas de hora do mundo. O fuso zero parte do Reino Unido e usamos o UTC para após fazer base de cálculo para a zona aonde estamos.

Por exemplo no Rio temos UTC -2, ou seja neste momento dia 02/02/2017 é 13:00 então UTC +0 que é em Londres são 15:00 do mesmo dia. Porém lembrem que isso muda. Pois estamos no horário de verão! Então quando o mesmo acabar vamos estar a UTC -3.

Tem alguns UTCs não triviais com -2.30 e até -2.15. Mas esses casos são bem específicos.

Quando armazenamos o horário que alguem clicou no botão com UtcNow estamos armazenando no horário do  UTC +0 Sempre! E se queremos exibir para alguem fazemos o calculo para isso.

### E o GMT?

O GMT que era anteriormente como se fosse o UTC +0 não é tão preciso de acordo com a comunidade científica em seus Leap Seconds (não encontrei uma fonte científica se alguem encontrar por favor me avise)

### UTC para Local

Aqui é que tem uma pegadinha. Em uma aplicação web converter da hora local para UTC é bastante simples… Em compensação converter de UTC para local não é tão trivial quanto usar `ToLocal` do Visual Studio.

O motivo é que chamando `ToLocal` vamos pegar o TimeZone do servidor e não da onde o usuário esta acessando mesmo.

O jeito então é fazermos essa conversão no Javascript! Mas tem mais um detalhe também.

Quando armazenamos a data como UTC o formato dela é de UTC: 2017-02-15T13:00:00. Neste caso peecisamos converter essa data para o formato da [ISO 8601](https://www.w3.org/TR/NOTE-datetime) ou isso nó código:

`dateTime.ToString("yyyy/MM/dd HH:mm:ss");`

Agora sim podemos mandar essa data para nosso Front transformar para local

### Front para Local

Estando com nossa data no formato correto em nosso Front agora precisamod tranformar ela em Local, mas infelizmente o Javascript não facilita como o C# nesse caso. Mas também não impossibilita.

Para exibirimos o horário local vamos usar a função `getTimeZoneOffset` do objeto `Date`.

A função `getTimeZoneOffset` nos retorna a diferença em minutos do UTC +0 para o UTC em que estamos! Para saber mais sobre o `getTimeZoneOffset` você pode ler ele na [MDN](https://developer.mozilla.org/pt-BR/docs/Web/JavaScript/Reference/Global_Objects/Date/getTimezoneOffset)

Vamos aplicar como faríamos isso:

```csharp
var d = new Date();
var offsetms = d.getTimezoneOffset() * 60 * 1000;
$('.UTCTime').each(function () {
    try {

        var text = $(this).html();
        var language = options.lng;

        if (text == "")
            return;

        var n = new Date(text);
        n = new Date(n.valueOf() - offsetms);

        console.log(n);
    }
    catch (ex) {
        console.warn("Error converting time", ex);
    }
});
```

No código acima estamos pegando a diferença em minutos e tranformando em milisegundos assim podemos calcular uma data menos a outra.
Perceba que estamos guardando a data que recebemos do nosso Back-End em um span com a classe `UTCTime`. Nela pegamos a data e subtraimos com os milisegundos.

Com isso temos nossa data convertida para Local!