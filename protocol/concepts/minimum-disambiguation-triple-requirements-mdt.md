---
description: >-
  Entities submitted to the protocol must pass the Minimum Disambiguation Triple
  requirements associated with the entity's type.
---

# Minimum Disambiguation Triple Requirements (MDT)

The Minimum Disambiguation Triples (MDT) for an entity type are the smallest set of unique triples needed to define an entity of that type, and are required to create an entity of that type. MDT requirements will change depending on the entity's proposed type - a 'person' has different MDT requirements from a 'company', for example. Minimum disambiguation triples are required to create an entity in the protocol.\
\
An entity that does not meet its MDT requirements may be ambiguously defined. For instance,\
\
'Tim Cook' with triple\
\* 'Tim Cook' -> 'Location' -> 'California'\
\
could refer to a number of different entities that have the same name and location. \
\
'Tim Cook' with triples\
\* 'Tim Cook' -> 'CEO' -> 'Apple Inc.' and \
\* 'Tim Cook' -> 'Twitter URL' -> '[https://twitter.com/tim\_cook](https://twitter.com/tim\_cook)'\
\
is a unique entity that any user can disambiguate.
