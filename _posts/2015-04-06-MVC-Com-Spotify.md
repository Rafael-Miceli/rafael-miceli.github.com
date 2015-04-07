---
author: Rafael Miceli
comments: true
layout: default
category : MVC
title: "MVC com Spotify"
date: 2015-04-06 09:00:00
---

Na semana passada eu tive de criar um projeto que consumisse a API do Spotify. Eu pesquisei uma maneira de fazer isso utilizando AspNet MVC, mas encontrei muito pouca coisa sobre como consumir com .Net, e menos ainda com MVC. Então, cá estou eu para compartilhar como foi minha experiência com isso.

Para consumir da API do Spotify, a primeira coisa que precisamos é criar um Spotify Application. Para isso acesse o link: https://developer.spotify.com

![passo 1](http://rafael-miceli.com.br/ico/MVC-com-Spotify/passo1.png)

![passo 2](http://rafael-miceli.com.br/ico/MVC-com-Spotify/passo2.png)

Escolha um nome e uma descrição para sua Spotify Application

![passo 3](http://rafael-miceli.com.br/ico/MVC-com-Spotify/passo3.png)

Após ter criado a application, o Spotify vai nos dar um Clien ID e vai nos pedir um Redirect URI.

![passo 4](http://rafael-miceli.com.br/ico/MVC-com-Spotify/passo4.png)

O Redirect URI é o endereço que o Spotify usa para nos mandar um redirect com o Token que usaremos para fazer as subsequentes chamadas à API. 
Por enquanto vamos deixar o Redirect URI em branco, vamos volta a ele mais tarde.
Ok, vamos começar implementando a autenticação. Se você for acessar músicas, playlists ou o perfil de um usuário você tem que estar autenticado para isto, mas primeiro precisamos decidir que método de autenticação vamos usar. Spotify permite nos autenticarmos de 3 formas diferentes.
Para este projeto vamos usar “Implicit Grant Flow”.

Se você quiser saber mais sobre autenticações com o Spotify acesse a documentação deles:
https://developer.spotify.com/web-api/authorization-guide

Ótimo, vamos começar a codar!

###Mãos no código!

Crie um projeto MVC empty e crie um controller “Home” com uma Action Index e sua respectiva View.

![passo 5](http://rafael-miceli.com.br/ico/MVC-com-Spotify/passo5.png)

Vamos rodar nosso site para ver se está funcionando.

![passo 6](http://rafael-miceli.com.br/ico/MVC-com-Spotify/passo6.png)

Excelente, agora para nos autenticarmos, crie um botão de login, que vai ser um link para nosso projeto se autenticar com o Spotify. No atributo href vamos pegar o valor de uma propriedade ViewBag chamada “AuthUri”.

![passo 7](http://rafael-miceli.com.br/ico/MVC-com-Spotify/passo7.png)

Em nosso controller vamos setar nossa propriedade AuthUri.

![passo 8](http://rafael-miceli.com.br/ico/MVC-com-Spotify/passo8.png)

Explicando as variáveis acima:

- O “clientId”, representar o valor de nosso Client ID no Spotify Application.
- “redirctUri” representa o valor de nossa Redirect URI no Spotify Application.
- Scope representa cada permissão que estamos pedindo para acessar na sua conta Spotify.

Como você pode ver Scope ainda não existe. Vamos cria-lo.

**Scope**


    [Flags]
    public enum Scope
    {
        [StringAttribute("")]
        NONE = 1,
        [StringAttribute("playlist-modify-public")]
        PLAYLIST_MODIFY_PUBLIC = 2,
        [StringAttribute("playlist-modify-private")]
        PLAYLIST_MODIFY_PRIVATE = 4,
        [StringAttribute("playlist-read-private")]
        PLAYLIST_READ_PRIVATE = 8,
        [StringAttribute("streaming")]
        STREAMING = 16,
        [StringAttribute("user-read-private")]
        USER_READ_PRIVATE = 32,
        [StringAttribute("user-read-email")]
        USER_READ_EMAIL = 64,
        [StringAttribute("user-library-read")]
        USER_LIBRARAY_READ = 128,
        [StringAttribute("user-library-modify")]
        USER_LIBRARY_MODIFY = 256,
        [StringAttribute("user-follow-modify")]
        USER_FOLLOW_MODIFY = 512,
        [StringAttribute("user-follow-read")]
        USER_FOLLOW_READ = 1024,
        [StringAttribute("user-read-birthdate")]
        USER_READ_BIRTHDATE = 2048
    }

    public static class ScopeUtil
    {
        public static string GetStringAttribute<T>(this T en, String separator) where T : struct, IConvertible
        {
            Enum e = (Enum)(object)en;
            IEnumerable<StringAttribute> attributes =
            Enum.GetValues(typeof(T))
            .Cast<T>()
            .Where(v => e.HasFlag((Enum)(object)v))
            .Select(v => typeof(T).GetField(v.ToString()))
            .Select(f => f.GetCustomAttributes(typeof(StringAttribute), false)[0])
            .Cast<StringAttribute>();

            List<String> list = new List<String>();
            attributes.ToList().ForEach((element) => list.Add(element.Text));
            return string.Join(" ", list);
        }
    }


**StringAttribute**

	public class StringAttribute : Attribute
    {
        public String Text { get; set; }
        public StringAttribute(String text)
        {
            this.Text = text;
        }
    }


Ótimo, agora temos nossa url para login pronta! Após pressionar o link, vamos ser redirecionados a página de autenticação do Spotify, que vai nos perguntar se permitimos que nossa Aplicação acesse informações (definidas pelo nosso objeto Scope) de nossa conta.
Se você aceitar o Spotify vai nos redirecionar para outra página, que seria? A Uri que definimos em nosso Redirect URI.
Vamos fazer isto. Vamos pegar a Url que nosso IIS subiu nossa aplicação, e vamos criar uma nova Action, para recebermos o redirect do Spotify.

![passo 10](http://rafael-miceli.com.br/ico/MVC-com-Spotify/passo10.png)

![passo 11](http://rafael-miceli.com.br/ico/MVC-com-Spotify/passo11.png)

![passo 12](http://rafael-miceli.com.br/ico/MVC-com-Spotify/passo12.png)

Após confirmarmos que nossa aplicação pode consumir dados da nossa conta Spotify, Spotify vai nos redirecionar para o endereço da Redirect URI com o Token que precisamos para as sub seguintes chamadas à API.

![passo 13](http://rafael-miceli.com.br/ico/MVC-com-Spotify/passo13.png)

Mas, tem um catch ai!

O Spotify nos envia o token como um parâmetro e não como QueryString. Infelizmente o Server Side de nossa aplicação não busca valores após o símbolo `#`.
A maneira mais fácil de pegar os valores que ficam após o símbolo `#` é colocando um pouco de *azeite* em nosso código como o seguinte código Javascript:

![passo 14](http://rafael-miceli.com.br/ico/MVC-com-Spotify/passo14.png)

Com este Javascript nós substituímos o símbolo `#` por `?`, transformando nosso parâmetro em QueryString.
Agora só precisamos capturar a QueryString com parâmetros em nossa Action e _Voila_!

![passo 15](http://rafael-miceli.com.br/ico/MVC-com-Spotify/passo15.png)

Aqui estão nosso Token e outros valores.

![passo 16](http://rafael-miceli.com.br/ico/MVC-com-Spotify/passo16.png)

Após isto podemos realizar as chamadas para a API do Spotify e pegarmos os nomes de nossas playlists.
Mas isso fica para o próximo artigo...

###Update

O código usado para este artigo estão diponiveis no meu [Github](https://github.com/Rafael-Miceli/Blog-Codes/tree/master/IdeiasComAzeite/SpotifyApiConsumer/SpotifyConsumer)
