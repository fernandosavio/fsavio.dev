---
type: post
author: Fernando Sávio
date: 2016-06-06T05:00:00Z
title: "Disqus: usando tags HTML"
description: "Uma 'documentação' pessoal (cheatsheet) das tags aceitas pelo Disqus."
categories: 
  - html
  - disqus
tags:
  - disqus
  - comentarios
---

Tenho visto muita gente "alertando" seus spoilers nos comentários do [Disqus] de vários sites da seguinte maneira:

```
[SPOILERS]
Lorem ipsum dolor sit amet, consectetur adipiscing elit. Quisque quis tellus lectus. 
Fusce egestas magna quis ligula tempus scelerisque. Etiam risus nunc, consequat sed 
accumsan vel, rhoncus ut augue. In venenatis elit vitae urna sollicitudin suscipit. 
Mauris vulputate pharetra pellentesque. Mauris a urna nibh. Cras varius pretium erat. 
Duis neque tortor, sagittis vel volutpat vel, maximus nec dui. Mauris vel tellus dui. 
Sed sollicitudin ligula eros, vel ornare elit semper nec.
```

ou

```
Spoiler!!!!!
.
.
.
.
.
.
.
.
.
. (aqui teria um "Leia mais")
.
.
.
.
.
Lorem ipsum dolor sit amet, consectetur adipiscing elit. Quisque quis tellus lectus. 
Fusce egestas magna quis ligula tempus scelerisque. Etiam risus nunc, consequat sed 
accumsan vel, rhoncus ut augue. In venenatis elit vitae urna sollicitudin suscipit. 
Mauris vulputate pharetra pellentesque. Mauris a urna nibh. Cras varius pretium erat. 
Duis neque tortor, sagittis vel volutpat vel, maximus nec dui. Mauris vel tellus dui. 
Sed sollicitudin ligula eros, vel ornare elit semper nec.
```

Depois de ver estes comentários fui ao nobre amigo Google para lembrar quais são as *tags* aceitas pelo [Disqus]. Foi onde encontrei [este post](disqus_tags) onde mostra as tags aceitas pelo [Disqus].

Enfim, decidi documentar aqui no meu blog para facilitar minha vida (e de mais alguém, espero) e também vou deixar um resumo dessas *features* antes de cada sessão de comentário como uma *cola* para quem for comentar aqui no blog.

Bora lá, começando pela famigerada tag `<spoiler>`!


## &lt;spoiler&gt;
**Código**:
```html
Lorem ipsum dolor sit amet, <spoiler>consectetur</spoiler> adipiscing 
elit. Quisque quis tellus lectus.
```
**Resultado**:

![Resultado para a tag spoiler](/images/post/disqus/disqus_spoiler_tag.gif)


## &lt;a&gt;

**Código**:
```html
Lorem ipsum dolor sit amet, <a href="https://mussumipsum.com/">consectetur</a> 
adipiscing elit. Quisque quis tellus lectus.
```
**Resultado**:

![Resultado para a tag a](/images/post/disqus/disqus_link_tag.png)



## &lt;strong&gt; ou &lt;b&gt;

**Código**:
```html
Lorem ipsum dolor sit amet, <strong>consectetur</strong> adipiscing 
elit. <b>Quisque</b> quis tellus lectus.
```
**Resultado**:

![Resultado para a tag strong](/images/post/disqus/disqus_strong_tag.png)



## &lt;em&gt; ou &lt;i&gt;

**Código**:
```html
Lorem ipsum dolor sit amet, <em>consectetur</em> adipiscing 
elit. <i>Quisque</i> quis tellus lectus.
```
**Resultado**:

![Resultado para a tag em](/images/post/disqus/disqus_emphasis_tag.png)




## &lt;strike&gt; ou &lt;s&gt;

**Código**:
```html
Lorem ipsum dolor sit amet, <strike>consectetur</strike> adipiscing 
elit. <s>Quisque</s> quis tellus lectus.
```
**Resultado**:

![Resultado para a tag strike](/images/post/disqus/disqus_strike_tag.png)




## &lt;blockquote&gt;

**Código**:
```html
<blockquote>
Lorem ipsum dolor sit amet, consectetur adipiscing elit. 
Quisque quis tellus lectus.
</blockquote>
```
**Resultado**:

![Resultado para a tag blockquote](/images/post/disqus/disqus_blockquote_tag.png)




## &lt;pre&gt; e &lt;code&gt;

*Código*:
```html
<pre><code class="python">
## Fibonacci using recursion
def fib(n):
    if n==1 or n==2:
        return 1
    return fib(n-1)+fib(n-2)

print fib(5)
</code></pre>
```


Escapando HTML nos códigos:

*Código*:
```html
<pre><code class="html">
Lorem ipsum dolor sit amet, &#60;strong&#62;consectetur&#60;/strong&#62; 
adipiscing elit. Quisque quis tellus lectus. &#60;strike&#62;Fusce 
egestas&#60;/strike&#62; magna quis ligula tempus scelerisque.
</code></pre>
```

*Resultado*:

![Resultado para a tag code](/images/post/disqus/disqus_code_tag.png)



*PS: as tags `<cite>`, `<span>`, `<p>`, `<caption>` e `<table>` não foram incluídas no post por não ter efeito aparente nos testes.*


[Disqus]: https://disqus.com/
[disqus_tags]: https://help.disqus.com/customer/portal/articles/466253-what-html-tags-are-allowed-within-comments-
