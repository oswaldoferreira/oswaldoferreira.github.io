---
layout: post
title: Criando e testando uma API básica em Rails
description: "Abordagem simples para utilização do Rails para servir dados para web."
tags: [rspec, spec, rails, json, api]
image:
  feature:
  credit:
  creditlink:
comments:
share:
---

## Rails & HTTP

O rails é uma poderosa framework que nos permite desenvolver de forma ágil e produtiva,
possibilitando o foco no que realmente temos como fim, não como meio.

Em vista disso, hoje iremos aprender como criar uma api simples para servir
dados de nossas aplicações no formato [JSON](http://pt.wikipedia.org/wiki/JSON).

Partindo do princípio básico do [HTTP](http://pt.wikipedia.org/wiki/Hypertext_Transfer_Protocol),
primeiramente precisamos de uma URL que responda à requisições *GET*.

### Routes

Com esta rota geramos uma url que nos encaminha para o controller **Users**.

{% highlight ruby %}
  namespace :api do
    namespace :v1 do
      get :users, to: 'users#index'
    end
  end
{% endhighlight %}

Output do **rake routes**:

{% highlight bash %}
  api_v1_users GET  /api/v1/users(.:format) api/v1/users#index
{% endhighlight %}


### Controller

Nossa controller tem um papel simples. Retornar todos usuários em formato *JSON*.

{% highlight ruby %}
class Api::V1::UsersController < ApplicationController
  def index
    @users = User.all
    render json: @users
  end
end
{% endhighlight %}

### Specs

Neste caso estamos testando se o retorno da resposta HTTP é 200 (success),
transformando o JSON em um Hash e validando seu retorno.

{% highlight ruby %}
describe Api::V1::UsersController do
  context 'GET index' do
    it 'when users' do
      User.create! name: 'John', birthday: Date.parse('14/03/1991')
      get 'index'
      expect(response).to be_success

      json = JSON.parse(response.body)
      expect(json).to eq [{"id"=>1, "name"=>"John", "birthday"=>"1991-03-14"}]
    end

    it 'when no users' do
      get 'index'
      expect(response).to be_success

      json = JSON.parse(response.body)
      expect(json).to be_empty
    end
  end
end
{% endhighlight %}

Com apenas isso já temos um serviço que dispõe uma rota para retorno de todos usuários.

### E esse é o fim do início.

Este é um caso super simples, e claro, podemos melhora-lo utilizando mais models e
autenticação/autorização com as gems Cancan e Devise, tornando sua API mais segura.

Em breve criarei um post mais completo e com mais exemplos, por enquanto ficamos por aí..










