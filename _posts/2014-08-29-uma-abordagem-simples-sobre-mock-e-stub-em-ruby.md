---
layout: post
title: "Introdução à Mock e Stub com Rspec"
description: "Como aplicar ''fake'' objects em seus testes em Ruby"
modified: 2014-08-29 00:09:53 -0300
tags: [mock, stub, ruby, boas praticas, rspec, tdd, unit test]
image:
  feature: texture-feature-X1.jpg
  credit:
  creditlink:
comments: true
published: true
---

Em determinadas situações, onde múltiplas classes são desenvolvidas quase simultaneamente,
  podemos utilizar objetos *fake* para criar suas relações. Essa técnica possui uma função simples e bem definida, manter a aplicação isoladamente testada.


A utilização do **mock** permite gerar objetos que fazem o papel de dublê de outra classe (que muitas vezes ainda nem foi criada) em um teste, ou seja, retiramos a necessidade (dependência) da classe que não estamos testando neste momento.


O **stub** nada mais é que uma saída forjada para uma chamada à um objeto, ou seja,
ele responde como queremos à mensagens enviadas, atuando como um dublê de métodos.

### Exemplo

Vamos estabelecer o seguinte exemplo bem abstrato aplicando o
[TDD](http://pt.wikipedia.org/wiki/Test_Driven_Development):

#### “Uma pessoa só é feliz quando está no Park"

{% highlight ruby %}
  describe Person do
    let(:place)  { double('Place') }
    let(:person) { Person.new place: place }

    describe 'is_happy?' do
      it 'when true' do
        place.stub(:name).and_return('Park')
        expect(person.is_happy?).to be_true
      end

      it 'when false' do
        place.stub(:name).and_return('Any other place')
        expect(person.is_happy?).to be_false
      end
    end
  end
{% endhighlight %}

{% highlight ruby %}
  class Person
    def initialize(params)
      @place = params.fetch(:place)
    end

    def is_happy?
      place.name == 'Park'
    end
  end
{% endhighlight %}

Neste exemplo testamos qual seria a influência de uma classe externa (Place),
que ainda não existe. Reparem na utilização do **double('Place')** e o **stub(:name)**.
O *double* é o mock de place, ou seja, um objeto fictício, enquanto o *stub* gera
um método em cima do mock. Sendo assim, podemos testar os envios de mensagem internamente,
criando no próprio teste um contrato de como será implementada sua classe **Place**,
por exemplo.

### Links importantes

- Syntax [Mock & Stub](https://relishapp.com/rspec/rspec-mocks/docs)
- Interessado em TDD? [Test Driven Development: By Example](http://www.amazon.com/Test-Driven-Development-By-Example/dp/0321146530)




