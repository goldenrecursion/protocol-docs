---
description: >-
  Bounties incentivize the submission and verification of especially valuable
  data with higher rewards.
---

# Bounties

Creating a graph valuable for downstream consumption requires thoughtfully incorporating signals for what kind of data is valued. Bounties serve as a mechanism to attach economic incentives for submitting specific data requested by the market, and thus create a marketplace between data requesters and submitters.

Bounties are initially limited to requests for triples with a specified subject entity and predicate. Each bounty will specify the number of triples that are eligible for an incentive. For example, a bounty for Golden -> Investors may provide economic incentives for the first five triples accepted, while a bounty for Golden -> Location may only incentivize a single triple.

Users who submit a triple with a subject-predicate combination matching an open bounty are eligible to receive their incentive after that triple is accepted in voting and added to the protocol.

Bounties may be completed in the dApp's UI or programmatically. See [completing-a-bounty.md](../guides/completing-a-bounty.md "mention").

Later, specifications and payout logic for bounties can be specified on-chain.
