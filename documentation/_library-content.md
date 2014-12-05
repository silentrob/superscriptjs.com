

# Library API

This doc will show you how to use SuperScript to create a new application or intergration.

```sh
$ npm install superscript

```

We are assuming you have already serup node.js and have NPM installed (included with node) as well as a working understanding of JavaScript.
<a class="doc-anchor" name="bot"></a>
## The Bot Class

```javascript
var superscript = require("superscript");

/**
- conversationJSONData: A JSON file of all the scripted conversation compiled down to one file. This file is generated from the parse util in ./bin/parse

- options is a object litteral.
- callback is a function that returns callback(err, bot)

**/
new superscript(conversationJSONData, options, callback);

```

The Bot Instance returned from the callback has a few methods you can use to build out your bot.

### Reply Method
```javascript
/**
This is the main way to communcate with your bot.
- user_id, String, is a unique token used to identify what user is sending the message
- message, String, is the message or input from the user
- callback(err, reply), Function, is where the reply 
**/
bot.reply(user_id, message, callback);
```

### userConnect Method
```javascript
/**
Connecting a user to the system will register the user prior to the first message being sent. This means the bot can reach out to the user BEFORE any conversation has started.
It makes the bot aware of who is within earshot.
- user_id, String, User Token
**/
bot.userConnect(user_id);
```

### userDisconnect Method
```javascript
/**
This removes a connected user from the system.
- user_id, String, User Token
**/
bot.userDisconnect(user_id);
```

### getUser Method
```javascript
/**
getUser will return the user object from the bot. It is primarily a helper function.
- user_id, String, User Token
**/
bot.getUser(user_id);

```
<a class="doc-anchor" name="events"></a>
## Events
Superscript will emits events as well.

### message event

```javascript
bot.on('message', function(user_id, bot_message){
	console.log(user_id, bot_message);
});

```

If your script has things for the bot to say first, or during long awkward pauses, the will be sent to this event.


<a class="doc-anchor" name="example"></a>
## Example 

Here is the complete minimum example:
```javascript
var superscript = require("superscript");

new superscript('./data.json', {}, function(err, bot){
	bot.reply("exampleUser", "Hello Bot", function(err, reply){
		console.log(reply);
	})
});

```

<div class="doc-box doc-info">
	For a interactive example application, follow the steps on the getting started page and setup a chat server with Telnet.
</div>


