---
description: >-
  Welcome to Golden's official documentation for its decentralized knowledge
  graph API!
---

# Golden API Docs

![](.gitbook/assets/golden\_api\_docs.png)

## GraphQL API

Golden's GraphQL API offers developers powerful and flexible queries to retrieve, submit, and validate the data in the Golden protocol. For more information on Golden and the protocol please read the [Golden Protocol FAQ](https://www.notion.so/goldenhq/Golden-Protocol-FAQ-78ae2357b9af44aeaa655cb1b1966ee4) guide.

{% hint style="info" %}
The Golden protocol is currently live on the Goerli testnet.&#x20;

Golden has prestaked testnet points to wallets with a certain number of testnet points to get started. To become eligible to submit or validate triples with the API or dApp, you must connect a wallet on Golden.com and submit at least one triple.

Validators submitting incorrect validations (those that do not agree with the consensus validation vote on a triple) will be penalized by losing a portion of their testnet points (testnet points will be used to calculate future eligible airdrops). In extreme cases, validators may lose all of their testnet points and/or their access to dApp.

Correct validations (those that agree with the consensus validation vote on the triple) will be rewarded with testnet points.

**Please read the** [**Triple Validation Guide**](https://www.notion.so/goldenhq/Triple-Validation-Guide-84ec0a78cfe941b9876007cccca61b31) **before submitting or validating triples.**
{% endhint %}

Developers using the Golden API to create agents that submit and validate triples are encouraged to join the [Golden Discord group](https://discord.gg/golden-protocol) to share ideas, feedback, and requests**.**

## Full Reference

Already have experience with GraphQL APIs and just want to jump in? Check out the API reference and GraphiQL console.

{% content-ref url="reference/api-reference.md" %}
[api-reference.md](reference/api-reference.md)
{% endcontent-ref %}

## Guides

Read through our guides to get comfortable with forming GraphQL requests and the API schemas.

{% content-ref url="guides/" %}
[guides](guides/)
{% endcontent-ref %}

## Python Users

The Golden community highly encourages data-oriented developers to interact with the Golden protocol, so we provide our very own python wrapper to help you get started.

{% content-ref url="godel-python-sdk/" %}
[godel-python-sdk](godel-python-sdk/)
{% endcontent-ref %}
