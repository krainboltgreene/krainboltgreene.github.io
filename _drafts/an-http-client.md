---
title: "Your First Ruby SDK"
---

Along with the rise of [HATEOS API standards](http://jsonapi.com) I've noticed an
equal rise in the number of API clients I'm shipping. Recently I had to build an internal access HTTP API client and I got to see what developers are like as users. This log will details both my methods and opinions on good gem design and client design. Sometimes I'll sneak in an opinion on web api standards, but defer to the professionals before me.

To those of you who have built a gem before I suggest just skipping the first section of this blog. I basically provide a home grown gem scaffold that has all the tools I use setup. You can see it [here](https://github.com/krainboltgreene/blankgem.gem) and probably figure out what to do (or use your own system).


**Getting Started** An HTTP client is essentially a program that knows about a service (the API) and it's resources. As we know libraries (or gems, in Ruby) are a package of code that serve a common purpose. What I'm saying is that you should make your clients into libraries in order to avoid tightly coupled integration and really re-use your work to save lots of time and future pain.

I've found that there are a few important things I want in all of my gems. These tools and patterns speed up the time it takes me to get to the first test and usage of the library. I've codified this into [a repository that I clone from](https://github.com/krainboltgreene/blankgem.gem) for every new gem. Just follow these steps to get working:

``` shell
$ git clone git@github.com:krainboltgreene/blankgem.gem.git endorsemint-client.gem
$ cd endorsemint-client.gem
$ git remote rename origin upstream
$ git remote add origin [uri_for_your_repo]
$ git branch -u origin/master
```

The `upstream` remote lets you rebase if I push any changes to my blankgem.gem repository. A good way to keep up with latest changes across the board.

To get started you'll want to run the script `bin/format client` This should leave you with a fresh repository. To start using the tools just `bundle install` and then go ahead and run `[bundle exec] rake` which shows your current test suite. It'll be empty, for now.


**Phone Home** The overarching goal of a client is to make a request to a server. Specifically we'll be talking via HTTP over a TCP connection. While I've used most of the libraries out there my main experience lies with [http](https://github.com/tarcieri/http) so we'll be using that for this client.

First we'll need to add the gem to our gemspec (for those of you playing with the blankgem fork the file is called `endorsemint-client.gemspec`). We can get the latest version number from [rubygems][http://rubygems.org/gems/http] and we'll want to add it to the runtime dependency list:

``` text
 endorsemint-client.gemspec | 1 +
 1 file changed, 1 insertion(+)
```

``` diff
--- endorsemint-client.gemspec
+++ endorsemint-client.gemspec
@@ -20,6 +20,7 @@ Gem::Specification.new do |spec|
   spec.test_files    = Dir["test/**/*", "spec/**/*"]
   spec.require_paths = ["lib"]

+  spec.add_development_dependency "http", "~> 0.6"
   spec.add_development_dependency "bundler", "~> 1.3"
   spec.add_development_dependency "rspec", "~> 3.0"
   spec.add_development_dependency "rake", "~> 10.1"
```

We'll add it to the root file like so:

``` text
 lib/endorsemint/client.rb | 2 ++
 1 file changed, 2 insertions(+)
```

``` diff
--- a/lib/endorsemint/client.rb
+++ b/lib/endorsemint/client.rb
@@ -1,3 +1,5 @@
+require "http"
+
 module Endorsemint
   class Client
     require_relative "endorsemint/version"
```

That does it for setup.


**RRR** Based on my experience I've found that remote client libraries are best split into three pieces (one public, two private):

  - **Request**: The piece that knows how to access the service, normalizes the data, and finally serializes it to an accepted format.
  - **Response**: The piece that knows how to handle the response, denormalizes the data, and finally deserializes based on the content format.
  - **Representation**: The final piece that the end user will interface with in order to operate on the resources.

Generally the `Representation` (sometimes called `Model` or `FacadePattern`) knows how to craft `Request` objects and is constructed from `Response` data. Any extra steps in any process orbits these concepts.

We start off by describing how the `Request` will look for the Endorsemint `Account` endpoint in a test:

``` text

```

``` diff

```


Like with every `describe` block we'll want a value that represents an instance of this class:



Now we can easily reference `request` for our tests. We'll continue by checking to see if `Card`, when told to make a call to the server, returns a response object:



While running `[bundle exec] rake spec` you should see some `uninitialized constant` exceptions raised. To fix these we'll start defining those constants:



Now we run our tests (`[bundle exec] rake spec`) and get a more helpful response:



This returns the familiar `uninitialized constant` exception for `HearthAPI::Response::Card`. We'll define the two new constants like so:



Now we just need to make sure that our `Response` object can turn into a `Model` (Yeah, I know but it's shorter than `Representation` and a lot less spelling-error prone):



I like the word `cast` here because it evokes the concept of casting a mold and I think that fits within the context of a `Model`. After running `[bundle exec] rake spec` you should see a helpful hint on our next step:



Finally we'll need to define the `Model` namespace and it's first inhabitants, the `Model::Card`:



When we're molding this model we want it to behave like a representation of the data. This means taking on some specific traits and behaviors:



Our spec error should look like this:



This satisfies the requirement of having the `Card#name` method, but running the tests again shows us another missing piece:



So now we'll have to generate the initial card data:



And now we can see the last piece required by our implementation:




**Making Sure Things Work** Normally I take the Disappointed Developer approach to testing my work, that is the first time a piece of code doesn't work as expected I write a test for that piece. I feel like this is an appropriate amount of coverage since I try my best to make things simple and testing can be boring. Either way you'll want to use some special tools for this kind of library, and those are:

  - [webmock](https://github.com/bblimke/webmock)
  - [vcr](https://github.com/vcr/vcr)

Both of these libraries have really simple setups and integration with most testing frameworks. They make pretending to be an API endpoint really easy.


---

**TODO**:

  * splitting your gem into the three parts:
    * request
    * response
    * representation
    * layering on a DSL
  * making your classes accessible and DI
  * how to write tests for http apis
