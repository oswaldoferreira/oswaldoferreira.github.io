---
layout: post
title: "Armazenando em Hstore - Pitfalls &amp; Hints"
description: Indicações de pitfalls e hints enquanto percorremos as possibilidades do Hstore (utilizando Rails)
modified: 2014-09-29 22:49:50 -0300
tags: [hstore, postgres, rails]
image:
  feature: texture-feature-X2.jpg
  credit:
  creditlink:
comments: true
published: true
---

O hstore é utilizado para serializar e armazenar estruturas Hash no Postgres,
possibilitando queries e indexações desses dados de maneira simples e rápida. Neste post indicarei pitfalls e hints enquanto percorremos grande parte das possibilidades dessa ferramenta.

## Utilização

Normalmente, ele é utilizado quando precisamos armazenar dados que não possuem grandes vínculos no sistema, que tendem a se multiplicar, e não possuem atributos tão fixos. Bons candidatos ao Hstore: localização e histórico.


## Setup (Migration):

{% highlight ruby %}
class SetupHstore < ActiveRecord::Migration
  def self.up
    execute "CREATE EXTENSION IF NOT EXISTS hstore"
  end

  def self.down
    execute "DROP EXTENSION IF EXISTS hstore"
  end
end
{% endhighlight %}

## Criando uma tabela com atributo Hstore (Migration):

{% highlight ruby %}
class CreateCabs < ActiveRecord::Migration
  def change
    create_table :cab do |t|
      t.float :traveled_distance
      t.hstore :properties

      t.timestamps
    end
  end
end
{% endhighlight %}

## Playing around

{% highlight ruby %}

cab = Cab.create!(properties: { lat: 123, lng: 321, active: true })
cab['properties']
=> {"lat"=>"123", "lng"=>"321", "active"=>"true"}

{% endhighlight %}

#### As keys/values do hstore armazenado sempre serão convertidos em strings, não importando o objeto ruby. O postgres armazena apenas strings nestes campos.

## Query

Contem a key 'active'

{% highlight ruby %}
Cab.where("properties ? 'active'")
{% endhighlight %}

Onde a key 'lat' for '123'

{% highlight ruby %}
Cab.where("properties -> 'lat' = '123'")
{% endhighlight %}

A mesma query 2x mais rápida (utilizando indexes)

{% highlight ruby %}
Cab.where("data @> 'foo=>bar'")
{% endhighlight %}


Onde 'active' for diferente de 'true':

{% highlight ruby %}
Cab.where("properties -> 'active' <> 'true'")
{% endhighlight %}

#### Já que o hstore converte todas keys/values para string, atenção ao fazer boolean queries, sempre force um *.to_s* para garantir.

Objetos onde 'lng' se parece com '23'

{% highlight ruby %}
Cab.where("properties -> 'lng' LIKE '%23%'")
{% endhighlight %}

## Armazenando listas como valor

Já sabemos que o Hstore transforma keys/values em strings, mas ainda assim podemos armazenar listas como valor. Elas são persistidas e retornam em formato *Json*, o que torna o *parse* muito fácil.

{% highlight ruby %}
cab = Cab.create(properties: { lat: 11122, lng: 33221, drivers: ['Donnie', 'Jane'] })
cab.properties['drivers']
=> "['Donnie', 'Jane']"

JSON.parse(cab.properties['drivers'])
=> ['Donnie', 'Jane']
{% endhighlight %}

#### Hashes nested não funcionam. O hash interno é convertido em string, e o postgres não consegue montar um *Json* de retorno pra esse valor. Uma solução que não recomendo é a utilização do método *eval* na hash string retornada (tendências cabalisticas).

## Adicionando atributos extras (coool)

Uma vantagem bem legal do hstore é a similaridade ao mundo NoSql, onde novas keys/values podem ser inseridas a qualquer momento, de maneira bem fácil. Por exemplo, podemos decidir inserir a numeração da placa do nosso *Cab*, mas nem todos objetos precisam ter esse atributo. Reparem que faço um merge dos atributos antigos para não perde-los.

{% highlight ruby %}
cab.update!(properties: cab.properties.merge(plate: 'XXX3344'))
cab.properties
=> { "lat"=>"11122", "lng"=>"33221", "active"=>"true", "drivers"=>['Donnie', 'Jane'], "plate"=>'XXX3344' }
{% endhighlight %}

## Acessando valores de maneira simples

1. Podemos criar manualmente os accessors de nossas propriedades

{% highlight ruby %}
def drivers
  self.properties['drivers']
end
{% endhighlight %}

2. Ou podemos utilizar o accessor helper

{% highlight ruby %}
store_accessor :properties, :drivers
{% endhighlight %}

#### O store_accessor ainda habilita a utilização das validações de atributos do Rails.

{% highlight ruby %}
store_accessor :properties, :lat, :lng
validates :lat, :lng, presence: true
{% endhighlight %}


## Concluindo

O hstore facilita demais o manejo de flat hashes (um nível de nesting). Já que
temos acesso às validações do Rails, podemos assegurar um tipo de dado de um atributo,
e apenas fazer a conversão no caminho de volta.

Se o caso for mais complexo, envolvendo nested hashes e mais "relações", uma das opções é a utilização
do formato JSON, onde acabariamos abrindo mão de benefícios que o Hstore
proporciona (como queries, indexes, etc). No entando, o melhor dos mundos neste
caso seria optar por um banco NoSql, como MongoDB, CouchDB, entre outros.

