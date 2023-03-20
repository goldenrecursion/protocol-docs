---
description: >-
  Learn about the changes launching in the next phase of the Golden Protocol's
  testnet.
---

# Testnet Phase 2

On \[X date] the Golden Protocol testnet officially entered **Phase 2.** The major change in this phase is that we have implemented a gating mechanism for both submitters and verifiers to earn points and impact consensus. We have entered this phase to improve data quality and limit the influence of bots, lazy voting and sybil attacks. Those who have participated in testnet phase 1 will keep the points they have earned up until now.



### **How testnet phase 2 will work:**



**Become a qualified verifier to earn points, impact consensus and submit triples**

* _Elected verifiers_ have initially been chosen manually by the Golden team based on their history of high-accuracy verification. We will adjust the _elected verifier_ cohort on a regular basis through methods such as scheduled election, manual wallet review, and community review.
* Each account will go through an "initial verification period" where you will vote on a series of triples.&#x20;
* Consistent similarity in your voting behavior with _elected verifiers_ on these initial triples will grant you a `QUALIFIED_VERIFIER` role on your account and unlock the ability to earn points and impact consensus. You must accurately vote on an adequate number of triples to be added to the qualified verifier pool. Please continue verifying triples until you begin to earn points.
* The time you first gain the `QUALIFIED_VERIFIER` role, you will earn a 500 point _initialTestnetPointAward_. You will no longer gain this award during account creation. Votes before this award will not require collateral, but there will be a limit on the number of pending votes allowed to be cast before you are qualified.
* If you do not vote similarly to the elected verifiers after this initial verification period, you will be temporarily locked out of verification for 30 days.
* All votes cast before you earn the `QUALIFIED_VERIFIER` role will _NOT_ be eligible for points after the `QUALIFIED_VERIFIER` role is applied, they are only used to qualify you.



**After becoming a qualified verifier**

* If after you gain the `QUALIFIED_VERIFIER` role and your voting behavior consistently diverges with these elected verifiers, your `QUALIFIED_VERIFIER` role will be removed and you will not earn points or impact consensus until your similarity goes above a quality threshold again.&#x20;
* Submitters will need to become a _qualified verifier_ before submitting data.
* You can query `userRoles` in our GraphQL API to see if an account is a _qualified verifier_



This phase will increase data quality, protect submitters' efforts, and allow high accuracy verifiers to earn points.&#x20;

