---
title: "Docker For Development (Rails Edition)"
draft: false
---

On-boarding, the process of bringing a new worker into the fold, is often the hard part in starting at a company. Usually you are sat in front of a company computer and given a wiki link, GitHub repository, or maybe just an email with the steps for setting up your machine. If you’re lucky it’s been updated in the last year. We have the tools to do this better now and I want to explore them with you.

The first time I remember an on-boarding experience as a developer was at a well-to-do contracting firm. It wasn’t my first or second software position, but it was definitely the one where I discovered on-boarding as a singular concept. It was a painful experience, but not unique to that company.

This time I’m the new developer. I don’t think I was given a wiki or email list, but that’s probably because I didn’t give them a chance. I had one month to move and as they weren’t a remote company it was unlikely I’d be able to contribute to something the team was doing. I’ve never been one to sit on my hands however and so I talked my co-workers into letting me do a non-deadlined experiment.

I had been recently playing a lot with Docker for local development, but I hadn’t perfected it. In fact my main points of contention seemed to be things that wouldn’t be fixed for a few years. See, Docker is in a unique and precarious situation: Because it was "born" in the modern era of software, where code meets social networks, it has the dubious trait of being seen as both standard and immature. You can read at least one-hundred blog posts a week about Docker and yet the next day you’ll find something entirely new.

One good example of the edge in which Docker resides is the local development story. The idea is simple: Your application requires a certain environment to function. Not every development machine is fully equipped for that environment. You either cut out those development machines or you use virtualization. Using virtualization allows the developer to be machine agnostic which is a boon any company should recognize.

**Learning Pains**

Every technology has growing pains and Docker is not an exception, however there is a difference. Docker seems to have this community where things are prototyped under a non-normal name (boot2docker) and then when the community realizes the importance of a tool it consumes it under a new name (docker-machine). There are a few things I’ve learned throughout the whole process:

  0. boot2docker-cli (the thing you install from brew install boot2docker) is deprecated in favor of docker-machine.
  0. Don’t install anything from brew anymore, because now we’ve got Docker Toolbox.
  0. Grab a drink and some food you’re going to be here all night.

So knowing those pre-tips I’ll get into each of the tools and what they encompass. You can give up on learning the internals of all these tools, because there’s way too much.

**docker-machine**

Presumably you’ve got a computer right now and as long as it’s a Windows or Mac you’ll have access to docker-machine. The docker-machine command line is hilariously robust. Hilarious because it’s such a new tool. It will manage, mutate, and communicate with your virtual machines (locally or remotely).

The express purpose of this command in this context is to create the development intermediary. You’ll do this by running:


``` shell
$ docker-machine create -d virtualbox difference-engineers
```

For us this creates a specifically named virtual machine on top of virtualbox. If you look at the --help documentation for the command you’ll see a whole lot of options we’re ignoring. It creates this virtual machine with the highly acclaimed boot2docker-iso, an incredibly lightweight and efficient intermediary.

The next steps will seem very familiar to anyone who used boot2docker-cli:

``` shell
$ eval "$(docker-machine env difference-engineers)"
```

This exports the environment variables needed for docker, docker-compose, and other tools to talk to the right VM and containers. The main difference between boot2docker-cli and this is that it’s specific to the virtual machine intermediary. If you have multiple Docker-based development projects you’re going to need to figure out how to run that script on a per project basis. These set environment variables allow you to talk to the created machine.

I walked you through the basics of using docker-machine above, but that only gets you to the house. Now we have to show you the (metaphorical) paintbrush and ladder so your team can define the perfect environment.

**docker-compose**

There are two ways to use Docker:

  0. An isolated ecosystem for your program sitting inside a container.
  0. A series of ecosystems that are interconnected, with a service per container.

I think the future and mature way to use Docker is the latter. If like me you agree then you’re going to need docker-compose. Not only is it a tool for managing the web of containers it’s also the gateway for the docker command. Anything the docker command can do so also can the docker-compose, but with one small caveat: Most of the time you have to specify the container name. For example, these two commands are equivalent:

``` shell
$ docker run /bin/bash
$ docker run web bin/bash
```

Where did the container name web come from? Well it’s defined in our `docker-compose.yml` configuration file:

``` yml
web:
  command: bin/rails server --port=$PORT --binding=$binding
  volumes:
    - /usr/src/application:/usr/src/application
  build: .
  env_file: .env.web
  ports:
    - "3000:3000"
  links:
    - postgres
    - memcached
postgres:
  image: postgres:9.4
  ports:
    - 5432
memcached:
  image: memcached:1.4
  ports:
    - 11211
```

Let’s break this beast down into smaller parts:

**The Container Definition**

``` yml
web:
  # ...
postgres:
  # ...
memcached:
  # ...
```

The root keys of this document are all container names. Here I’ve defined three specific containers that will be set as environment variables:

``` shell
APPLICATION_MEMCACHED_1_PORT=tcp://172.17.0.2:11211
MEMCACHED_PORT=tcp://172.17.0.2:11211
MEMCACHED_1_PORT=tcp://172.17.0.2:11211
APPLICATION_POSTGRES_1_PORT=tcp://172.17.0.3:5432
POSTGRES_PORT=tcp://172.17.0.3:5432
POSTGRES_1_PORT=tcp://172.17.0.3:5432
```


There are going to be a ton more of these, but there are the important values. You’ll notice there seem to be duplicates. This is due to the nature of containers. You might want to scale up your PostgreSQL containers so you have 5 at the same time. The way you would programmatically differentiate between them is via these values.

Further you’ll find the `/etc/hosts` file has been mutated and they include some great shortcuts:

``` terminal
$ cat /etc/hosts
172.17.0.5  6896242e97ed
127.0.0.1  localhost
::1  localhost ip6-localhost ip6-loopback
fe00::0  ip6-localnet
ff00::0  ip6-mcastprefix
ff02::1  ip6-allnodes
ff02::2  ip6-allrouters
172.17.0.3  postgres 0db19fc810d1 application_postgres_1
172.17.0.3  postgres_1 0db19fc810d1 application_postgres_1
172.17.0.2  application_memcached_1 1bac9dfc3096
172.17.0.3  application_postgres_1 0db19fc810d1
172.17.0.2  memcached 1bac9dfc3096 application_memcached_1
172.17.0.2  memcached_1 1bac9dfc3096 application_memcached_1
172.17.0.3  application_postgres_1
172.17.0.3  application_postgres_1.bridge
172.17.0.5  application_web_run_3
172.17.0.5  application_web_run_3.bridge
172.17.0.2  application_memcached_1
172.17.0.2  application_memcached_1.bridge
```

This allows you to make very simplistic connection definitions:

``` yml
default: &default
  adapter: postgresql
  encoding: unicode
  pool: 5
  username: postgres
  host: postgres
```

The other important key here is the volume: key, as it describes the mounting of the intermediary machine (or host) to the guest machine (the container):

``` yml
volumes:
  - /usr/src/application:/usr/src/application
```

**docker**

docker is the tool for manipulating the containers. It gives you programatic access to containers, instructions for building those containers, and details on how the outside world communicates with those containers. All of this is designed in a `Dockerfile` file. You can see ours here:

``` dockerfile
FROM debian:jessie

ENV DEBIAN_FRONTEND noninteractive
ENV SOURCE "/usr/src/..."
WORKDIR $SOURCE

# Installing build dependencies
RUN echo "Installing build dependencies" \
  # Updating the apt-get index
  && apt-get update \
  # Grabbing the core libraries
  && apt-get install -y --no-install-recommends \
    git \
    libmagickcore-dev \
    libmagickwand-dev \
    libpng-dev \
    libpq-dev \
    postgresql-client-9.4 \
    libqt5webkit5-dev \
    qt5-default \
  # Cleaning up apt lists cache
  && rm -rf /var/lib/apt/lists/*

# CRuby Setup
RUN echo "Installing CRuby" \
  && apt-get update \
  && apt-get install -y --no-install-recommends ruby2.1

# Node.js Setup
RUN echo "Installing Node.js" \
  && apt-get update \
  && apt-get install -y --no-install-recommends nodejs
```

There are four parts to this file that are important so we’ll quickly go through that detail now:

**The FROM Instruction**

```
FROM debian:jessie
```

This is the base image it builds from. Many companies will want to pick official pre-designed images like `ruby:2.2` or `node:3.0`. You might be tempted to use the latest value for the version, but this is a mind killer. An external source mutates an index and suddenly you’re building from scratch. We personally chose to go with a bare bones approach as we wanted to get some insight into the process. The only flaw with `FROM` in my opinion is that you can’t compound multiple to form a chain of composition.

**The ENV Instruction**

``` dockerfile
ENV DEBIAN_FRONTEND noninteractive
ENV SOURCE "/usr/src/..."
WORKDIR $SOURCE
```

Here you’ll define some environment variables for the build process. As discussed in the docker-compose section you’ll actually want to define application specific environment variables using the env-file keyword, but those values aren’t present until docker build is finished. We’ve included these to have tighter control over our build process. The `WORKDIR` isn’t an environment variable, but it’s basically the same idea just for docker’s target directory purposes.

**The RUN Instruction**

``` dockerfile
# Installing build dependencies
RUN echo "Installing build dependencies" ...

# CRuby Setup
RUN echo "Installing CRuby" ...

# Node.js Setup
RUN echo "Installing Node.js" ...
```

This is the meat of the `Dockerfile` and where most of your pre-application directory setup should go. In most companies you’ll probably see a lot to do with bundle install or similar. Due to the constraints of `VOLUME` vs `-v/--volume` (they aren’t the same thing) we’ve opted to only do pre-sync operations here. You can learn about `RUN` in other tutorials but here’s what I’ll suggest:

  0. Keep like things together.
  0. Make sure to clean up after yourself.
  0. Don’t be afraid to compile from source.

This is the last part of the easy stuff, now we get into the advanced moving parts. A lot of this might disappear in a few years, the docker team works fast, but for now it's something I'm dealing with.

**docker-rsync**

Most developers want to mirror their source code to the guest container. They’d like to be able to edit and commit on the host while running and testing on the guest. There are three primary ways to do this and none of them are particularly pleasant.

**The COPY Instruction**

You may have noticed a COPY operation that seems to take a SOURCE (host) and copy it to a DESTINATION (guest). This seems ideal up until you actually mutate a file on the host and you notice no change is present in the guest’s source. This is because COPY is a one time operation and in fact is a build cache busting operation. If you want the changes you’ve made to your host source you will need to rebuild from that line. This is terrible and really should be avoided.

People also use COPY for pre-installing dependencies like this:

``` dockerfile
COPY Gemfile /usr/src/application/Gemfile

COPY Gemfile.lock /usr/src/application/Gemfile.lock

RUN bundle install
```

This is great because it caches the dependencies in the build, but terrible because it fails hard for things like npm where you have two options:

  0. Global installation, breaking `require()`
  0. Local installation, which gets wiped when you finally do use `VOLUME`/`volumes:`/`-v`

Basically a show stopper for any tool that generates filesystem data local to the source (npm, bower, jspm, component, vulcanize, ...).

**The VOLUME instruction or -v/--volume flag**

By now you’ve already seen this as it’s the next suggestion from many people doing docker-based development. By now you’ve already been hit by the TERRIBLE lag when doing a file request. Not only is `VOLUME` different from volumes:, `-v`, `--volume` internally, but it was never intended as a sync. On a moderately sized Rails application with 100~ assets (uncompressed because of development mode) a single uncomplicated request can take 8-12 seconds. If you, like me, investigate this further it’s noticeable by the 600ms Time-To-First-Byte latency. Think about the journey that file request takes from the browser to your host machine’s filesystem.

Really the issue is vboxfs a notoriously slow read layer, but fast for writes. Like me you’ll look for alternatives and come across NFS, but quickly find that the problem has shifted to slow writes and fast reads. Either way an elongated development process and very painful cycle.

Having gone through the same issues I discovered docker-rsync. docker-rsync is an extra layer not yet a part of the official toolkit, but definitely a valuable solution. It’s job is simple: It will (one-way) rsync your files to the intermediary machine. The volumes:, -v, --volume pick up the changes we’ve made and copy them to the host. That means we get fast reads and fast writes. Here’s how we use rsync:

```
$ docker-rsync -watch=false -dst="/usr/src/" difference-engineers
```

Note: This might change in the future, as we noticed an issue with paths.

This salvation isn’t without problems since as described it’s only one-way. Rails developers will have an interesting situation where (due to some hosting circumstances) they have to commit a generated file like `Gemfile.lock`. This means that (for one single file) you need a way bring back the generated content if your plan is to run entirely on docker’s development environment. A few of my friends and I have come up with some absurd ideas for solving this (the latest of which is to use cron, cp, and an "exploit").

Note: This information might be helpful, but mostly it was just me flexing my Google Draw skills.

It’s hard to explain so I’ve made some helper images:

First we have the host machine:

Phase 1

Then we have the intermediary boot2docker virtual machine on virtualbox (or whatever you use):

Phase 2

Now by default docker-machine vboxfs mounts the source to the boot2docker virtual machine:

Phase 3

Finally we have the guest container created and the volume shared directory on all 3 environments:

Phase 4a

However, as stated previously, this creates some issues so lets back up a step.

We’ll use docker-rsync to sync the files one way to the intermediary machine:

Phase 4b

Now we just have to periodically cp certain files from the rsync directory to the two-way sync’ed mount (seen later):

Phase 5b

Finally we volume mount from the intermediary machine to the guest machine:

Phase 6b

Now we have a fully working, fast, and cross-platform development environment that requires zero mutating of your computer, junks version managers (rvm, rbenv, nvm, etc), and is always isolated.

**Concerns & The Future**

I like docker and I also like what I’m dubbing docker-development. There are a lot of drawbacks to this situation:

  0. It gets significantly more complex if you need back files
  0. It’s a large system, things can break easily
  0. It requires a tool that isn’t a part of the official ecosystem
  0. The cogs need serious uniformity
  0. I don’t think docker-rsync is windows compatible

It’s very apparent that while docker is another great contender for devops it’s also a huge asset for local development and teaching. Most of my issues will blow over after a time. My company had managed to turn it’s entire development process into a single button and that button is only 5 shell script lines long.

Here’s what I’d like to see out of the future:

``` dockerfile
AUTHOR "Kurtis Rainbolt-Greene <kurtis@rainbolt-greene.online>"

MUTATE docker/machine
MUTATE docker/compose
MUTATE docker/sync

ISO boot2docker
MACHINE difference-engineers

CONTAINER web
FROM ubuntu:latest

APPLY ruby:latest
APPLY nodejs:latest
APPLY postgres-client:latest

LINK postgres:database
LINK memcached:cache

ENVFILE .env.web

RUN apt-get update && apt-get install -y imagemagik

SYNC .:/usr/src/application

RUN bundle install
RUN npm install
RUN bower install

EXPOSE 3000

CMD ["bin/rails", "server"]

CONTAINER postgres:latest

CONTAINER memcached:latest
```

If it’s not terribly clear the `MUTATE` instructions allow third parties to add new instructions. This opens the door for `SYNC`, `ENVFILE`, `CONTAINER`, `LINK`, and `APPLY` (which is just `FROM` but ignoring the `APPLY`’d `FROM`).

**NOTE**

The cron job described above to bring back generated assets looks like this:

``` shell
docker-machine ssh "(crontab -l ; echo "0 * * * * cp -f /rsync/usr/src/application/Gemfile.lock ${pwd}") | sort - | uniq - | crontab -"
```

Enjoy!
