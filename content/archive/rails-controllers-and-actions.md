---
title: "Rails Controllers and Actions"
draft: false
---

Deep down somewhere it's always bothered me how Rails handles the concept of Controllers in it's custom MVC framework. If you don't know I'll attempt to explain: In Rails you have a router. The router decides where requests go based on some meta-method magic. For example lets take this Rails routes.rb file:

``` ruby
Blog::Application.routes.draw do
  resources :ideas
  resources :articles
end
```

Rails takes these instructions and understands a few things from it:

  1. There's a `ideas` resource
    - And that includes a `IdeasController`
    - And it will have 7 Actions
    - And gets mapped to the `/ideas` path
    - And generates `*idea(s)_(path|url)` methods
  2. There's a `articles` resource
    - And that includes a `ArticlesController`
    - And it will have 7 Actions
    - And gets mapped to the `/articles` path
    - And generates `*article(s)_(path|url)` methods

Those seven actions are as follows:

  1. `GET /ideas` => `IdeasController#index`
  2. `GET /ideas/new` => `IdeasController#new`
  3. `POST /ideas` => `IdeasController#create`
  4. `GET /ideas/edit` => `IdeasController#edit`
  5. `PUT /ideas/:id` => `IdeasController#update`
  6. `GET /ideas/:id` => `IdeasController#show`
  7. `DELETE /ideas` => `IdeasController#destroy`

Each of these methods would be defined on the `IdeasController` class like so:

``` ruby
class IdeasController < ApplicationController
  def show

  end

  def index

  end

  def new

  end

  def create

  end

  def edit

  end

  def update

  end

  def destroy

  end
end
```

Notice anything strange about this whole setup? Despite each being a class and each class having entirely different functionality (one in the context of an ideas resource and the other an articles resource) neither has an `initialize` method that you control. There other thing you might notice is that the action methods themselves portray these class as having multiple presentations.

That's not what is actually going on, though. These "actions" each encapsulate entirely different logics instead of taking the same data and presenting it a different way. The update, destroy, and create function, for instance, has no presentation.

Here's what I've wanted out of Rails lately:

``` ruby
class Ideas::New < Controller::Action
  expose :idea

  def initialize(header, parameters)
    authenticate_http_token(header)

    idea = Idea.new parameters[:idea]
  end

  def to_html
    html do
      header do
        h1.title { idea.title }
      end
      paragraph.body { idea.body }
    end
  end

  def to_json
    Oj.dump(idea.attributes)
  end
end
```
