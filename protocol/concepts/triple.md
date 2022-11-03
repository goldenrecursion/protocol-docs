---
description: >-
  A triple is a set of data that codifies a statement in the form of
  subject–predicate–object expressions and is used in structured data statements
  in the Protocol.
---

# Triple

A triple (also know as a 'fact triple', 'semantic triple', 'SPO triple') is a set of data that codifies a statement in the form of subject–predicate–object expressions. Triples represent the base unit of knowledge in the Golden Protocol's Knowledge Graph.\
\
Triples are portable, composable, verifiable, and can be disambiguated. They function as a building block for forming statements around entities, data, and their relationships.\
\
For example, ‘Apple Inc.’ → ‘CEO’ → ‘Tim Cook’ is a triple.\
\* The _subject_ of the triple is the entity that the triple is referring to ('Apple Inc.')\
\* The _predicate_ of the triple defines the relationship between the subject and object (‘CEO’)\
\* The _object_ of the triple is the entity or data that the subject has a relationship to (‘Tim Cook’)\
\
You can find the list of supported predicates in the protocol on its public [schema page](https://dapp.golden.xyz/schema). Each object takes a different data type, based on the triple predicate. The accepted data types can vary, from simple string or integer values, to links into other entities already submitted into the graph.\
\
You can get started programmatically submitting triples on the protocol by following [the Protocol API docs](https://app.gitbook.com/o/GxTwjp9KiiL0b4GGYCBj/s/DDs2CreQGfhAB63p7YMp/\~/changes/KHNOU1ZmkB5JTBXHBbMf/api/readme) or through Golden.com's UI by following the [Adding Structured Data Guide](https://www.notion.so/goldenhq/Adding-Structured-Data-Guide-ae657337bf4f4e54ae4402df083c76ac).\
\
The submitter will receive testnet points for submitting a triple that goes on to be validated. Later, the submitter may be eligible for revenue share in the triple's subject entity through an NFT.
