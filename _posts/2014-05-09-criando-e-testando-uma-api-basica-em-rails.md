---
layout: post
title: Criando e testando uma API básica no Rails
tags: [rspec, spec, rails, json, api]
comments: true
---

## Rails & HTTP

O rails é uma poderosa framework que nos permite desenvolver de forma ágil e produtiva,
possibilitando foco no que realmente temos como fim, não como meio.

Em vista disso, hoje iremos aprender como criar uma api simples para servir
dados de nossas aplicações no formato [JSON](http://pt.wikipedia.org/wiki/JSON).

Partindo do princípio básico do [HTTP](http://pt.wikipedia.org/wiki/Hypertext_Transfer_Protocol),
primeiramente precisamos de uma URL que responda à requisições *GET*.

## routes.rb

Criando namespaces exclusivos para API nos ajudará a isolar comportamentos, direcionando para uma
controller que terá o papel único de retornar dados em *JSON*. Neste caso estamos
direcionando para controller **api/v1/users_controller.rb**.

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


### api/v1/users_controller.rb

Nossa controller neste momento terá apenas uma action (index), e esta irá responder
com todos os usuários de nossa aplicação.

{% highlight ruby %}
class Api::V1::UsersController < ApplicationController
  def index
    @users = User.all
    render json: @users
  end
end
{% endhighlight %}

### Specs

Neste caso estamos testando se o resposta da requisição HTTP é 200 (success),
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

Com apenas isso já temos o necessário para responder requisições GET com todos
objetos de usuários de nossa aplicação em formato *JSON*, possibilitando
sua utilização por qualquer outra aplicação web.

### E esse é o fim do início.

Este é um caso super simples, e claro, podemos melhora-lo utilizando mais models e
autenticação/autorização com as gems Devise e Cancan, tornando sua API mais completa e segura.

Em breve criarei um post mais completo e com mais exemplos, por enquanto ficamos por aqui..
