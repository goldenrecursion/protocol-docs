# Verification

Here is a short guide to verifying triples on the Golden protocol. Before casting votes, be sure to read the [triple-verification-guide.md](../../protocol/guides/triple-verification-guide.md "mention").

## Prerequisites

* **Godel** [authentication.md](../../godel-python-sdk/authentication.md "mention")

### 1. Connect to Golden Protocol API

Let's connect the python wrapper to the Golden GraphQL API.

Make sure you ran through the prerequisites for this guide and have learned to authenticate and retrieve your JWT token.&#x20;

```python
from godel import GoldenAPI

JWT_TOKEN = #YOUR_JWT_TOKEN_HERE
API_URL = "https://dapp.golden.xyz/graphql"
SANDBOX_URL = "https://sandbox.dapp.golden.xyz/graphql" # Use the sandbox API to test your submissions
goldapi = GoldenAPI(url=API_URL)
goldapi.set_jwt_token(jwt_token=JWT_TOKEN)
```

### 2. Retrieve unverified triple

To retrieve a verification task from the verification queue use the `unvalidated_triple()`call. This will return a single unverified triple that has been assigned to your wallet. &#x20;

```python
# Get an unvalidated triple
data = goldapi.unvalidated_triple()["data"]
unvalidated_triple = data["triple"]
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

### 4. Cast verification vote

You can verify the triple by creating a validation mutation with your selected response: `ACCEPTED`, `REJECTED`, or `SKIPPED`. You can inspect the validation types using the protocol schema.

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

#### WARNING: Running the code below may charge gas fees and stake testnet points with your wallet. You may lose testnet points by submitting incorrect data.

```python
# Create Validation
goldapi.create_validation(
    triple_id=triple_id,
    validation_type=validation_type
)
```

```python
```
