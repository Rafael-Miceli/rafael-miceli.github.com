---
author: Rafael Miceli
comments: true
layout: default
category : MVC
title: "AspNet 5 e MVC 6"
date: 2015-05-06 18:00:00
---

Se você desenvolveu com AspNet MVC no passado, verá grande mudanças no MVC 6. Vamos dar uma breve olhada em algumas mudanças. 

### MVC não está mais no system.web 

Isso mesmo galera, e no que isto nos impacta? 

Bem, quando atualizamos nosso projeto usando o antigo MVC para o novo, podemos esperar que algumas coisas quebrem. 

### Onde está o global.asax? 

Você não vai mais achar o global.asax no seu projeto, mas você vai encontrar um arquivo startup.cs.  

Se você já usou **_katana_** ou algo relacionado a especificação owin, você vai encontrar uma familiaridade aqui. 

Com owin, quando você quer usar um novo middleware você cria um startup.cs que roda o método configure como mágica. 

Agora, grande parte das configurações de frameworks da nossa aplicação vai ocorrer neste método. 

### Dependency injection nativa 

Como mostrado, a forma que nossa aplicação AspNet inicia mudou, e com isso, o novo AspNet agora possui dependency injection nativa.  

Dentro de nossa classe startup nos podemos criar um método chamado configurationServices. Neste método nos podemos resolver nossas dependências ou usar um IOC container para o mesmo. 

### Onde está meu web.config? 

Se você se perguntou,"_aonde está o web.config?_" Bem, você está certo de uma coisa, não está mais aí. 

No passado configuração no .Net com os configs era complexa.  

Você tinha o machine.config, depois o web.config do seu projeto, e ainda talvez webs.config na estrutura de pastas do seu projeto. 

Hoje o novo AspNet não vem com nenhuma configuração! "_OK, vou então adicionar meu web.config_". Você pode fazer isso, mas o preferencial agora é que você adicione um config.json, claro que você ainda pode usar seu web.config, até arquivos init, até mesmo variáveis de ambiente! Mas vamos nos aprofundar mais nisso em um próximo post. 

### MVC ou web API? Que tal os dois? 

Nesta nova versão do AspNet, MVC e web API tornaram-se um só! Como? 

Se você criar um controller MVC você vai perceber que ele herda de controller e se você criar um web API controller, você vai ver que ele também herda de controller. Ambos no mesmo namespace! 

Mas no que isso nos ajuda? 

- Se você precisar de um global filter, vai pode ter apenas em um lugar.
- Se você precisar de um model binder customizado, vai poder ter apenas em um lugar 
- Quando a Microsoft lançar novas features, vai vir para os dois. 

E está é apenas uma parte do que está por vir. 

Este post é bem high level, no próximo vamos criar uma aplicação do zero no novo MVC. 
