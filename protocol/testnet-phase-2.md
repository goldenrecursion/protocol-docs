---
description: Learn about the next phase of the Golden testnet
---

# Testnet Phase 2

Testnet phase 2 introduces new requirements for submitters and verifiers prior to earning points and impacting consensus. Testnet phase 2 is designed to improve data quality and limit the influence of lazy voting and Sybil attacks. Those who have participated in testnet phase 1 will keep the points they have earned up until now if they are good actors. These are the initial mechanics of testnet phase 2 and are subject to modification and improvement.

* All existing and new users must pass a qualification process to earn points.
* There exists new groups of users: _Elected Verifiers, Qualified Verifiers, Qualified Submitters and everyone else._
* Only _Qualified Verifiers_ votes impact consensus. \*\*
* Only _Qualified Submitters_ are able to submit data.
* _Elected Verifiers_ are used to set accuracy benchmarks for the qualification process.
* Users who have not passed the qualification process will not earn points in this phase.
* Historical Testnet Phase 1 points will be respected for good actors.
* Bad actors will get banned from the system.
* Users who vote with sufficient similarity to the _Elected Verifiers_ will be promoted to a _Qualified Verifier**.**_
* Once you are a Qualified Verifier you are given 500 points for collateralization purposes. You will need to stake these points in order to begin performing actions.
* _Elected Verifiers_ are selected based on verification accuracy.
* A _Qualified Verifier_ whose verification accuracy falls below the required threshold will lose the _Qualified Verifier_ status and will not earn points until the status is regained.
* API users can query `userRoles` in our GraphQL API to see if an account is a `QUALIFIED_VERIFIER` or `ELECTED_VERIFIER`.
* Submitters will first need to become a Q_ualified Verifier_ before submitting data. We will be introducing a new mechanism to qualify as a submitter without the need to perform verification.
* Votes during the qualification phase do not impact consensus and will not earn points.

