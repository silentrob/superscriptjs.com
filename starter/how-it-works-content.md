## What is this thing

SuperScript is a dialog system + bot engine for creating human-like conversation chat bots. It exposes an expressive script for crafting dialogue and features text-expansion using [WordNet] and information retrieval and extraction using [ConceptNet]. 

## What comes in the box
* Dialog Engine
* Multi-User Platform for easy intergration with Group Chat systems
* Message Pipeline with POS Tagging, Sentence Analysys and Question Tagging
* Extensible Plugin Architecture
* A built in Graph database using LevelDB and each user has their own SubLeve DB.
* [ConceptNet](http://conceptnet5.media.mit.edu/) General purpose database for knowledge extraction.
* [WordNet](http://wordnet.princeton.edu/) DB for word and concept expansion.

## How it works

The message pipeline contains many steps, and varies from other implementations.

When input comes into the system we convert the input into a message object. The message object contains multiple permutations of the original object and has been analyzed for parts of speech and question classification. The message is first handled by the reasoning system, before being sent to the dialog engine for processing.
