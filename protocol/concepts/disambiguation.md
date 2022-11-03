---
description: >-
  Disambiguation is the process of retrieving the canonical entity in the Protocol which is subject of a group of submitted triples.
---

# Disambiguation

Disambiguation is the process of retrieving the entity on the Graph's Protocol that would be the subject of a set of triples.

For example, from the triple ‘Apple Inc.’ → ‘CEO’ → ‘Tim Cook’, we know that an entity named *Apple Inc.*, has *Tim Cook* as a CEO. What we don't know, is the unique handle that represents the subject entity on the Graph. The disambiguation process will help us find the actual entity reference existing in the graph.

A good disambiguation strategy is important for both data submission and retrieval as it helps target the following problems:

- Prevents the creation of duplicates. If we can disambiguate effectively, it's harder for users to resubmit data that already exists onto the protocol, so there's less waste in the validation process, and rewards for contributing to the Protocol have less dillution.
- Makes search efficient, which increases the usefulness of the graph for data retrieval.

Since this is a core problem of building a knowledge graph, there's a new [API query](dapp.golden.xyz/graphiql), `disambiguateTriples()`, which can be used by any user for data retrieval, or as part of their triple submission flow.

To help onboarding users, there's a detailed tutorial on how you can use this API during the triple submission process on the [API section](https://docs.golden.xyz/api/api-disambiguation-tutorial)
