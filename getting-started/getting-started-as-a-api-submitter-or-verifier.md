---
description: >-
  Help build the decentralized protocol for knowledge by submitting and
  verifying information at scale.
---

# Getting started as a API submitter or verifier

## Steps TL;DR

1. [Get into the dApp](../protocol/guides/accessing-the-dapp.md) and retrieve your [authentication (JWT) token](../api/authentication.md).&#x20;
2. Follow the quick start guide in the API docs&#x20;
3. [Join Discord](https://discord.com/invite/golden-protocol), introduce yourself to the `#developers` channel and say what you are interested in making.
4. Explore the available guides, [data schema](https://dapp.golden.xyz/schema), [apps-and-demos.md](../data-and-tools/apps-and-demos.md "mention"), datasets, and posts.



## Getting into the dApp and collecting your JWT authentication token

To access the Golden protocol API you need to first authenticate with a JWT token. To get your token:

1. Follow [accessing-the-dapp.md](../protocol/guides/accessing-the-dapp.md "mention") to create an account and gain access to the dApp. After successfully logging in you will be able to [visit your profile page](https://dapp.golden.xyz/profile) to retrieve your [JWT token](../godel-python-sdk/authentication.md).
2. You can test that your token is working correctly by using one of our [apps-and-demos.md](../data-and-tools/apps-and-demos.md "mention").&#x20;
3. Read the [README (1).md](<../README (1).md> "mention") to get started with the GraphQL API or Python SDK.&#x20;

## Learn the knowledge graph schema

Before submitting or verifying data it is essential to understand the schema for the knowledge graph. The graph is created from subject predicate object (SPO) triples, for more background information read [triple.md](../protocol/concepts/triple.md "mention").

* You can currently submit and verify information related to the predicates listed in the [protocol schema](https://dapp.golden.xyz/schema).
* If the information you wish to submit is not included in this schema, you may request it through the [predicate-improvement-process.md](../governance/predicates/predicate-improvement-process.md "mention").

## API/SDK Tutorials

### Data submission

For a full tutorial on data submission follow the disambiguation walkthrough read[python-sdk-disambiguation-tutorial.md](../api/disambiguation-service/python-sdk-disambiguation-tutorial.md "mention")

### Verification

A verification tutorial using the Python SDK can be found at [verification.md](../api/godel-python-sdk/verification.md "mention").
