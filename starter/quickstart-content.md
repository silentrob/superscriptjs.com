
# Installing Dependancies

SuperScript has a dependency on <a href="https://github.com/silentrob/conceptnet" title="Helps setup Concept 4 Database with MySQL and provides some sugar for getting some data out">ConceptNet Bridge</a> and requires <a href="http://mysql.com">MySQL</a>, and manually importing the SQL database.


# Installing

First, create a directory to hold your bot, if you haven't already done so, and make that your working directory.
```sh
$ mkdir mybot
$ cd mybot
```

Create a `package.json` file in the directory of interest, if it does not exist already, with the `npm init` command.

```sh
$ npm init
```

Install SuperScript in the app directory and save it in the dependecies list:

```sh
$ npm install superscript --save
```

# Your first bot

Create a Topic Folder - This is where all the conversations rules will go

```sh
$ mkdir topics
```


Creating your first rule. Create a main.ss in your topic folder with the following text. <br/>[Learn more about rules.](/documentation/scripting)

```sh
+ hello world
- Hi from your bot.
```

## Compile the topic folder

Superscript uses a two step compile / run process to speed up the initial app load time.

```sh
$ ./node_nodules/superscript/bin/parse

```

This will compile the topics down to a *data.json* file which we will feed into the bot engine.


```sh
Usage: parse [options]

Options:

  -h, --help           output usage information
  -V, --version        output the version number
  -p, --path [type]    Input Path
  -o, --output [type]  Output options
  -f, --force [type]   Force Save if output file already exists

```

## Creating your first bot

Lets use the telnet example server to test out our new bot.

```sh
$ cp ./node_modules/superscript/example/telnet.js ./bot.js

```

Run the telnet server

```sh
$ node bot.js

```

In a new window, create a telnet connection.

```sh
$ telnet localhost 2000

Trying 127.0.0.1...
Connected to localhost.
Escape character is '^]'.
Welcome to the Telnet server!
Hello 127.0.0.1:53878! Type /quit to disconnect.

You>

```




<!-- <div class="doc-box doc-info">
Node modules installed with the `--save` option are added to the `dependencies` list in the `package.json` file.
Then using `npm install` in the app directory will automatically install modules in the dependecies list.
</div> -->
