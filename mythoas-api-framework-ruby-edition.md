If you're visiting my blog you might already be familiar with my opinion on web API standards and practices. If not I'll give you the quick bullet points:

  - I love [HTTP][HTTP]
  - If I have a choice the server is going to be behaving according to the [REST][REST] constraints
  - Furthermore I wildly prefer using the constraints defined for [HATEOS]()
  - The format of the messages will probably be in JSON
  - The object bodies will follow the [application/vnd.api+json][JSONAPI] media type

That should give you a complete enough picture of me as a API developer and also an idea of how Shogun works.

I created Shogun in an attempt to standardize what I was doing in various other frameworks, like [Rails][RAILS], [Sinatra][SINATRA], [Grape][GRAPE], and even [Crepe][CREPE]. These tools are in their own right very good and any of them could have been a home for me. I found myself more often than not modifying things, adding pieces, and removing settings each project. Any of these changes could theoretically be packaged into a library, but I also found myself not liking the internals of these gems.

I'll admit straight up I'm a snob when it comes to software architecture. For what I do know I have a very strong opinion on. I like to be given objects that I can substitute for whatever the developer thinks I should be using. I like to have access to values, read from `ENV`, and other things that make changes easy. Libraries are meant to be shared to save time and should be open to the very real possibility of change.

With this in mind I set out to write my own framework. The task seemed daunting at first, but after the first couple of mistakes and hours I realized HTTP frameworks like this are four components:

  - A router middleware(s)
  - A set of configurations
  - Some business logic molds
  - A scaffolding command

Shogun's router is incredibly simple because I didn't want to spend a lot of early investment on something I knew would change. It sits as a Rack middleware and is given a anonymous function of routes:

``` ruby
->(router) do
  Accounts::Endpoint.new(router: router)
  Sessions::Endpoint.new(router: router)
end
```

Each of these `Endpoint` objects attaches a `Route` object to the `Router` object. Here's what a `Endpoint` looks like:

``` ruby
class Endpoint
  include Shogun::Endpoint

  def initialize(router:)
    member(router: router, verb: "get", control: Show)
    collection(router: router, verb: "get", control: List)
    collection(router: router, verb: "post", control: Create)
    member(router: router, verb: "puts", control: Update)
    member(router: router, verb: "delete", control: Destroy)
  end
end
```

Here we have a class that inherits some beneficial API from Shogun, and a constructor for matching verbs to controls. I think this part especially highlights the organism oriented design of shogun. The `Endpoint` object knows how to a specific set of behaviors. It takes a router and starts attaching matcher to that router.