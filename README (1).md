---
description: >-
  Welcome to Golden's official documentation for its decentralized knowledge
  graph API!
---

# Golden Protocol API Docs

![](.gitbook/assets/golden\_api\_docs.png)

Golden's GraphQL API offers developers powerful and flexible queries to retrieve, submit, and verify the data in the Golden protocol. For more information on Golden and the protocol please read the [README (2).md](<README (2).md> "mention") guide.

{% hint style="info" %}
The Golden protocol is currently live on the Goerli testnet.

Golden has prestaked testnet points to wallets with submitted triples on Golden.com to get started. To become eligible to submit or verify triples with the API or dApp, you must connect a wallet on Golden.com and submit at least one triple.

Verifiers submitting incorrect verifications (those that do not match the consensus verification vote on a triple) will be penalized by losing a portion of their testnet points (testnet points will be used to calculate future eligible airdrops). In extreme cases, verifiers may lose all of their testnet points and/or their access to dApp.

Correct verifications (those that agree with the consensus verification vote on the triple) will be rewarded with testnet points.

Attribution and eligibility for testnet points on triple submissions will be assigned by the earliest timestamped transaction.

**Please read the** [triple-verification-guide.md](protocol/guides/triple-verification-guide.md "mention") **before submitting or verifying triples.**
{% endhint %}

Developers using the Golden API to create agents that submit and verify triples are encouraged to join the [Golden Discord group](https://discord.gg/golden-protocol) to share ideas, feedback, and requests.

## Full API Reference

Already have experience with GraphQL APIs and just want to jump in? Check out the API reference and GraphiQL console.

{% content-ref url="api/overview/api-reference.md" %}
[api-reference.md](api/overview/api-reference.md)
{% endcontent-ref %}

## Python Users

The Golden community highly encourages data-oriented developers to interact with the Golden protocol, so we provide our very own python wrapper to help you get started.

{% content-ref url="godel-python-sdk/" %}
[godel-python-sdk](godel-python-sdk/)
{% endcontent-ref %}
