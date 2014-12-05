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
- {redirect=greeting}

+ greeting
- Hello
- Ola
- Good day

```

Here is another use for redirecting.

```sh
?:WH *
- I'm not sure. {redirect=ask}
- No idea... {redirect=ask}

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

<div class="doc-box doc-info">
  There will be an entire doc on custom functions, but for now, explore <b>this</b> inside the plugin and review the test suite to get a better picture of some of the things possible.
</div>


<!-- <a class="doc-anchor" name="topics" ></a>
# Topics


<a class="doc-anchor" name="objref" ></a>
# Object Reference

 -->
 
 [RiveScript]:http://www.rivescript.com/
 [ChatScript]:http://chatscript.sourceforge.net/
