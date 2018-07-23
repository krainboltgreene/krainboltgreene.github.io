---
title: "How I learned a little about databases"
draft: false
---

I've been programming about three years now with most of my work done in web development. A serious part of web development work is with databases. You store user information, statistics, scraped data, and anything else that's important enough to be persisted in a database. It's an important and yet often marginalized side of modern web development.

My first real language was [Ruby][RUBY] and so far it remains the steadfast tool that I use when I need to build a solution to a problem. My good friend [@bhuga][BHUGA] once lamented how envious he was that I got to start with Ruby. I think in reality of it is that by starting with Ruby I've avoided learning all the effort it took to do things with older programming languages. In this way my start with databases is similar, because my first big boy database was MongoDB.

[MongoDB][MONGODB] is now not exceptionally unique with other contestants like [Riak][RIAK] becoming rapidly popular in the [node.js][NODEJS] community, but 2 years ago it was amazing to me. I had only started build web applications, using Sinatra and Sequel at first. Sequel's default documentation suggested [SQLite][SQLITE] as a starter backend, but I was too ignorant to understand that SQLite was also a database. I certainly didn't understand relationships or what they actually entailed.

I was pushed very early on to learn how to write [Rails][RAILS] applications mainly because I was very poor and people pay a lot of money for Rails applications. Going from [Sinatra][SINATRA] to Rails was a hard transition for my junior developer skilled self, but even harder was attempting to use things like MySQL or setting up [PostgreSQL][POSTGRESQL]. I still remember the frustrating error messages that (now that I understand) probably meant I didn't have a postgres role created with the right powers. MongoDB was as fresh as the air I was inhaling in the middle of Oregon.

At the time I was still using an [Ubuntu][UBUNTU] laptop. I had managed to teach myself how to use [xmonad][XMONAD] oddly enough, so I wasn't a neophyte to reading in-progress documentation. MongoDB was a brilliantly easy setup process, devoid of user permission systems, passwords, database seeding, [ncurser][NCURSER] GUI systems, and all sorts of other things I had to do just to get a simple SQL server running locally. I'll enumerate the steps below:

  1. `apt-get install mongodb`

I apologize for the longevity of that process, but you know how these things can be! All kidding aside that was exactly the process required to start storing username, email, password, and getting it back again. Thats all my applications really did at this point anyways, because I was still learning things like what actions a resources :articles clause created for the controller. I should mention at this point the brilliant simplicity that is [Mongoid][MONGOID] and how it absolutely helped me start loving MongoDB for all the wrong reasons.

Mongoid afforded me so many luxuries that I still wish I had access even today. First and foremost being schema-less. Not having to conform to a specific database structure and thus not having to rebuild the entire thing every time I want to add a column is diabolically maddening. If I could keep and only keep one thing from Mongoid and transplant it into [ActiveRecord][ACTIVERECORD] it would be having the schema in the models instead of in a migration (or five) and a schema.rb

Mongoid is a collision of good documentation, simple interfaces, and good architecture. I didn't have to know about join tables, eager loading, database constraints, locking mechanism, or any other advanced jib that I now know. Mongoid let me as a young and stupid developer work on the concepts my models tried to encompass without having to worry about schemas or migrations. Here's a simple `Mongoid` class:

```ruby
class Person
  include Mongoid::Document
  include Mongoid::Timestamps

  field :name, default: "James T. Kirk"
  field :aliases, type: Array
  field :age, type: Integer, index: true

  embeds_many :honors

  has_many :shipmates
end
```

I didn't have to run `rake db:create` or `rake db:migrate`. I just opened up my rails console and typed `Person.create name: “Kurtis Rainbolt-Greene”` and suddenly I had a piece of persistent data I could find if I needed too. Mongoid was so simplistic that my concerns were allowed to roam elsewhere in the application. Mr. Lavender lamented about this as well; suggesting that perhaps I should learn how my database works before I gallivant off into the night building web applications. He was right, there would be pain in my future.

While I was building my toy applications with Mongoid and MongoDB I had a naive idea of how a real application in real world circumstances performs. In my mind the 1-2 seconds it took to gather all 12 blog posts from an account was fine. How fast did a web application really need to be anyhow? I championed MongoDB to pretty much all the developers I knew and oddly enough they only laughed at me a little bit.

The second contract job I ever took in New Orleans was building an API for an iPhone application. My immediate first choice was to use this oh so simple database (which was also easy to setup on Heroku) for this application. This was also the first time I had to build something that would have a large amount of data. The database concept itself looked something like this:

  - Teachers had many Students
  - Students had many and belonged to many Parents.
  - Class rooms had many Students
  - Teachers had and belonged to many Classes

First of all before we get to the meat of the problem here (which I'm sure many of you have already guessed) lets talk about how entirely infuriating it is to not be able to have a model called Class. The next language I invent or use will either forgo classes or call them something else entirely.

For those of you who haven't figured the problem out yet, the problem that arose in this client's application is that MongoDB isn't a relational database. You can mimic relationships in Mongoid, as you can see in the example above, but (much to my chagrin) that doesn't mean much other than a extra database calls per ID in the relational array stored on the mongodb record. Once you have a hundred or a thousand entries you've got serious performance problems.

Any serious MongoDB expert will tell you this up front and groan about what an idiot I was for not understanding MongoDB. They'd be absolutely right on both counts and right to say so, but thats really beside the point: MongoDB was so bloody easy that I could ignore those things for so very long. Long enough to get on my own two feet developing applications. Long enough that when I needed to know what N+1 meant I could finally understand the bad documentation that litters the web.

The teachers out there reading this will know what this kind of value is for students. The sooner they can start making things, the sooner they're done setting up, the faster many people learn. We all start off wanting to make things and at the time for my simplistic needs Mongoid & MongoDB made persistence a save haven where I could ignore all the worlds problems that frankly I didn't need to solve yet.

[RUBY]: https://www.ruby-lang.org/en/
[BHUGA]: http://bhuga.net/
[MONGODB]: http://www.mongodb.org/
[RIAK]: http://basho.com/riak/
[NODEJS]: http://nodejs.org/
[SQLITE]: http://www.sqlite.org/
[RAILS]: http://rubyonrails.org/
[SINATRA]: http://www.sinatrarb.com/
[POSTGRESQL]: http://www.postgresql.org/
[UBUNTU]: http://www.ubuntu.com/
[XMONAD]: http://xmonad.org/
[NCURSER]: http://www.gnu.org/software/ncurses/
[MONGOID]: http://mongoid.org/en/mongoid/index.html
[ACTIVERECORD]: http://api.rubyonrails.org/classes/ActiveRecord/Base.html
