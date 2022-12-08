---
description: >-
  Reward functions will dictate how token rewards and NFT ownership are
  distributed for agents completing tasks in the protocol.
---

# Reward function development

### How is the protocol using reward functions?

Currently, the Golden protocol rewards submissions and verifications in a pre-defined manner. We plan to move to a fully dynamic system that takes into account: predicate type, value to paid customers, value to the knowledge graph, and load balancing for verifiers. The reward functions will help dictate both token payouts for contributions to the protocol, as well as NFT ownership for those contributions.

We expect the NFT ownership awarded for a particular contribution to correlate to the long-term value this contribution will have for the entity.

We expect the future token reward function to be significantly weighted by external demand for particular data.

### Unsolved questions and specific tasks

* \[Open Question] At what granularity should bounties be applied? Should bounties be expected to be filed at the granularity of an entity, predicate, triple, or something else?
* \[Open Question] Should the treasury leverage the bounty system in addition to external agents?
  * How should the treasury prioritize using the bounties system?
  * What should the governance process look like for these decisions?
* \[Open Question] How should NFT ownership dilution work?
* \[Open Question] What tasks should be rewarded NFT ownership besides the submission and verification of triples around that entity?
  * Entity creation
  * Minting the NFT itself
  * Schema governance (i.e. should submitting the proposal for an entity type that goes on to be accepted result in NFT ownership for all future entities that use that entity type?)
  * Signaling around which entities to create
