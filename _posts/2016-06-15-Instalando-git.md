---
author: Rafael Miceli
comments: true
layout: default 
category : git
title: "Instalando git" 
date: 2016-06-15 10:30:00
---

Git é provavelmente a ferramenta de controle de versionamento mais famosa atualmente.

Criado em 2005 pela comunidade Linux e pelo Linus Torvald, é extremamente poderoso para o trabalho com múltiplos branchs e até por isso exige uma certa curva de aprendizado.

E vamos ao aprender.

### Instalando git

Antes de usar o git precisamos instalar o mesmo. Para isso vamos no seguinte link: https://git-scm.com/downloads

Após baixar vamos começar a instalar o git seguindo wizard.

Lembrando que esses são os passos do git para windows.

### Tela 1

Na primeira tela aceitamos a licença clicando em Next.

### Tela 2

![passo1](http://rafael-miceli.com.br/ico/Instalando-git/passo1.png)

Na segunda escolhemos quais componentes queremos instalar como por exemplo ícone adicional na área de trabalho, ou adicionar a opção de abrir com o __Git Bash__ na pasta em que estiver clicando com o botão direito.

### Tela 3

![passo2](http://rafael-miceli.com.br/ico/Instalando-git/passo2.png)

A terceira tela você configura o __PATH__ da sua maquina em uma das seguintes opções.

1 - Você não modifica seu __PATH__, e para você usar o comando _git_ vai conseguir usar apenas no __Git Bash__

2 - A segunda opção você insere em seu __PATH__ o comando _git_, e assim consegue usar o comando tanto no __Bash__ quanto no __Command Prompt__ ou __Powershell__.

3 - A ultima opção o além de inserir o comando _git_ no __PATH__ ele também vai inserir algumas ferramentas _Linux_ como: _find_ ; _kill_ ; _sort_, e entre 20 a 30 outros.

Essa última opção você tem de estar bem ciente do que está escolhendo. Pois algumas utilidades ambos os Sistemas Operacionais (Linux e Windows) possuem alguns comandos como o _find_, _kill_ e _sort_. Apesar do comando ser o mesmo, eles funcionam de forma diferente (não vou entrar em tantos detalhes).

Selecione a opção 2.

### Tela 4

![passo3](http://rafael-miceli.com.br/ico/Instalando-git/passo3.png)

Na quarta tela temos mais 3 opções.

1 - Está é a opção recomendada para __Windows__, uma vez que quebra de linha em arquivos são diferentes no Windows e no Linux/MAC:

>Falando um pouco de quebra de linha (que por si só é um assunto com mais história por trás), nós tempos antigos da maquina de escrever o termo _LF_ (usado no linux e no MAC) significa __Line Feed__ e era usado para quebrar a linha, mas matinha o _cursor_ da máquina na mesma posição horizontal. Enquanto __CR__ significa __Carriage Return__, que é para fazer o _cursor_ voltar ao inicio da página. Windows usa como quebra de linha o código CRLF enquanto o Linux LF.
 
Nesta opção cada vez que você realizar o commit de um arquivo ele vai ser convertido de CRLF para LF, e toda vez que você realizar o checkout ele vai converter de LF para CRLF.

2 - Nesta opção você vai realizar o checkout do jeito que vier, ou seja, se vier LF, vai continuar LF, não vai transformar para CRLF, mas ao fazer commit, ele vai tranformar de CRLF para LF.

3 - Nesta opção (como de se esperar) ao realizar checkout vai vir como LF e ao realizar commit não vai transformar de CRLF para LF, vai mandar como CRLF mesmo.

Selecione opção 1.

### Tela 5

![passo4](http://rafael-miceli.com.br/ico/Instalando-git/passo4.png)

Nesta quinta tela escolhemos entre usar o terminal __MinTTY__ ou o terminal default do windows __Command Prompt (cmd)__. Nunca usei o MinTTY mas muitos falam que ele é excelente, principalmente para quem não está no windows 10.

Algumas das vantagens que o Mintty oferece são:

Redimensionar a tela para qualquer tamanho (até maximizar)
Ações de copiar e colar mais fácil
Mais rápido

Selecione a opção do Mintty. E lembre-se que isso anda não impede de você usar o comando _git_ no cmd ou Powershell uma vez que você adicionou ele a seu __PATH__.

### Tela 6

![passo5](http://rafael-miceli.com.br/ico/Instalando-git/passo5.png)

Nesta tela podemos ativar as seguintes opções:

- Habilitar file system caching.

Podemos selecionar esta opção uma vez o _filesystem_ do windows é bem diferente do linux (no qual o git foi otimizado), com isso ganhamos um boost em performance, visto que em algumas situações o git pode ficar mais lento.

- Habilitar Git Credential Manager (GCM)

Podemos selecionar a opção de habilitar o __Git Credential Manager (GCM)__, que basicamente é uma funcionalidade para você armazenar sua usuário e senha, e não precisar ficar escrevendo os mesmos a cada sincronização com seus repositórios. O GCM possuí bastante funcionalidade e caso você queira conhecer um pouco mais a fundo recomendo o post feito a respeito apenas a ele no [blog da msdn](https://blogs.msdn.microsoft.com/visualstudioalm/2015/12/08/announcing-the-git-credential-manager-for-windows-1-0/).

Pode manter as duas opções selecionadas.

E após isso, a instalação vai rolar com todas as opções que escolhemos anteriormente.

No próximo post vamos criar um repositório local e como funciona o git localmente.