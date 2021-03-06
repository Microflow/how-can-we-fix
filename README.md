<h1 align="center"> How Can We Fix?</h1>

<p align="center">
<img src="http://i.imgur.com/MzThVqS.jpg" width="53%" height="auto">
</p>


### *How Can We Fix is an open source project written in Elixir/Phoenix. A crowdwriting website aimed at solving problems in work or society. Fix is a fork of the awesome [Alchemy Book](https://github.com/rudi-c/alchemy-book)* 

Check out the example Alchemy app at **alchemy.digitalfreepen.com**. We'll make our own version as soon as possible. 

## How can we fix will help users answer questions like these...

- What can we do to fix pollution in our local area?
- How can I raise money for an app?
- How can I open an organic restaurant in my area?
- How can I meditate better?
- I just broke up with my girlfriend, how can I heal my broken heart?
- How can I slim down and get a stronger body?
- How can I be more productive?

Someone asks a question. Users and teams compete to write a well written document. Each doc has one master editor. Alchemy book has great functionality but the CSS needs to me more simple. 

## Want some Elixir practice? Please take a look at our [issues](https://github.com/Microflow/how-can-we-fix/issues)

### [Main list of issues](https://github.com/Microflow/how-can-we-fix/issues)

### [bugs here](https://github.com/Microflow/how-can-we-fix/labels/bug)

<!-- ### [New features here](https://github.com/Microflow/how-can-we-fix/labels/New%20feature%21)-->

## Latest issues!

### [mix ecto.create returns: == Compilation error in file web/views/document_view.ex == ](https://github.com/Microflow/how-can-we-fix/issues/1)

### You can find us in the beginners room at [Elixir-lang Slack](https://elixir-lang.slack.com/messages/C0HEX82NR/) 

## User Stories

## Readers

John logs on to the main How Can We fix page and sees two teams writing about how to improve the local economy.
Matt opens How Can We fix and sees two teams writing a guide on how to raise money locally.
Luke opens Fix and looks at people writing about how to open a local restaurant.

## Writers

Any editor can start a document and let people join their team. 

## Local Forks

How Can We Fix is open source. You're welcome to make a fork for your local area. 

## Enterprise Forks

Fix can easily be used by companies to write internal documents. 

## Roadmap

Right now we're working on better fonts, better background, user logon, better colors, EDITOR prilileges, and a main page with two team panes. 

- Change font to Raleway or Montserrat. Later, give users a choice of more fonts. 
- Change background photo to something simpler, or simple white.
- User configurable username.
- EDITOR function. One user gets edit priviliges, and final say over the whole document. 
- **See user colors** function. CLick, and see who wrote what.
- Main page where you can see two teams writing in separate windows. 

We make problem solving fun, powerful and free!

<p align="center">
<img src="http://i.imgur.com/6Gytiim.jpg" width="55%" height="auto">
</p>


## Original Alchemy Book ReadMe Below.

<!--![Alchemy Book](https://raw.githubusercontent.com/rudi-c/alchemy-book/master/alchemybook.gif)-->

# The Alchemy Book

This application is a toy collaborative editor that demonstrates using
Conflict-Free Replicated Data Types (CRDTs) to achieve real-time synchronization. The server 
is written in  Elixir and mostly of the CRDT logic is written in Typescript.

The aim of this project is educative and optimized for understandability rather
than performance. The blog post that explains the intuition behind this technique
can be found at
[A simple approach to building a real-time collaborative text editor](http://digitalfreepen.com/2017/10/06/simple-real-time-collaborative-text-editor.html).

I named the project Alchemy Book since I thought it would look nice with a blackboard
theme. Since this is not going to compete with Google Docs/Dropbox Paper/etc anyway, practicality
is not a concern.

## Setup

Assuming you have Elixir installed, and Postgres installed and running
(see Elixir & Phoenix [setup guide](https://hexdocs.pm/phoenix/up_and_running.html))

```
mix deps.get
mix ecto.create
mix ecto.migrate
```

Run with

```
mix phoenix.server
```

## Typescript

Install the Typescript compiler and linter globally

```
npm install -g tslint typescript
```

and run the linter with `npm run lint`

## Testing

We use Ava

```
npm install -g ava
```

which you can run with `npm test`

## Elixir static checking

Use `mix dialyzer` to do static checking with Dialyzer. The first time should
take a lot of time as it runs on the dependencies too, although the results will
be cached. Note that there are still a lot of (what I think are) spurious
warnings. This is to be fixed later.

## Deploying

There's two ways to deploy to production.

1) The simple way is to checkout this repository (or your fork) within your
server and setup Erlang, Elixir, and all the appropriate tools (this is the
same as running it a prod build locally).

2) Send the self-contained package in `alchemy_book.tar.gz` to your server,
unpack and run. This is less work than (1) and can be done with uncommited
changes which can be useful for testing. The thing I haven't figured out with
this approach is how to include and run the ecto migrate script, in the package,
so I still do (1) when I need to run `mix ecto.migrate`.

In either case, you'll want to make a production build. Create a symbolic link
to your `prod.secret.exs` file (and maybe to your deploy script too):

```
ln -s <path>/prod.secret.exs config/prod.secret.exs
```

with

```
config :alchemy_book, AlchemyBook.Endpoint,
  secret_key_base: "<secret key>"

# Configure your database
config :alchemy_book, AlchemyBook.Repo,
  adapter: Ecto.Adapters.Postgres,
  username: "<username>",
  password: "<password>",
  database: "alchemy_book_prod",
  pool_size: 20
```

Compile a production build with.

```
MIX_ENV=prod mix phoenix.digest
MIX_ENV=prod mix release --env=prod
```

The database will need to be created and migrated in production mode too:
```
MIX_ENV=prod mix ecto.create
MIX_ENV=prod mix ecto.migrate
```

And run with

```
PORT=8080 _build/prod/rel/alchemy_book/bin/alchemy_book <console/foreground/start>
```

# Technical notes

## Logoot

This repository contains a mostly faithful implementation of [Logoot](https://hal.archives-ouvertes.fr/inria-00432368/document).
Note that Logoot was not originally designed for real-time editing. It was designed as a way
to implement a decentralized Wikipedia with a unique ID for each line and sending line
insertion/removals. I just changed it to be character-based (send individual character changes). This
means that the memory overhead is huge (10x the size of the text itself)
but that should be ok as most people don't write text documents of more than a few megabytes.

Logoot is supposed to work in the absence of a central server, and I think it should
still be possible here, with some minor changes. The current implementation uses a
centralized server though to relay messages between clients.

## Phoenix Presence CRDT

The presence indicator (that shows who else is present in the document) as well as cursor position is implemented using the Presence library that ships with Phoenix. Fun fact, Presence is itself a CRDT.

## Notes about the code

I tried to keep a reasonably good code quality and write comments so that it's easy
for people to poke around the code. However, a couple of heads-up:

- There is some unused code laying around for signing up, logging in and creating documents
per user. It originally started that way, but I figured that it's simpler to just have
anonymous document sessions for the purpose of a demo.

- There's an unfinished attempt at using a more efficient order statistics tree for storing
CRDT characters. I left it there since it makes the array-based implementation better abstracted
even if it's not used.

- I used immutable data structures where possible (it's hard when CodeMirror is inherently
stateful). It's not really necessary, but you'll notice there's a bit of functional-style
programming inspired from OCaml at places.

## Things that would be nice to implement

- [ ] Re-enable user login and implement access control
- [ ] Linting for .tsx and .ex files
- [ ] Upgrade to Elixir 1.5 and Phoenix 3
- [ ] Having characters fade-in and fade-out when remote changes arrive would be cool
- [ ] Fuzzy testing for the CRDT logic
