
# Installing Dependencies

SuperScript has a dependency on [Node JS](nodejs.org) and [Mongo](http://www.mongodb.org/). Please make sure they are installed prior to beginning.


# Installing the Realtime Editor (NEW)

SuperScript now has a fancy editor for crafting dialogue and authoring bots. The documentation on scripting will be applicable to both approaches, however parsing text files eventually be completely replaced by the SSC Editor in a future release.

```sh
$ git clone https://github.com/silentrob/superscript-editor.git
cd superscript-editor
npm install
npm start

Then Open your browser to http://localhost:3000
```

At this point, you are done. Just open your browser and start editing / chatting.


# Alternatively -- Installing the Library

First install superscript globally

```sh
$ npm install superscript -g
```

Then generate a new bot with either the telnet or slack client

```sh
$ bot-init mybot --client slack
$ cd mybot
```

Finally, install the dependencies locally.

```sh
$ npm install
```


## Your first bot

Open the Topic Folder - This is where all the conversations rules will go

```sh
$ cd topics
```

Creating your first rule. Create a main.ss in your topic folder with the following text. <br/>[Learn more about rules.](/documentation/scripting)

```sh
+ hello world
- Hi from your bot.
```

## Compile the topic folder

Superscript uses a two step compile / run process to speed up the initial app load time.

```sh
$ parse

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

SuperScript comes with two clients, a Slack client, and Telnet Client. They are included by initing a new project with the --client switch.


## Help! I followed the steps and nothing works!
Read the debugging section at the bottom of the [scripting docs](https://github.com/silentrob/superscript/wiki) and if your still stuck, create an issue on the [github repo](https://github.com/silentrob/superscript/issues).



<!-- <div class="doc-box doc-info">
Node modules installed with the `--save` option are added to the `dependencies` list in the `package.json` file.
Then using `npm install` in the app directory will automatically install modules in the dependencies list.
</div> -->
