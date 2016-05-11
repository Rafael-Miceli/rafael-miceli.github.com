---
author: Rafael Miceli
comments: true
layout: default 
category : OAuth 
title: "Publicando uma aplicação MVC e seus settings" 
date: 2016-05-11 09:30:00
---

Quando usamos o Publish Web Wizard para publicarmos nossa aplicação web, no menu Settings temos 3 opções que geralmente não entendemos completamente seu funcionamento. Vamos passar por elas rapidamente e entender um pouco mais o que elas podem causar em alguns cenários.

![passo1](http://rafael-miceli.com.br/ico/Publicando-uma-aplicacao-MVC-e-seus-settings/passo1.png)

### Opção Remove additional files at destination

Nesta opção passando o olho rápido quando vamos publicar temos a impressão que vamos apagar todos os arquivos e colocar novos no lugar.

Na verdade o que o Web Deploy faz nessa ocasião é comparar a estrutura de pastas que está no destino com a estrutura atual que estamos para publicar e remove do servidor o que foi removido da estrutura atual.

Exemplo:

Digamos que você não queira mapear seu web.config para ser publicado junto de sua aplicação, pois você não quer ter acidentes de mudanças neste config. 

Então você adiciona o web.config que você quer uma vez no servidor e depois publica seu site sem ele ir junto.

Ao publicar o site se selecionar a opção __Remove additional files at destination__ como o web.config não está mapeado no projeto, ele vai ser excluido e não queremos isso.

Concluindo, essa opção não exclui tudo e coloca os novos no lugar, ela compara a estrutura do nosso projeto e __REMOVE__ o que não estiver na estrutura do projeto que estamos publicando.

### Opção Precompile during publishing

Selecionando esta opção, no momento da publicação, nossa aplicação pré compila nossas Views e códigos dentro da pasta App_Code.

Isso é muito útil quando possuímos uma aplicação de grande porte, pois quando realizarem o primeiro acesso ao sistema, ao invés de ser aquela demora que costumar ser (pelo motivo de pré compilar _on the fly_), o site vai responder muito mais rápido.

Um detalhe importante, é que está opção __não resolve problemas de performance__.

### Opção Exclude files from the App_Data folder

Esta é a opção mais fácil de entender, selecionando a mesma excluímos os arquivos (geralmente _.mdf_) desta pasta.

Uma vez que na grande maioria das vezes hoje em dia nossa aplicação vai acessar um banco de dados SqlExpress não há tanta necessidade de termos um arquivo .mdf na mesma.

### Conclusão

Passamos por essas opções quase sempre que vamos publicar uma aplicação web e agora entendemos melhor como elas funcionam.

Em um outro post vamos nos adentrar um pouco nas opções avançadas da opção __Precompile during publishing__.
