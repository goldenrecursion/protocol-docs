---
description: >-
  Users must use testnet points as collateral against their votes and
  submissions
---

# Staking

The testnet point balance in a user's [wallet](https://dapp.golden.xyz/wallet/points/contributions)[ in the dApp](https://dapp.golden.xyz/wallet/points/contributions) reflects the number of testnet points they have _staked_. Users use a portion of their staked testnet points as collateral against new votes and submissions.

Staking limits a user's ability to contribute a significant number of erroneous submissions or votes to the protocol. In order to submit or vote, a user's staked testnet points balance must be greater than the sum of that user's testnet points collateralized against pending votes and submissions.

The number of testnet points required as collateral against a vote or submission will be equivalent to the maximum testnet points that could be slashed due to that vote or submission.

* When a vote agrees with consensus / a triple is accepted, the vote caster / triple submitter is returned the testnet points collateralized against that vote / submission and receives an additional reward
* When a vote disagrees with consensus / a triple is rejected, the vote caster / triple submitter loses the testnet points they collateralized against that vote / submission

You will not be able to contribute new votes or submissions if:

* Your staked testnet point balance is 0
* Your entire staked testnet point balance is collateralized
