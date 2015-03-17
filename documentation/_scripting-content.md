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
SuperScript is a dialogue scripting language and bot engine for creating chat bots. This software could be used to create a personal assistant like Siri or Google Now, however SuperScript also supports fully interactive style chatting.

If you are familiar to conversational scripting software, SuperScript is simular to [RiveScript](http://rivescript.com) with its ease of use in Syntax and [ChatScript](http://sourceforge.net/projects/chatscript/) for its expressive and powerful nature.

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
Only wildcards that are NOT ZERO length can be used to capture input. So wildcards will not capture the text back, but exact length wildcards and optionals will capture the input.
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

<a class="doc-anchor" name="oob"></a>
## Out of bound data

Out of bound replies data is used to pass extra metadata back to the client. The reply object can have extra properties added on from any reply.

```sh
+ can you (txt|sms) me the address
- {^hasNumber(true)} ^addMessageProp(medium,txt) 34345 Old Yale Rd.
- {^hasNumber(false)} I need your number first.
```
The resulting reply object look something like this:

```js
{ 
    string: "34345 Old Yale Rd.", 
    medium: "txt", 
    createdAt: "..." 
}
```

The function `^addMessageProp(key,value)` can be used from any reply or you can augment the message props variable directly from within any custom function.

```javascript
  this.message.props['key'] = value;
```


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

<a class="doc-anchor" name="filters"></a>
## Filter functions

<div class="doc-box doc-info">
 In the Community Editor Filters have their own text box with an auto complete.
</div>


Filters are used to filter out replies or triggers from being used. They are a like using conditional logic in your app. Filters are make use of Plugins that return a true / false value.

```sh
+ when I see this
- {^someTruthFunction()} Say this
- {^someTruthFunction()} Say that

```

In this example the someTruthFunction is evaluated when we are making the decision to pick a reply. Lets see a real example based on one of the Loebner questions.

```sh
+ my name is <name>
- {^hasName(false)} ^save(name,<cap1>) Nice to meet you, <cap1>.
- {^hasName(true)} I know, you already told me your name.
```

hasName checks to see if we know the users name, and if we don’t, (false) then we say “Nice to meet you <name>” and we save it using the save function. Then when we hit this trigger again, hasName(true) will be filtered in, and reply with a cheeky remark. “I know, you already told me your name.”

Filter functions are not just for replies, and they can be applied to triggers too, and I see extending this to topics as well, but we can save that for another release.

```sh
+ {^not(fish)} I like *
- Thats great.
```

The syntax is identical to filters in replies, this function not(…) will return false if the message contains the word fish.



### what is `this`
Custom functions are called in their own scope. This is done to give you control over most of the imprtant bits of the system.

`this.message`

We expose the entire message object which is broken down 12 diffent ways and includes all the words in raw and lemma form, all the parts-of-speech and even compound 'things' like entities and dates.

`this.user`

The user who sent the message as we know them. This will allow us to see the past messages in their history, how many conversaion replies we have had, and when the conversation started.

We also have direct access to the users memory.

`this.user.memory` and `this.user.memory.db`

From the user memory you can query a sub-level graph DB for known facts the user may have shared or save new ones.

`this.botfacts` and `this.botfacts.db`

This is identical to the sub-level graph for the user except it contains only things about the bot and can be loaded on init.

`this.facts` and `this.facts.db`

This is simular to the users sub-level graph except it contains all the global facts.

`this.topicSystem` 

We expose the topic system to the plugins in the even you want to create a new topic reply or trigger dynamically.

Because you are in the fun wild world of Node.js you can also install any library from NPM and use it, and since the plugs are async, you can even make calls to Databases, API's or other remote web services.

<div class="doc-box doc-info">
  There will be an entire doc on custom functions, but for now, explore <b>this</b> inside the plugin and review the test suite to get a better picture of some of the things possible.
</div>


<a class="doc-anchor" name="topics" ></a>
# Topics

## What is a topic            

A Topic is way to group triggers and replies together in some arbitrary, or logical way. There are no limit to how many topics you create. Topics have been complety redesigned after Version 0.2.0. SuperScript will how continue to try to find matches for your input outside of your current topic.


<a class="doc-anchor" name="random"></a>
## The 'random' topic
  
All dialogue belongs to a topic and even if you do not use any topics, those triggers and replies will be stored in the random topic.

### Creating a new topic

Topics are created by using the follow syntax

```sh
> topic topic_name (topic_keyword1 topic_keyword2 topic_keywordN)
  + message as normal
  - reply as normal
< topic

```

Topics can not be nested in the file. *This would be invalid*:

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

Topics can have a flag attached to them that modifies its behaviour. *keep*, *system* and *nostay* 
 
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

### system

System topics do not appear in the normal topic flow and you must provide your own way to access them, usually the `^respond()` function workes great for that.

```sh
 > topic random
  + * who *
  - ^respond(who)

  + * what *
  - ^respond(what)
< topic


 > topic:system who
  + who is *
  - I'm not sure I know you're talking about.
< topic

 > topic:system what
  + what do you like *
  - I like all sorts of things...
< topic
```
<a class="doc-anchor" name="flow"></a>
## Topic Flow

As noted eariler, the topic logic has improved to allow for easier scripting, and your bot should have access to more gambits. It is imprtant that your topics have well defined keywords, this will help when trying to figure out what topic to check nect.

The basic flow for each message cycle is as follows. *Pre topic, current topic, next best matched topic(s), any remaining topics, post topic*. Keep in mind that system topics will never appeat in the normal flow.

You can match more than once. Providing your match does not produce any output, the system will keep looking for matches. This is extra helpful when you have plugins or functions that try to do extra parsing on the message or make assumptions that may be false.


<a class="doc-anchor" name="functions"></a>
## Functions

This sections covers a few built-in or commonly used functions to help your crafting.

### Flow related

#### ^respond()
The `^respond(topic)` function is used in the reply it takes the current input and check it against the selected topic more matches. Once it gets to the end of the topic it will unwind its way back and continue checking in the normal path. There is an example of this function in the system flag above.

Consider

```sh
 > topic random
  + * i *
  - ^respond(i_topic)

  + * you *
  - ^respond(you_topic)

  + * we *
  - ^respond(we_topic)

< topic


 > topic i_topic
  + i am *
  - ^respond(i_am_topic)

  + i was *
  - ^respond(i_was_topic)

  + i need *
  - ^respond(i_need_topic)

< topic

 > topic i_am_topic
  + i am on fire
  - That's not good.

  + i am a wimp
  - You should eat your veggies.
< topic
```

#### ^topicRedirect()

`^topicRedirect(topic,trigger)` is used in the reply and is very simular to reply redirects except they redirect you to a new topic.

```sh

  + say something random
  - ^topicRedirect(something_random, __say__)

 > topic something_random
  + __say__
  - Wind on the face is not good for you.
  - I watched Mr. Dressup as a kid.
< topic

```

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