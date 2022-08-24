---
description: >-
  Here is a short guide to validating triples on the Golden protocol. Before
  casting votes, be sure to read the Triple Validation Guide
---

# Validation

Here is a short guide to validating triples on the Golden protocol. Before casting votes, be sure to read the [Triple Validation Guide](https://www.notion.so/goldenhq/Triple-Validation-Guide-84ec0a78cfe941b9876007cccca61b31)

## Prerequisite

[Godel Authentication](https://docs.golden.xyz/godel-python-sdk/authentication)

### 1. Connect to Golden Web3 API

Let's connect the python wrapper to the Golden GraphQL API.

Make sure you ran through the prerequisites for this guide and have learned to authenticate and retrieve your JWT token in Godel.

```python
from godel import GoldenAPI

JWT_TOKEN = "ey098sd908v79899789877986567967845jh567hj5679568df678678daf6786789s569ghm567457hm8g567n8678fb8790678sd56756n456h8d4f5gn865648"
goldapi = GoldenAPI(jwt_token=JWT_TOKEN)
```

### 2. Retrieve unvalidated triple

There are a couple options for retrieving an unvalidated triple.

The first way we'd recommend is to retrieve a validation task from Golden's API. We'll have an endpoint that provides a queue of validation tasks.

You can validate the triple by creating a validation for the triple which will submit your validation (accept, reject) to the protocol.

```python
# Get unvalidated triple
data = goldapi.unvalidated_triple()["data"]
unvalidated_triple = data["unvalidatedTriple"]
unvalidated_triple
```

```
{'id': '2a2b80d5-b841-4723-b949-210a11500db3',
 'citationsByTripleId': {'nodes': [{'url': 'https://www.linkedin.com/search/results/all/?keywords=Miles%20Wolff'}]},
 'dateCreated': '2022-08-22T15:59:24.617456',
 'objectEntityId': None,
 'objectValue': '509.763.3584 x29455',
 'subjectId': 'd95ef4b0-e006-4203-bfb4-adbe99f63ce7',
 'userId': '0xppaf8ojpphfszeokh5eecmqbf82pelqm6f63jpjf',
 'predicateId': 'c090be24-6c35-45d8-8a81-32e57a3d48dd'}
```

### 4. Create validation

```python
from godel.schema import ValidationType

# Retrieve Choices for Validation
ValidationType
```

```
enum ValidationType {
  ACCEPTED
  REJECTED
  SKIPPED
}
```

```python
# Create validation with the triple id and your validation type
triple_id = unvalidated_triple["id"]
validation_type = "REJECTED"
triple_id
```

```
'2a2b80d5-b841-4723-b949-210a11500db3'
```

### WARNING: Running the code below may charge gas fees and stake testnet points with your wallet. You may lose testnet points by submitting incorrect data.

```python
# Create Validation
goldapi.create_validation(
    triple_id=triple_id,
    validation_type=validation_type
)
```

```python
```
