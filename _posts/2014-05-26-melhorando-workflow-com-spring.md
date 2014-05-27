---
layout: post
title: "Melhorando seu workflow no Rails com Spring"
description: "Highlights do novo preloader nativo do Rails 4.1"
modified: 2014-05-26 19:30:34 -0300
tags: [rails, 4.1, ruby, spring, preloader]
image:
  feature: texture-feature-X1.jpg
  credit:
  creditlink:
share:
comments: true
published: true
---

Dia 18/04 foi lançada a tão esperada versão 4.1 do Rails. Nessa release
houveram grandes mudanças e features para tornar ainda mais eficiente
nosso workflow, sem falar no aumento de segurança da framework. Uma das features
introduzidas foi o **Spring**, uma ferramenta booter que recebeu melhorias e
agora se torna built-in no Rails 4.1.

## Spring

O spring é um preloader atualmente nativo que basicamente inicia sua aplicação
em background, tornando extremamente mais rápida qualquer ação que inicie o
ambiente de desenvolvimento do Rails, como specs, rake tasks e migrations.

### Na prática:

Inicializando uma pequena spec suite, temos os seguintes resultados sem a utlização do **spring**:

{% highlight bash %}
  time rspec spec/models/.
  ..

  Finished in 14.38 seconds
  520 examples, 0 failures

  18,38s user 2,11s system 94% cpu 21,742 total
{% endhighlight %}

#### Total: 21,74

São **21.74** segundos no total e **14.38** segundos rodando os testes,
o que nos dá **7.36 segundos de booting, esperando o teste começar a ser executado**.

Lembrando que este tempo irá variar pelo tamanho da aplicação, quantidade de
gems e máquina utilizada.

Após a execução do primeiro comando utilizando *spring*, sua aplicação já estará
rodando em background, portanto a execução do primeiro comando será lenta,
como no exemplo acima. A partir da segunda execução já podemos ver os resultados:

{% highlight bash %}
  time spring rspec spec/models/.
  ..

  Finished in 16.03 seconds
  520 examples, 0 failures, 1 pending

  0,08s user 0,02s system 0% cpu 17,728 total
{% endhighlight %}

#### Total: 17,73

Como podemos ver, ganhamos aproximadamente **4 segundos**, o que pode ser uma
grande diferença para equipes que praticam TDD ou executam testes com grande frequência.


### Conselhos & Pitfalls

- Setup rápido no [readme](https://github.com/rails/spring/blob/master/README.md)
- Você pode checar o status da sua aplicação pelo comando **spring status**.
- Evite manter a aplicação em background por mais de 1 dia, pois o spring
não recarrega a aplicação de tempo em tempo, o que pode causar instabilidade em
specs que utilizam Date, Time, e DateTime. Neste caso utilize **spring stop**.


## Links

- Guia de upgrade para Rails 4.1 - [Link](http://edgeguides.rubyonrails.org/upgrading_ruby_on_rails.html#upgrading-from-rails-4-0-to-rails-4-1).
- Rails 4.1 Release Notes - [Link](http://edgeguides.rubyonrails.org/4_1_release_notes.html)
- Spring Gem - [Link](https://github.com/rails/spring/)



