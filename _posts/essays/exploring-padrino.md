---
title: "Exploring Padrino"
---

I've finally decided to test the "Padrino" waters again. I've heard a lot of good things about it lately, and [a recent benchmark blew my fucking mind](https://github.com/DAddYE/web-frameworks-benchmark/wiki/achiu). So here I am, looking at the website, in my terminal, and with an editor open. A word about Padrino first: It's not an MVC framework, despite what the website says and how familiar that architecture feels. It's actually a [PAC](http://en.wikipedia.org/wiki/Presentation-abstraction-control), and it would do you well to remember this when using Padrino.

My first step of course, was to make a directory for my project. Since I didn't know if I'd like Padrino, this time, I figured I might want to try out multiple ideas. This means making a whole `padrino` directory in my `~/repo/ruby/` path. Having done so I then made the project directory (`~/repo/ruby/padrino/cloudname`) so that I'd have a place to put my `.rvmrc`. Specifically this is so I could install Padrino into the cloudname gemset.

Having made the `padrino` directory, the `cloudname` directory, and the `.rvmrc` with `rvm --create --rvmrc 1.9.2@cloudname` (and cleaning up the extra rc file) I went on to install Padrino. To do this I looked at the website instructions which promptly told me to use `gem install padrino`. No sight of anything related to Bundler, which will probably annoy me.
The resulting output is as follows:

```
krainboltgreene on MacMini.local in ~/repo/ruby/padrino/cloudname
$ gem install padrino
Fetching: tilt-1.3.2.gem (100%)
Fetching: rack-1.3.2.gem (100%)
Fetching: sinatra-1.2.6.gem (100%)
Fetching: url_mount-0.2.1.gem (100%)
Fetching: http_router-0.9.7.gem (100%)
Fetching: thor-0.14.6.gem (100%)
Fetching: activesupport-3.0.9.gem (100%)
Fetching: padrino-core-0.10.1.gem (100%)
********************
 UPGRADE NOTES

 When upgrading, please 'enable :sessions' for each application as shown here: http://bit.ly/kODKMx
 When upgrading, please 'register Padrino::Rendering' for each application as shown here: https://gist.github.com/1d36a35794dbbd664ea4
********************

Fetching: i18n-0.5.0.gem (100%)
Fetching: padrino-helpers-0.10.1.gem (100%)
Fetching: mime-types-1.16.gem (100%)
Fetching: polyglot-0.3.2.gem (100%)
Fetching: treetop-1.4.10.gem (100%)
Fetching: mail-2.3.0.gem (100%)
Fetching: padrino-mailer-0.10.1.gem (100%)
Fetching: bundler-1.0.17.gem (100%)
Fetching: diff-lcs-1.1.2.gem (100%)
Fetching: grit-2.4.1.gem (100%)
Fetching: padrino-gen-0.10.1.gem (100%)
Fetching: padrino-cache-0.10.1.gem (100%)
Fetching: padrino-admin-0.10.1.gem (100%)
Fetching: padrino-0.10.1.gem (100%)
Successfully installed tilt-1.3.2
Successfully installed rack-1.3.2
Successfully installed sinatra-1.2.6
Successfully installed url_mount-0.2.1
Successfully installed http_router-0.9.7
Successfully installed thor-0.14.6
Successfully installed activesupport-3.0.9
Successfully installed padrino-core-0.10.1
Successfully installed i18n-0.5.0
Successfully installed padrino-helpers-0.10.1
Successfully installed mime-types-1.16
Successfully installed polyglot-0.3.2
Successfully installed treetop-1.4.10
Successfully installed mail-2.3.0
Successfully installed padrino-mailer-0.10.1
Successfully installed bundler-1.0.17
Successfully installed diff-lcs-1.1.2
Successfully installed grit-2.4.1
Successfully installed padrino-gen-0.10.1
Successfully installed padrino-cache-0.10.1
Successfully installed padrino-admin-0.10.1
Successfully installed padrino-0.10.1
22 gems installed
```

After carefully reading the documentation on the various generators, including the project generator, I decided on using this setup:

```
padrino generate project cloudname --orm mongoid --test riot --script jquery --renderer slim --mock rr  --stylesheet compass
```

Which denotes a mongoid orm, riot test suite, jquery library, slim html renderer, and rr for mocking. It produced this output:

```
krainboltgreene on MacMini.local in ~/repo/ruby/padrino
$ padrino generate project cloudname --orm mongoid --test riot --script jquery --renderer slim --mock rr  --stylesheet compass
      create
      create  .gitignore
      create  config.ru
      create  config/apps.rb
      create  config/boot.rb
      create  public/favicon.ico
      create  public/images
      create  public/javascripts
      create  public/stylesheets
      create  tmp
      create  .components
      create  app
      create  app/app.rb
      create  app/controllers
      create  app/helpers
      create  app/views
      create  app/views/layouts
      create  Gemfile
Applying 'mongoid' (orm)...
       apply  orms/mongoid
      insert  Gemfile
      insert  Gemfile
      create  config/database.rb
Applying 'riot' (test)...
       apply  tests/riot
      insert  Gemfile
      insert  Gemfile
      create  test/test_config.rb
      create  test/test.rake
Applying 'rr' (mock)...
       apply  mocks/rr
      insert  Gemfile
      insert  test/test_config.rb
Applying 'jquery' (script)...
       apply  scripts/jquery
      create  public/javascripts/jquery.js
      create  public/javascripts/jquery-ujs.js
      create  public/javascripts/application.js
Applying 'slim' (renderer)...
       apply  renderers/slim
      insert  Gemfile
Applying 'compass' (stylesheet)...
       apply  stylesheets/compass
      insert  Gemfile
      create  lib/compass_plugin.rb
      insert  app/app.rb
      create  app/stylesheets
      create  app/stylesheets/application.scss
      create  app/stylesheets/partials/_base.scss
   identical  .components

=================================================================
cloudname is ready for development!
=================================================================
$ cd ./cloudname
$ bundle install
=================================================================
```

It should be noted that I attempted to run the command in `~/repo/ruby/padrino/cloudname/` with the -r option set to `../cloudname`. Instead of building the project in my current directory it built it in `~/repo/ruby/padrino/cloudname/cloudname`.

At this time I started my MongoDB instance, on the 127.0.0.1:28017 address/port.
Following the instructions I moved into the cloudname directory and ran `bundle install`:

```
krainboltgreene on MacMini.local in ~/repo/ruby/padrino/cloudname
$ bundle install
Fetching source index for http://rubygems.org/
Installing rake (0.9.2)
Installing activesupport (3.0.9)
Installing builder (2.1.2)
Installing i18n (0.5.0)
Installing activemodel (3.0.9)
Installing bson (1.3.1)
Installing bson_ext (1.3.1) with native extensions
Using bundler (1.0.15)
Installing chunky_png (1.2.1)
Installing fssm (0.2.7)
Installing sass (3.1.7)
Installing compass (0.11.5)
Installing diff-lcs (1.1.2)
Using mime-types (1.16)
Installing grit (2.4.1)
Installing rack (1.3.2)
Installing url_mount (0.2.1)
Installing http_router (0.9.7)
Installing polyglot (0.3.2)
Installing treetop (1.4.10)
Installing mail (2.3.0)
Installing mongo (1.3.1)
Installing tzinfo (0.3.29)
Installing mongoid (2.1.7)
Installing tilt (1.3.2)
Installing sinatra (1.2.6)
Using thor (0.14.6)
Installing padrino-core (0.10.1)
Installing padrino-helpers (0.10.1)
Installing padrino-admin (0.10.1)
Installing padrino-cache (0.10.1)
Installing padrino-gen (0.10.1)
Installing padrino-mailer (0.10.1)
Installing padrino (0.10.1)
Installing rack-flash (0.1.2)
Installing rack-test (0.6.1)
Installing rr (1.0.3)
Installing riot (0.12.4)
Installing temple (0.3.2)
Installing slim (1.0.1)
```

So all of my gems installed just fine, from the auto-generated gemfile. Almost every tutorial and article talks about the Padrino admin interface that ships with Padrino, so I had to try that out first:

```
krainboltgreene on MacMini.local in ~/repo/ruby/padrino/cloudname
$ padrino g admin
       force  .components
      create  admin
       exist  admin
      create  admin/controllers/base.rb
      create  admin/controllers/sessions.rb
      create  public/admin
      create  public/admin/stylesheets/base.css
      create  public/admin/stylesheets/themes/amro/style.css
      create  public/admin/stylesheets/themes/bec-green/style.css
      create  public/admin/stylesheets/themes/bec/style.css
      create  public/admin/stylesheets/themes/blue/style.css
      create  public/admin/stylesheets/themes/default/style.css
      create  public/admin/stylesheets/themes/djime-cerulean/style.css
      create  public/admin/stylesheets/themes/kathleene/style.css
      create  public/admin/stylesheets/themes/olive/style.css
      create  public/admin/stylesheets/themes/orange/style.css
      create  public/admin/stylesheets/themes/reidb-greenish/style.css
      create  public/admin/stylesheets/themes/ruby/style.css
      create  public/admin/stylesheets/themes/warehouse/style.css
      create  admin/app.rb
      append  config/apps.rb
       apply  orms/mongoid
       apply  tests/riot
      create  models/account.rb
      create  test/models/account_test.rb
      create  admin/views/accounts
      create  admin/controllers/accounts.rb
      create  admin/views/accounts/_form.slim
      create  admin/views/accounts/edit.slim
      create  admin/views/accounts/index.slim
      create  admin/views/accounts/new.slim
      insert  admin/app.rb
       force  models/account.rb
      create  db/seeds.rb
       exist  admin/controllers
       exist  admin/views
      create  admin/views/base
      create  admin/views/layouts
      create  admin/views/sessions
      create  admin/views/base/_sidebar.slim
      create  admin/views/base/index.slim
      create  admin/views/layouts/application.slim
      create  admin/views/sessions/new.slim
      insert  admin/app.rb
      insert  Gemfile
        gsub  admin/views/accounts/_form.slim
        gsub  admin/controllers/accounts.rb

=================================================================
The admin panel has been mounted! Next, follow these steps:
=================================================================
  1) Run 'padrino rake seed'
  2) Visit the admin panel in the browser at '/admin'
=================================================================
```

I preceded to follow the instructions and ran into my second problem with Padrino:

```
krainboltgreene on MacMini.local in ~/repo/ruby/padrino/cloudname
$ padrino rake seed
Could not find gem 'bcrypt-ruby (>= 0)' in any of the gem sources listed in your Gemfile.
Run `bundle install` to install missing gems.
```

First thing I do is check for `bcrypt-ruby`:

```
krainboltgreene on MacMini.local in ~/repo/ruby/padrino/cloudname
$ gems | grep bcrypt
```

With zero results returned, as you can see. I've dealt with these kinds of problems before in Rails, though so I thought to follow the error messages instructions first. Suddenly, everything is working:

```
krainboltgreene on MacMini.local in ~/repo/ruby/padrino/cloudname
$ bundle install | grep bcrypt
Installing bcrypt-ruby (2.1.4) with native extensions
krainboltgreene on MacMini.local in ~/repo/ruby/padrino/cloudname
$ gems | grep bcrypt
bcrypt-ruby (2.1.4)
```

So it's time to get back to the original task, of seeding the database:

```
krainboltgreene on MacMini.local in ~/repo/ruby/padrino/cloudname
$ padrino rake seed
=> Executing Rake seed ...
Which email do you want use for logging into admin? kurtis@rainbolt-greene.online
Tell me the password to use: ***

Sorry but some thing went wrong!

   - Password is too short (minimum is 4 characters)
```

What an interesting twist, having a command line client to handle the admin account. Though I'm not really liking the 4 minimum, I can understand it. Lets try this again:

```
krainboltgreene on MacMini.local in ~/repo/ruby/padrino/cloudname
$ padrino rake seed
=> Executing Rake seed ...
Which email do you want use for logging into admin? kurtis@rainbolt-greene.online
Tell me the password to use: hqw7KsJ3cgfu9KsV8rP66xrcN

=================================================================
Account has been successfully created, now you can login with:
=================================================================
   email: kurtis@rainbolt-greene.online
   password: hqw7KsJ3cgfu9KsV8rP66xrcN
=================================================================
```

Much better! The second listed task is to visit the `/admin` path on the app. Of course that means I need to start Padrino, which means setting up Pow (hopefully)! To do this I use my ever faithful `powder` gem, with the command:

```
krainboltgreene on MacMini.local in ~/repo/ruby/padrino/cloudname
$ powder
Your application is now available at http://cloudname.dev/
krainboltgreene on Kurtis-Rainbolt-Greenes-Mac-mini.local in ~/repo/ruby/padrino/cloudname
$ powder open
# This is where the browser opens up to http://cloudname.dev/
```

Since I don't have a "proper" Padrino application it amusingly shows me the default Sinatra error page:

<img src="http://i.imgur.com/xiHAR.png" alt="" title="Hosted by imgur.com" width="640px"/>

The example in the middle of the page doesn't really make sense from Padrino's standpoint, as far as I know. While being built on Sinatra, you can't just declare a route anywhere. I was on a mission, though, and I wanted to check out this admin interface. After entering the `/admin` path I was served this login page:

<img src="http://i.imgur.com/pTJ7t.png" alt="" title="Hosted by imgur.com" width="640px"/>

The first thing that hit me was that "Bypass Login?" checkbox. I mean I know it's an admin interface, but was "Remember me?" too standard? In any case logging in was a breeze, but the admin interface wasn't:

<img src="http://i.imgur.com/r8ReH.png" alt="" title="Hosted by imgur.com" width="640px"/>

Lots of Ipsum Lorem, as you can see in that image. Nothing really stands out as important except random highlights. Nothing really seems like a set of controls, either. I could be looking at any old wordpress template.

Going to the `/admin/accounts/edit/:id` path showed me my admin account, with a few fields. Classic splitting of First Name and Last Name, where there doesn't need to be:

<img src="http://i.imgur.com/V73B2.png" alt="" title="Hosted by imgur.com" width="640px"/>

Having figured out that Padrino works fine so far I decided to finally initiate a git instance. I could probably skip this part, but some of you probably haven't used Git Flow and if I can introduce you to it, all the better. So lets get this party started:

```
krainboltgreene on Kurtis-Rainbolt-Greenes-Mac-mini.local in ~/repo/ruby/padrino/cloudname
$ git init
Initialized empty Git repository in /Users/krainboltgreene/repo/ruby/padrino/cloudname/.git/
krainboltgreene on Kurtis-Rainbolt-Greenes-Mac-mini.local in ~/repo/ruby/padrino/cloudname(cloudname|~|master*)
$ git add .
krainboltgreene on Kurtis-Rainbolt-Greenes-Mac-mini.local in ~/repo/ruby/padrino/cloudname(cloudname|~|master*)
$ git commit -asm "Initial commit."
[master (root-commit) e51de90] Initial commit.
 48 files changed, 5278 insertions(+), 0 deletions(-)
 create mode 100644 .components
 create mode 100644 .gitignore
 create mode 100644 .rvmrc
 create mode 100644 Gemfile
 create mode 100644 Gemfile.lock
 create mode 100644 README
 create mode 100644 admin/app.rb
 create mode 100644 admin/controllers/accounts.rb
 create mode 100644 admin/controllers/base.rb
 create mode 100644 admin/controllers/sessions.rb
 create mode 100644 admin/views/accounts/_form.slim
 create mode 100644 admin/views/accounts/edit.slim
 create mode 100644 admin/views/accounts/index.slim
 create mode 100644 admin/views/accounts/new.slim
 create mode 100644 admin/views/base/_sidebar.slim
 create mode 100644 admin/views/base/index.slim
 create mode 100644 admin/views/layouts/application.slim
 create mode 100644 admin/views/sessions/new.slim
 create mode 100644 app/app.rb
 create mode 100644 app/stylesheets/application.scss
 create mode 100644 app/stylesheets/partials/_base.scss
 create mode 100644 config.ru
 create mode 100644 config/apps.rb
 create mode 100644 config/boot.rb
 create mode 100644 config/database.rb
 create mode 100644 db/seeds.rb
 create mode 100644 lib/compass_plugin.rb
 create mode 100644 models/account.rb
 create mode 100644 public/admin/stylesheets/base.css
 create mode 100644 public/admin/stylesheets/themes/amro/style.css
 create mode 100644 public/admin/stylesheets/themes/bec-green/style.css
 create mode 100644 public/admin/stylesheets/themes/bec/style.css
 create mode 100644 public/admin/stylesheets/themes/blue/style.css
 create mode 100644 public/admin/stylesheets/themes/default/style.css
 create mode 100644 public/admin/stylesheets/themes/djime-cerulean/style.css
 create mode 100644 public/admin/stylesheets/themes/kathleene/style.css
 create mode 100644 public/admin/stylesheets/themes/olive/style.css
 create mode 100644 public/admin/stylesheets/themes/orange/style.css
 create mode 100644 public/admin/stylesheets/themes/reidb-greenish/style.css
 create mode 100644 public/admin/stylesheets/themes/ruby/style.css
 create mode 100644 public/admin/stylesheets/themes/warehouse/style.css
 create mode 100644 public/favicon.ico
 create mode 100644 public/javascripts/application.js
 create mode 100644 public/javascripts/jquery-ujs.js
 create mode 100644 public/javascripts/jquery.js
 create mode 100644 public/stylesheets/application.css
 create mode 100644 test/models/account_test.rb
 create mode 100644 test/test.rake
 create mode 100644 test/test_config.rb
krainboltgreene on Kurtis-Rainbolt-Greenes-Mac-mini.local in ~/repo/ruby/padrino/cloudname(cloudname|0m|master)
$ git flow init

Which branch should be used for bringing forth production releases?
   - master
Branch name for production releases: [master]
Branch name for "next release" development: [develop]

How to name your supporting branch prefixes?
Feature branches? [feature/]
Release branches? [release/]
Hotfix branches? [hotfix/]
Support branches? [support/]
Version tag prefix? [] ver
krainboltgreene on Kurtis-Rainbolt-Greenes-Mac-mini.local in ~/repo/ruby/padrino/cloudname(cloudname|0m|develop)
$ git flow feature start accounts-scaffolding
Switched to a new branch 'feature/accounts-scaffolding'

Summary of actions:
- A new branch 'feature/accounts-scaffolding' was created, based on 'develop'
- You are now on branch 'feature/accounts-scaffolding'

Now, start committing on your feature. When done, use:

     git flow feature finish accounts-scaffolding

krainboltgreene on Kurtis-Rainbolt-Greenes-Mac-mini.local in ~/repo/ruby/padrino/cloudname(cloudname|0m|feature/accounts-scaffolding)
$
```

So lets talk about this application before we go further. BrowserID is a new technology that implements Verified Email Protocol. You can read more at the website [here](https://browserid.org/). CloudName is going to be a Ruby version of that server, so we'll need at the very least accounts for users to sign in with. Accounts happens to need all three of the core PAC parts: Presentation (Commonly called a View), Abstraction (Sometimes called a Model), and Controller (You probably know this one). This'll let us explore Padrino completely, from start to finish.

Since this application is heavily focused on "Authentication" the top level application will be where the account resource resides. Padrino has already created two applications so far:  `app/` and `admin/`. So we'll need to dig into `app/` and create the Account resource.
