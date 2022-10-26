---
description: >-
  Ground Truth Triples are triples with a known answer that are used to test
  validators.
---

# Ground Truth Triples (GTT)

Ground truth triples (GTT) are triples shown to validators with a known answer. These triples are used as an anti-gaming mechanism to reduce low-accuracy validations.\
\
Periodically while voting, a ground truth triple will be added to a user's validation queue. If a validator encounters a ground truth triple and incorrectly validates it, their testnet points will be reduced. This process allows the protocol to quickly catch validators acting in bad faith and to remove them from the validation pool. Ground truth triples are indistinguishable from other triples and randomized in both frequency and predicate type.

If a user incorrectly votes on a ground truth triple they will be shown feedback immediately. Getting multiple ground truth triples wrong in a row will increasingly impact a user's testnet points.\
\
Over time, we expect triple submitters to become highly incentivized to submit accurately. At that point, ground truth triples will play a critical role in ensuring submitted triples are not blindly accepted by validators.
