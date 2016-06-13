---
author: Rafael Miceli
comments: true
layout: default 
category : controle_versionamento 
title: "Controle de Versionamento - Introdução" 
date: 2016-06-07 22:40:00
---

Hoje em dia em qualquer lugar que você for trabalhar com desenvolvimento de software vai ter um controle de versionamento nos projetos (e se não tem é porque a empresa possui muito má prática).

Controle de versionamento proporciona muitas vantagens como:

Manter o historico de mudanças do seu projeto
Facilidade em colaboração no desenvolvimento com equipes.
Controle de trabalhar em múltiplas tarefas sem implicar em outras.
Integração contínua no projeto.
E outras...

Para aproveitarmos dessas vantagens temos uma curva de aprendizagem.

### Tipos de controle de versionamento

Hoje temos dois tipos de controle versionamento famosos:

CVCS que é Centralized Version Control System, e DVCS que é a Decentralized version control system.

De controles de versionamento do tipo CVCS podemos citar o subversion(também o , que é o mais famoso, já de DVCS temos o git(também o mercurial ou y)

### Diferenças

A grande diferença entre esses dois tipos é como o próprio nome já diz.

CVCS trabalham sempre centralizados com um repositório. A seguinte imagem demonstra um pouco melhor:

![CVCS](http://rafael-miceli.com.br/ico/Controle-De-Versionamento-Introducao/CVCS.png)

DVCS __pode__ trabalhar com um repositório central, mas pode também não trabalhar com repositório central nenhum (apesar de muitos trabalharem sempre centralizando seus repositórios). Além disso usando DVCS você possui um clone do repositório central só seu em sua maquina aonde você pode realizar qualquer tipo de mudança. (Como criar branchs, realizar commits e mais...)

![DVCS](http://rafael-miceli.com.br/ico/Controle-De-Versionamento-Introducao/DVCS.png)

### Qual escolher?

Se você esta aprendendo e vai começar do zero recomendo __MUITO__ um DVCS, apesar de _IMHO_ ele possuir uma curva de aprendizado maior, hoje em dia é muito usado, possui muitas vantagens comparado aos CVCS e bem.... [Github](http://hitgub.com) né.

__MAS__... Se você já utiliza CVCS e precisa usar um controle de versionamento em um projeto e você não tem tempo hábil para aprender um novo continue no CVCS. A curva de aprendizagem vai acabar te tomando tempo (que as vezes não temos), e com CVCS você ainda consegue fazer o job.

Nos posts seguintes vou no focar no __git__ e como usar o mesmo para controle de versionamento de um projeto. 
Com a medida que for passando os posts vão ficar cada vez mais avançados e vamos conhecendo mais do __git__.
