# Create Triples

A short guide to adding triples to the Golden decentralized knowledge graph using the godel SDK. Please see the [Golden Protocol FAQ Guide](https://www.notion.so/goldenhq/Golden-Protocol-FAQ-78ae2357b9af44aeaa655cb1b1966ee4) and the [Adding Structured Data Guide](https://www.notion.so/goldenhq/Adding-Structured-Data-Guide-ae657337bf4f4e54ae4402df083c76ac) to learn more about Golden, data submission, and rewards.

Note: Attribution and eligibility for testnet points on triple submissions will be assigned by the earliest timestamped transaction.

## Prerequisite

[Authentication](https://docs.golden.xyz/guides/authentication)

[Godel Authentication](https://docs.golden.xyz/godel-python-sdk/authentication)

This guide requires you install Godel's data-tools.

You can do this with `pip install godel[data-tools]` and comes pre-installed if using the godel docker image.

### 1. Connect to Golden Web3 API

Let's connect the python wrapper to the Golden GraphQL API.

Make sure you ran through the prerequisites for this guide and have learned to authenticate and retrieve your JWT token in Godel.

```python
from godel import GoldenAPI

JWT_TOKEN = "ey098sd908v79899789877986567967845jh567hj5679568df678678daf6786789s569ghm567457hm8g567n8678fb8790678sd56756n456h8d4f5gn865648"
goldapi = GoldenAPI(jwt_token=JWT_TOKEN)
```

Test that you can hit the API with `entity_search()`, and we'll save the results so we can use the resulting entity as our subject entity for this guide.

```python
import pandas as pd

# Test with search
search_results = goldapi.entity_search(name="Miles")
search_results_df = pd.DataFrame(search_results["data"]["entityByName"]["nodes"])
search_results_df
```

.dataframe tbody tr th:only-of-type { vertical-align: middle; } .dataframe tbody tr th { vertical-align: top; } .dataframe thead th { text-align: right; }

|   | id                                   | name        | description                                       | thumbnail                                         | goldenId | pathname                                     |
| - | ------------------------------------ | ----------- | ------------------------------------------------- | ------------------------------------------------- | -------- | -------------------------------------------- |
| 0 | d95ef4b0-e006-4203-bfb4-adbe99f63ce7 | Miles Wolff | Id nihil blanditiis eius fugit odit blanditiis... | https://cloudflare-ipfs.com/ipfs/Qmd3W5DuhgHir... | None     | /entity/d95ef4b0-e006-4203-bfb4-adbe99f63ce7 |

### 2. Get Predicates

You can run the code below to get the list of accepted predicates available for the protocol.

```python
import pandas as pd

predicates = {}
for p in goldapi.predicates()["data"]["predicates"]["edges"]:
    p = p["node"]
    predicates[p["name"]] = {"id": p["id"], "objectType": p["objectType"]} 
predicates_df = pd.DataFrame(predicates).transpose()
predicates_df.head()
```

.dataframe tbody tr th:only-of-type { vertical-align: middle; } .dataframe tbody tr th { vertical-align: top; } .dataframe thead th { text-align: right; }

|                      | id                                   | objectType |
| -------------------- | ------------------------------------ | ---------- |
| CEO                  | 0a87e996-34b4-46ba-909a-70ab67b1f811 | ENTITY     |
| Email address        | 0efd0441-1ffc-4e30-8806-e58c434770c8 | STRING     |
| YouTube channel      | 12acb8fe-0573-4ca8-8cc1-180cc6ba3486 | ANY\_URI   |
| Total funding amount | 13a7e8b6-7270-4c99-81e9-9d752e0c295c | FLOAT      |
| Whitepaper           | 14fa743c-8161-42e8-a92f-5c29c70e87f8 | ANY\_URI   |

### 3. Create Triple

First, view the `CreateStatementInput` object schema.

```python
from godel.schema import CreateStatementInput, StatementInputRecordInput

CreateStatementInput
```

```
input CreateStatementInput {
  clientMutationId: String
  subjectId: UUID!
  predicateId: UUID!
  objectValue: String
  objectEntityId: UUID
  citationUrls: [String]
  qualifiers: [QualifierInputRecordInput]
}
```

Now you can input your triple data. Make sure to check the predicate object type to ensure your object entity ID or object value matches the object type.

#### Triple with Object Value Example

```python
# Triple data
subject_id = search_results_df["id"][0]  # Miles Wolff
predicate_id = predicates_df["id"][1]  # Email Address
object_value = "guide@example.com"

# Create statement input API
create_statement_input = CreateStatementInput(
    subject_id=subject_id,
    predicate_id=predicate_id,
    object_value=object_value,
    citation_urls=["https://golden.com"],
)
```

### WARNING: Running the code below may charge gas fees and stake testnet points with your wallet. You may lose testnet points by submitting incorrect data.

```python
data = goldapi.create_statement(create_statement_input=create_statement_input)
data
```

```
{'data': {'createStatement': {'statement': {'__typename': 'Statement',
    'id': '76d696d2-5295-4891-ae5a-3efdd9e5c084',
    'dateAccepted': None,
    'dateRejected': None,
    'userId': '0xf39Fd6e51aad88F6F4ce6aB8827279cffFb92266',
    'validationStatus': 'PENDING',
    'subject': {'id': 'd95ef4b0-e006-4203-bfb4-adbe99f63ce7',
     'pathname': '/entity/d95ef4b0-e006-4203-bfb4-adbe99f63ce7',
     'name': 'Miles Wolff',
     'thumbnail': 'https://cloudflare-ipfs.com/ipfs/Qmd3W5DuhgHirLHGVixi6V76LhCkZUz6pnFt5AJBiyvHye/avatar/1170.jpg'},
    'predicate': {'id': '0efd0441-1ffc-4e30-8806-e58c434770c8',
     'name': 'Email address',
     'description': 'The email address associated with this entity that is 1. public (it is openly known the entity is associated with this email address) and 2. official (the entity has formally associated themselves with this phone number by posting it on a website, social media link, etc. that they are in control of).',
     'label': 'Email address associated with an entity.',
     'objectType': 'STRING',
     'showInInfobox': True},
    'objectValue': 'guide@example.com',
    'objectEntity': None,
    'citationsByTripleId': {'nodes': [{'url': 'https://golden.com'}]},
    'qualifiersBySubjectId': {'nodes': []}}}}}
```

#### Triple with Object Entity ID Example

```python
# Triple data
subject_id = search_results_df["id"][0]  # Miles Wolff
predicate_id = "e4f94b98-c56a-4bd2-a9fd-5fd11603e7e8"  # CEO Of predicate
object_entity_id = "20ab9281-fd5f-4717-ab73-ecd24fff66fe"  # Huel and Sons Entity ID

# Create statement input API
create_statement_input = CreateStatementInput(
    subject_id=subject_id,
    predicate_id=predicate_id,
    object_entity_id=object_entity_id,
    citation_urls=["https://golden.com"],
)
```

### WARNING: Running the code below may charge gas fees and stake testnet points with your wallet. You may lose testnet points by submitting incorrect data.

```python
data = goldapi.create_statement(create_statement_input=create_statement_input)
data
```

```
{'data': {'createStatement': {'statement': {'__typename': 'Statement',
    'id': '07d779a7-1f2c-4d29-a7d0-94e2decf6ad6',
    'dateAccepted': None,
    'dateRejected': None,
    'userId': '0xf39Fd6e51aad88F6F4ce6aB8827279cffFb92266',
    'validationStatus': 'PENDING',
    'subject': {'id': 'd95ef4b0-e006-4203-bfb4-adbe99f63ce7',
     'pathname': '/entity/d95ef4b0-e006-4203-bfb4-adbe99f63ce7',
     'name': 'Miles Wolff',
     'thumbnail': 'https://cloudflare-ipfs.com/ipfs/Qmd3W5DuhgHirLHGVixi6V76LhCkZUz6pnFt5AJBiyvHye/avatar/1170.jpg'},
    'predicate': {'id': 'e4f94b98-c56a-4bd2-a9fd-5fd11603e7e8',
     'name': 'Founder of',
     'description': '',
     'label': 'Entity that a person founded.',
     'objectType': 'ENTITY',
     'showInInfobox': True},
    'objectValue': None,
    'objectEntity': {'id': '20ab9281-fd5f-4717-ab73-ecd24fff66fe',
     'pathname': '/entity/20ab9281-fd5f-4717-ab73-ecd24fff66fe',
     'name': 'Huel and Sons',
     'thumbnail': 'http://loremflickr.com/90/90/business'},
    'citationsByTripleId': {'nodes': [{'url': 'https://golden.com'}]},
    'qualifiersBySubjectId': {'nodes': []}}}}}
```

```python
```
