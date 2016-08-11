---
author: Rafael Miceli
comments: true
layout: default 
category : DateTime
title: "DateTime - Time Zones" 
date: 2016-08-11 10:00:00
---

A ideia por tras deste post é tentar explicar um pouco as práticas para organizar sua data e hora em aplicações Web. E aonde vamos chegar a ver um forma de resolver isso para aplicações internacionais.

Quando vamos criar uma aplicação Web em muitos casos nunca damos muita importancia quanto a questões como Time Zone.

Isso é comum, principalmente se estamos preparando uma aplicação que tem por definição atingir apenas seu território nacional. E que ainda assim, neste post, vamos descobrir que _dependendo do Domain da sua aplicação_ __você precisa se preocupar com seu Time Zone__, mesmo em terrirótio nacional.

### Time Zones

Antes de explicar as práticas para se trabalhar com DateTime primeiro é necessário que se entenda o conceito de [Time Zones](https://en.wikipedia.org/wiki/Time_zone). 

Não vou explicar todos os detalhes sobre Time Zone, no link acima esta a explicação definida pelo wikipedia em ingles, Porém, reforço uma coisa crucial que no wikipedia em portugues não diz: __Time Zones são definidas por questões políticas, economicas e sociais!__

Isso quer dizer que amanhã um país sofreu algum golpe ou algo do genero, _pode ser_ que ele mude seu time zone. Um exempo recente ocorreu com a [Crimeia em 2014](https://en.wikipedia.org/wiki/Time_in_Russia#2014_zone_boundary_changes)

### Sem se preocupar com Time Zones

Mas porque eu disse la em cima que para se preocupar com Time Zones _depende do Domain da sua aplicação_?

Bem, imagina que você pretende criar uma aplicação para você organizar seus livros, ou para te ajudar com formulas matemáticas, ou até mesmo para realizar entrega de compras de mercados de pequenas redes.

Nestas situações, em 99% das vezes, não precisamos nos preocupar com o Time Zone, já que geralmente esses programas não envolvem uma informação sobre data, ou quando envolve, esta data é apenas de uma pequena localidade.

No exemplo de app para organizar livros você não precisa se preocupar com a hora do seu acervo de livros.
No caso de entregas para pequenas redes de mercados você vai querer ter a hora do pedido para entregar, mas que, para seu foco em pequenas redes locais, em muitos casos você não vai precisar lidar com a mudança de Time Zones. Já que vão ser redes locais.

### Se preocupando com Time Zones 

Já, se formos realizar uma aplicação que vai realizar fornecimento de mercadoria em rede nacional, ou que pessoas de outros estados possam agendar uma hospedagem com você, então nessas situações temos pensar no TimeZone.

Só para se ter uma ideia como comos um país de proporção grande, temos 4 Time Zones diferentes!

![timezones](http://www.manual2brazil.com/LearnAbout/Travel-Needs/~/images/Travel-Needs-Time-Zones.jpg)

No próximo post, vamos ver o que fazer quando precisamos nos preocupar com Time Zones.