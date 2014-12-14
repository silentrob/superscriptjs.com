<!-- ## Documentation
* What is this thing
* Triggers
  * Basic rules
  * Wildcards
    * Variable Length Wildcards
    * Exact Length Wildcards
    * Open Eneded Catch alls
  * Alternate words in triggers
  * Optional Words in triggers
  * Captureing Data Back
  * Special Matches
    * Question Types
    * Answer Types
  * Wordnet Expansion
* Replies
  * Reply flag {keep} 
  * Reply redirect {@redirect=} 
  * Custom Functions and Plugins
    * Date Time Functions
    * Wordnet Functions
  * Reasoning with ConceptNet
* From Personal Assistant to best friend
  * "*" command and how it works
  * "*:1" special modifiers for first thing said
* Working with Topics
  * What is a topic
  * The '__begin__' topic
  * The 'random' topic
  * Topic Flags
    * keep
    * nostay
  * Topic before and after hooks
    * The 'pre' topic
    * The 'post' topic
* The Message Object
* The User Object -->

# Scripting Documentation

## What is this thing...
SuperScript is a dialogue scripting language and bot engine for creating chat bots. This software could be used to create a personal assistant like Siri or Google Now, however SuperScript also supports fully interactive style chatting as well and could reach out without being initiated.

If you are familiar to conversational scripting software, SuperScript is simular to [RiveScript] with its ease of use in Syntax and [ChatScript] for its expressive and powerful nature.

<a class="doc-anchor" name="triggers" ></a>
# Triggers

Triggers form the basis of all chat conversation, they begin with a *+* or *?* and create a regular expression to match input on. For example:

```sh
+ hello bot
- Hello, human!
```

Will match input that *'hello bot'* and reply with *'Hello, human!'*.

<a class="doc-anchor" name="rules" ></a>

## Basic Rules

Triggers are cleaned and normalized prior to being exposed to the engine, so the following rules are all identical.

```sh
+ hello bot
+ HELLO BOT
+ Hello, bot
+ hello     BOT!
```

Extra whitespace, and punctionation is removed, as well matches are always case insensitive.


<a class="doc-anchor" name="wildcards" ></a>
## Wildcards

We use wildcards to allow for more natural and expressive rules.

```sh
+ * hello *
- That is crazy.
```

A generic wildcard character will match *Zero to Unlimited* characters, words or tokens, and the input is *NOT* captured or saved by the system.

The rule above will match: *The dog said hello to the cat.* as well as *hello world* or *while hello*

<a class="doc-anchor" name="variable" ></a>
### Variable Length Wildcards

If you want to only allow a few words though, you might consider using a variable length wildcard

```sh
+ *~1 hello
- That is crazy.
```

The above will now match *hello* and *while hello* but fail to match *I said hello.*.
The syntax for variable length wildcards are *~n, where n is how many words you want to let though.


<div class="doc-box doc-info">
Variable length wildcards are great for capturing input where an adjective might be slipped in before a noun.
</div>

```sh
+ *~1 // Zero to One word
+ *~2 // Zero to Two words
+ *~3 // Zero to Three words
 ...
 + *~99 // Zero to Ninety-Nine words

```

<a class="doc-anchor" name="exact" ></a>
### Exact Length Wildcards

If you know exactly how many words you want to accept, but don't want to list what the words are, then an exact length wildcard might be what you're after. This is identical to the variable length wildcard above with only a slight difference.

```sh
+ *1 // One word
+ *2 // Two words
+ *3 // Three words
 ...
 + *99 // Ninty-Nine words

```

<a class="doc-anchor" name="alternate" ></a>
## Alternates

Alternates are used when you have a range of words that could fit into the rule, but one is required.
```sh
+ my (truck|car|van) is red
- my vehicle is blue.
```
<a class="doc-anchor" name="optional" ></a>
## Optionals

Optionals can be used to add an extra optional word.
```sh
+ my [big] red balloon [is awesome]
```

The above will allow all of these options below:
```sh
my red balloon
my big red balloon
my red balloon is awesome
my big red balloon is awesome
```


<a class="doc-anchor" name="capture" ></a>
## Capture Data Back

Sometimes you want to return the result of what was said back to the user in the reply. This is easily done by using the `<cap>` tag.

```sh
+ my name is *1
- nice to meet you <cap>.
```

*1 is substituted into `<cap>`. 

If you have more than one thing you want to capture, just add a number to the `<cap>`.
```sh
+ *1 is taller than *1
- Okay <cap1> is taller than <cap2>.
```

<div class="doc-box doc-info">
Only wildcards that are NOT ZERO length can be used to capture input. So wildcards and variable length wildcards will not capture the text back, but exact length wildcards and optionals will capture the input.
</div>


<a class="doc-anchor" name="special" ></a>
## Special Matches

<a class="doc-anchor" name="qtype" ></a>
### Question Types

We have seen above how *+* will match any input, if however you want to just match a question you can use *?* to begin your trigger pattern.

```sh
? * 
- Interesting question, let me get back to you on that.
```

SuperScript can go one step further and disseminate between different question types.

```sh
WH - WH Questions, who, what where when why. 
CH - Choice Questions, This or that.
YN - Yes/No Questions
TG - Tag Questions, Ann is taller then Jane. 
```


```sh
?:WH * store 

// Matches
// Who went to the store?
// Why did you go to the store?
// When did visit the store?
// Where is the nearest store?
```

```sh
?:YN * store 

// Matches
// Do you want to go to the store?
// Did you get everything you needed from the store?
```


<a class="doc-anchor" name="answer" ></a>
### Answer Types

We can also match based on the type of answer we expect back from the message, and SuperScript supports 8 broad categories and over 40 sub categories with 80% accuracy.

```sh
ABBR        - abbreviation
  abb       - abbreviation
  exp       - expression abbreviated
ENTY        - entities
  animal    - animals
  body      - organs of body
  color     - colors
  creative  - inventions, books and other creative pieces
  currency  - currency names
  event     - events
  food      - food
  instrument - musical instrument
  lang      - languages
  letter    - letters like a-z
  other     - other entities
  plant     - plants
  product   - products
  religion  - religions
  sport     - sports
  substance - elements and substances
  symbol    - symbols and signs
  technique - techniques and methods
  term      - equivalent terms
  vehicle   - vehicles
  word      - words with a special property
DESC        - description and abstract concepts
  def       - definition of sth.
  desc      - description of sth.
  manner    - manner of an action
  reason    - reasons
HUM         - human beings
  group     - a group or organization of persons
  ind       - an individual
  title     - title of a person
  desc      - description of a person
LOC         - locations
  city      - cities
  country   - countries
  mountain  - mountains
  other     - other locations
  state     - states
NUM         - numeric values
  code      - postcodes, phone number or other codes
  count     - number of sth.
  expression - numeric mathmatical expression
  date      - dates
  distance  - linear measures
  money     - prices
  order     - ranks
  other     - other numbers
  period    - the lasting time of sth.
  percent   - fractions
  speed     - speed
  temp      - temperature
  size      - size, area and volume
  weight    - weight
```

Here are some examples:

```sh
?:NUM:code * phone *
- No you can't have my number.

?:NUM *
- The answer is 42

```

<a class="doc-anchor" name="pos"></a>
## Parts of Speech

When input comes into the system, we tag it and analyze it to help make sense of what is being said. We also allow for inline subsitutations back into the actual trigger.

SuperScript has a few tagged keywords you can use in tiggers such as;

* `<noun>`, `<nounN>`, `<nouns>`
* `<adjective>`, `<adjectiveN>`, `<adjectives>`
* `<verb>`, `<verbN>`, `<verbs>`
* `<adverb>`, `<adverbN>`, `<adverbs>`
* `<pronoun>`, `<pronounN>` `<pronouns>`
* `<name>`, `<nameN>`, `<names>`

These tags can also have a numeric value attached to them to get even more specific.

```sh
// Tom is more tall than Mary
+ [if] <name1> [is|be] [more|less] <adjectives> (then|than) <name2>
```

Here we know to replace `<name1>` with Tom and `<name2>` with Mary, ensuring name1 does not equal name2.

<a class="doc-anchor" name="convo" ></a>
## Conversations
For conversation to really flow you need to have more then one exchange. We use a *%* command to recall the previous thing said and continue to build off of that.

```sh
+ * 
- What is your favorite color?

  
  % What is your favorite color?
  + *1
  - <cap> is mine too.

```

So let's walk though this example. Some input comes into the system and the open wildcard matches and replies with *What is your favorite color?*. 

When the next input comes into the system, we first check the history and see if the previous reply has more dialogue. This is noted by the *% What is your favorite color?* line.

We pull the next trigger, in this case it is **1* meaning one word. The bot then replies with *`<cap>` is mine too.* replacing that one word with `<cap>`.

<a class="doc-anchor" name="wordnet" ></a>
## WordNet Expansion


WordNet is a database of words and ontology including hypernym, synonyms and lots of other neat relationships between words.

SuperScript is using the raw WordNet library, as well it has expanded it to include fact tuples and provide even more relationships, through its scripted fact graph database.

These terms are expanded by using a tilde `~` before the word you want to expand.

```sh
+ i ~like ~sport

// Matches
I like hockey
I love baseball
I care for soccer
I perfer lacrosse

```

## A few more thoughts on Triggers
When input comes into the system it is bursted into seperate message objects and handled seperatly. We break input on ending punctuation AND commas following WH words.

```sh
> My name is John. What is your name?
Message one => My name is John 
Message two => What is your name

> My name is John, where are you from?
Message one => My name is John 
Message two => where are you from

```

The reply of each gambit is concatenated back together to form the final message to the user.


<a class="doc-anchor" name="replies" ></a>
# Replies

We have seen examples of Replies above in most of the examples, they start with a *-* followed by text. Replies appear in their final form. Meaning however it is written, that will be displayed to the user – grammar, mistakes and all.

```sh
+ i say this
- this is a reply
- this is another reply
- this is my final reply
```

Replies can be stacked up and will be chosen at *RANDOM*. Once a reply has been used, we discard it and won't use it again. So it is a good idea to have a few replies for every trigger. This will also keep your dialogue less robotic and more natural.


For longer replies that need to wrap accross lines, we use a carrot *^* to note the replies continues on to the next line.
```sh
+ tell me a haiku
- An old silent pond...
^ A frog jumps into the pond,
^ splash! Silence again.

```

<a class="doc-anchor" name="keepflag"></a>
## Keep Flag
If you have a reply that needs to stick around after it has been said you need to mark it with the *{keep}* flag.

```sh
+ i say this
- {keep} this is a reply
```
<div class="doc-box doc-info">
  Anything between {} will not show up in the final reply.
</div>

<a class="doc-anchor" name="redirect"></a>
## Redirecting

Sometimes you want to redirect from one reply to another.

```sh
+ hello friend
@greeting

+ greeting
- Hello
- Ola
- Good day

```

Here is another use for redirecting inline.

```sh
?:WH *
- I'm not sure. {@ask}
- No idea... {@ask}

+ ask
- How old are you?
- Where do you live?
- What are you doing?

```

By doing this we can break the reply into smaller pieces and create more dynamic / expressive replies that keep the conversation rolling.

<a class="doc-anchor" name="custom"></a>
## Custom functions

This is where things get interesting. Replies can execute a custom function where they have access to the entire message object, the user object and other resources like the fact databases.

```sh
?:NUM:expression *
- ^solveExpression()

// You can also pass captured values directly into the function like so.

?:WH * weather in <name>
- ^getWeather(<cap1>)

```

Functions are imported from the plugin folder at load time and are async Node.js functions.

```javascript
exports.solveExpression = function(cb) {
  cb(null, "What do I look like a computer?!");
}

exports.getWeather = function(city, cb) {
  cb(null, "It is probably sunny in " + city);
}

```

You can have more than one function in a reply, so this is also valid: 

```sh
+ my custom function
- ^callthisFunction() than call ^thisFunction()

```

The results of the callback will replace the function name. You can even nest functions within callback replies, so this is valid too.

```sh
+ my custom function
- ^nestedAFunction() 

```

```javascript
exports.nestedAFunction = function(cb) {
  cb(null, "We ^nestedBFunction()");
}

exports.nestedBFunction = function(cb) {
  cb(null, "are done");
}
```

The final reply will be *"We are done"*

The above example is contrived, remember you can also require the module and call the function directly from within the plugin code. This is also true for other NPM modules.


<div class="doc-box doc-info">
  There will be an entire doc on custom functions, but for now, explore <b>this</b> inside the plugin and review the test suite to get a better picture of some of the things possible.
</div>


<a class="doc-anchor" name="topics" ></a>
# Topics

## What is a topic            

A Topic is way to group triggers and replies together in some arbitrary, or logical way. There are no limit to how many topics you create, however navigating from topic to topic is done explicitly. You can think if a topic as a scope.

<a class="doc-anchor" name="random"></a>
## The 'random' topic
  
All dialogue belongs to a topic and even if you do not use any topics, those triggers and replies will be stored in the random topic.

### Creating a new topic

Topics are created by using the follow syntax

```sh
> topic topic_name
  + message as normal
  - reply as normal
< topic

```

While topic rules can be combined and nested, see includes below, they can not be nested in the file. *This would be invalid*:

```sh
> topic topic_a
  + message as normal
  - reply as normal
  > topic topic_b
    + some other message
    - some other reply
  < topic
< topic

```

### Changing to a new topic
To switch to a new topic you just need to set the new topic name in the reply.

```sh
  + I like animals
  - Me to {topic=animals}
```

If you are in a custom plugin you can also change topics directly by accessing the user object method *setTopic(...)*.

```javascript
exports.somefunction = function(cb) {
  this.user.setTopic("newTopic")
  cb(null, "")
}
```

<div class="doc-box doc-info">
  It is best practice to have one topic per file, however topics and span files.
</div>

<a class="doc-anchor" name="hooks"></a>
## Topic hooks

There are two special topics that run, one before your current topic, and one after.
We call them *pre* and *post* hooks or topics. If you need some trigger to be tested on every pass either before or after the regular triggers, this is where you would put it.

Pre and Post topics have a slightly different syntax, but function the same as normal topics.
```sh
> pre
  + pre hook
  - yep pre hook
< pre


> post
  + post hook
  - yep post hook
< post
```

As you can see the keyword topic has been replaced with 'pre' and 'post'.
  
<a class="doc-anchor" name="flags"></a>
## Topic flags

Topics can have a flag attached to them that modifies its behaviour. *keep* and *nostay* 
 
### keep

Keep is designed to disable to default behavour of exausting triggers, if you want the bot to say things over and over again, and apply keep just after the topic definition.
  
```sh
 > topic:keep keeptopic
  + i have one thing to say
  - I will keep saying it.
< topic
```

### nostay

Nostay can be used to bounce from one topic, but return back to the prevoius topic right after.
It might be a good way to break the flow up and allow the possibility to change the topic, but not do so forcefully. 

```sh
 > topic random
  + do you like ice cream
  - I do {topic=icecream}
< topic


 > topic:nostay icecream
  + i like *
  - my favourite flavour is vanilla, but we are off topic.
< topic
```


<a class="doc-anchor" name="include"></a>
## Topic includes

Topics can include triggers from other topics as well, this is done though either including them or inheriting them. 

```sh
> topic base
  + random thing
  - random reply...
< topic

> topic extbase includes base
  + more random things
  - and more random replies
< topic
```

If you are familiar with programming, this concept is similar to the mixin pattern.

<a class="doc-anchor" name="inherits"></a>
## Topic Inherits

```sh
> topic base
  + i like *
  - I like everything too

  + something clever
  - something witty back
< topic

> topic vehicle inherits base
  + i like *
  - I like vehicles too
< topic

> topic car inherits vehicle
  + i like *
  - I like cars too
< topic
```

When you inherit from the next topic, triggers that match get replaced. In the example above, if you are in the *car* topic, the "i like" trigger is overriding the same "i like" trigger from vehicle and base. However, you still have access to the *something clever* base trigger.


<a class="doc-anchor" name="debug"></a>
## Debugging

Making sure everything is working can be time consuming and frustrating. Start with the basics, check for warnings and error messages. All of SuperScript uses [debug](https://github.com/visionmedia/debug) and should make finding the problem much easier.

```sh
$  DEBUG=*,-Utils node_modules/superscript/bin/parse.js 
```

This says, lets see everything except things from the Util library. During the parse setup, we will load Normalizer, Parse each line, then sort the results prior to saving them.

Once you are confident all files, have been properly parsed you will have a data.json file. Or some other file if you choose to output to a different name.

Make sure the file is not empty, null or incomplete. It should have a few sections and each trigger you entered will be in their along with extra metadata.

Start the bot in debug mode.

```sh
$  DEBUG=*,-Utils node server.js
```
Assuming you copied over the telnet server example.
Here is output from my example bot Brit.

```sh
DEBUG=*,-Utils node server.js 
Script Loading Plugin +0ms ./plugins oppisite
Script Loading Plugin +3ms ./plugins rhymes
Script Loading Plugin +0ms ./plugins syllable
Script Loading Plugin +0ms ./plugins letterLookup
Script Loading Plugin +0ms ./plugins wordLength
Script Loading Plugin +0ms ./plugins nextNumber
Script Loading Plugin +1ms ./plugins createFact
Script Loading Plugin +0ms ./plugins resolveAdjective
Script Loading Plugin +0ms ./plugins evaluateExpression
Script Loading Plugin +0ms ./plugins numToRoman
Script Loading Plugin +0ms ./plugins numToHex
Script Loading Plugin +0ms ./plugins numToBinary
Script Loading Plugin +0ms ./plugins numMissing
Script Loading Plugin +0ms ./plugins numSequence
Script Loading Plugin +0ms ./plugins num
Script Loading Plugin +0ms ./plugins changetopic
Script Loading Plugin +0ms ./plugins changefunctionreply
Script Loading Plugin +0ms ./plugins getDOW
Script Loading Plugin +0ms ./plugins getDate
Script Loading Plugin +0ms ./plugins getDateTomorrow
Script Loading Plugin +0ms ./plugins getSeason
Script Loading Plugin +0ms ./plugins getTime
Script Loading Plugin +0ms ./plugins getTimeOfDay
Script Loading Plugin +0ms ./plugins getDayOfWeek
Script Loading Plugin +0ms ./plugins getMonth
Script Loading Plugin +0ms ./plugins save
Script Loading Plugin +0ms ./plugins wordnetDefine
Script Loading Plugin +0ms ./plugins plural
Script Loading Plugin +224ms /Users/robellis/projects/bot/brit/plugins aGender
Script Loading Plugin +0ms /Users/robellis/projects/bot/brit/plugins aBaby
Script Loading Plugin +0ms /Users/robellis/projects/bot/brit/plugins aLocation
Script Loading Plugin +0ms /Users/robellis/projects/bot/brit/plugins aGroup
Script Loading Plugin +0ms /Users/robellis/projects/bot/brit/plugins weather
Normalizer Loaded File +0ms { key: '_sys', file: 'systemessentials.txt' }
Normalizer Loaded File +4ms { key: '_extra', file: 'substitutes.txt' }
Normalizer Loaded File +116ms { key: '_contractions', file: 'contractions.txt' }
Normalizer Loaded File +14ms { key: '_interjections', file: 'interjections.txt' }
Normalizer Loaded File +65ms { key: '_britsh', file: 'british.txt' }
Normalizer Loaded File +198ms { key: '_spellfix', file: 'spellfix.txt' }
Normalizer Loaded File +314ms { key: '_texting', file: 'texting.txt' }
Normalizer Done Reading Subs +7ms
Normalizer Done Loading files +1ms
Script Questions Loaded +751ms
Script System Loaded, waiting for replies +0ms
TCP server running on port 2000.

```

This walks through the entire initialization process, showing various plugins loaded and is waiting to start chatting.
Once I open a telnet connection, we get much more output.

The message will walk though every piece of the framework showing how it gets to the final output. Usually the most important part of this process is to see what the message object looks like given your input, and how *GetReply* tries to match it with a given trigger.

```sh
GetReply Searching their topic 'random' for a match... +2ms
GetReply itorTopic +0ms __pre__
GetReply itorTopic +0ms random
GetReply Try to match 'test' against 9b9265b ((?:.*?)) +1ms
GetReply Try to match 'test' against 9b9265b ((?:.*?)) +0ms
GetReply Found a match! +0ms
GetReply Match found in Topic +0ms random
GetReply itorTopic +0ms __post__
GetReply Did we match? +0ms { trigger: '(?:.*?)',
options: { isQuestion: false, qType: false, qSubType: false },
reply: { '37c53b58': 'okay ~somethingmadeup {topic=car}' } }
GetReply Looking at choices: +1ms [ 'okay ~somethingmadeup {topic=car}' ]
GetReply We like this choice +0ms okay ~somethingmadeup {topic=car}
```

If you recall, the system will process pre topics, than your current topic, followed by the post topics, in our case the default random topic.

If you notice for each trigger, we will have two 

```sh
GetReply Try to match 'test' against 9b9265b ((?:.*?)) +1ms
```

This is because we will try to match against the given input as well as the input lemmatized. 

Finally, we see we do indeed have a match, and the system will show us what reply it choose, prividing we had more than one.





<!-- <a class="doc-anchor" name="objref" ></a>
# Object Reference
 -->