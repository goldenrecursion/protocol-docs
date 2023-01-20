---
description: >-
  Reputation measures the protocol's confidence that a user is acting in good
  faith and affects that user's influence.
---

# Reputation

A user's reputation score represents the confidence the protocol has that this user is acting in good faith. Every user starts with a reputation score of 0 that is recalculated with:

* **Verifications**: a user's vote agreeing / disagreeing with the consensus outcome of a triple will increase / decrease the user's reputation score, respectively.
* **Submissions**: a user's submission reaching a consensus accept / reject will increase / decrease the user's reputation score, respectively.

A user's reputation score influences their voting weight in the [verification.md](verification.md "mention") process. Future improvements to reputation offer an opportunity to dynamically decrease the collateral requirements for submissions and verifications.

There is a finite amount of influence any one user can have in the Golden protocol. Initial reputation scores were calculated with a normalized distribution that will be adjusted over time.

Reputation scores are displayed in the top bar of the Golden dApp when logged in as shown below.
