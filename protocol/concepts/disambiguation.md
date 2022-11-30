---
description: >-
  Disambiguation is the process of determining the uniqueness and existence of
  an entity or triple.
---

# Disambiguation

Disambiguation in the context of the knowledge graph is the process of determining the uniqueness and existence of an entity or triple.&#x20;

For example, from the triple ‘`Apple Inc.’ → ‘CEO’ → ‘Tim Cook’`, we know that an entity named _Apple Inc._, has _Tim Cook_ as a CEO. What we don't know, is the unique handle that represents the subject entity on the Graph. The disambiguation process will help us find the actual entity reference existing in the graph.

A good disambiguation strategy is important for both data submission and retrieval as it helps target the following problems:

* Prevents the creation of duplicates. If we can disambiguate effectively, it's harder for users to resubmit data that already exists onto the protocol, so there's less waste in the verification process, and rewards for contributing to the Protocol have less dilution.
* Makes search efficient, which increases the usefulness of the graph for data retrieval.

Since this is a core problem of building a knowledge graph, there is an [API query](https://dapp.golden.xyz/graphiql), `disambiguateTriples()`, which can be used by any user for data retrieval, or as part of their triple submission flow.

This process is offered for use through the [disambiguation-service](../../api/disambiguation-service/ "mention"), where specific entities can be determined by the submission of a set of triples.

To help onboard users, there's a detailed tutorial on how you can use this API during the triple submission process in the [API section](../../api/disambiguation-service/).
