# Create Entities

A short guide to adding Entities with Minimum Disambiguation Triples(MDTs) to the Golden decentralized knowledge graph using the godel SDK. Please see the [Golden Protocol FAQ Guide](https://www.notion.so/goldenhq/Golden-Protocol-FAQ-78ae2357b9af44aeaa655cb1b1966ee4) and the [Adding Structured Data Guide](https://www.notion.so/goldenhq/Adding-Structured-Data-Guide-ae657337bf4f4e54ae4402df083c76ac) to learn more about Golden, data submission, and rewards.

Note: Attribution and eligibility for testnet points on triple submissions will be assigned by the earliest timestamped transaction.

## Prerequisite

[Godel Authentication](https://docs.golden.xyz/godel-python-sdk/authentication)

This guide requires you install Godel's data-tools.

You can do this with `pip install godel[data-tools].` This comes pre-installed if using the godel docker image.

### 1. Connect to Golden Web3 API

Let's connect the python wrapper to the Golden GraphQL API.

Make sure you ran through the prerequisites for this guide and have learned to authenticate and retrieve your JWT token in Godel.

```python
from godel import GoldenAPI

JWT_TOKEN = #YOUR_JWT_TOKEN_HERE
API_URL = "https://dapp.golden.xyz/graphql"
SANDBOX_URL = "https://sandbox.dapp.golden.xyz/graphql" # Use the sandbox API to test your submissions
goldapi = GoldenAPI(url=API_URL)
goldapi.set_jwt_token(jwt_token=JWT_TOKEN)
```

```python
import pandas as pd

# Test with search
search_results = goldapi.entity_search(name="Miles")
search_results_df = pd.DataFrame(search_results["data"]["entityByName"]["nodes"])
search_results_df
```

|   | id                                   | name        | description                                       | thumbnail                                         | goldenId | pathname                                     |
| - | ------------------------------------ | ----------- | ------------------------------------------------- | ------------------------------------------------- | -------- | -------------------------------------------- |
| 0 | d95ef4b0-e006-4203-bfb4-adbe99f63ce7 | Miles Wolff | Id nihil blanditiis eius fugit odit blanditiis... | https://cloudflare-ipfs.com/ipfs/Qmd3W5DuhgHir... | None     | /entity/d95ef4b0-e006-4203-bfb4-adbe99f63ce7 |

### 2. Get Predicates and Entity Types

You can run the code below to get the list of allowed predicates in the current schema, their ids, and their expected object data type.

```python
import pandas as pd

predicates = {}
for p in goldapi.predicates()["data"]["predicates"]["edges"]:
    p = p["node"]
    predicates[p["name"]] = {"id": p["id"], "objectType": p["objectType"]} 
predicates_df = pd.DataFrame(predicates).transpose()
predicates_df.head()
```

|                      | id                                   | objectType |
| -------------------- | ------------------------------------ | ---------- |
| CEO                  | 0a87e996-34b4-46ba-909a-70ab67b1f811 | ENTITY     |
| Email address        | 0efd0441-1ffc-4e30-8806-e58c434770c8 | STRING     |
| YouTube channel      | 12acb8fe-0573-4ca8-8cc1-180cc6ba3486 | ANY\_URI   |
| Total funding amount | 13a7e8b6-7270-4c99-81e9-9d752e0c295c | FLOAT      |
| Whitepaper           | 14fa743c-8161-42e8-a92f-5c29c70e87f8 | ANY\_URI   |

```python
templates = {}
for t in goldapi.templates()["data"]["templates"]["edges"]:
    t = t["node"]
    templates[t["entity"]["name"]] = {"id": t["id"], "entityId": t["entityId"], "entityDescription": t["entity"]["description"]} 
templates_df = pd.DataFrame(templates).transpose()
templates_df
```

|         | id                                   | entityId                             | entityDescription |
| ------- | ------------------------------------ | ------------------------------------ | ----------------- |
| Person  | 0dea0f27-ce0c-4b4f-8ddb-5ff6adb57e12 | 0c4e6054-5fd8-48a8-817c-f6611278f755 | None              |
| Company | 9553f193-a46a-4b46-9c0f-5287289644a6 | 0a9fcc89-e14b-47af-85c3-8465ca607c29 | None              |

### 3. Check for Duplicates

#### Source Data

We need source data on the entity you would like to submit. Once we have this defined, we should check for existing duplicates of this entity in the graph.

```python
name = "John Doe"
is_a = "0c4e6054-5fd8-48a8-817c-f6611278f755"  # Person Template Entity Id
ceo_of = "6086ed2c-3e45-4480-8ad6-aced06fac6fd"  # Golden Entity ID
email_address = "john.doe@example.com"
citation_urls = ["https://golden.com/wiki/johndoe"]
```

Calling the disambiguation function is necessary to create the entity in the next step, as the disambiguationCallId is required in the `CreateEntityInput` object.

```
john_doe_input = [
    {
        'Name': name,
        'Is a': is_a,
        'CEO of': ceo_of,
        'Email Address': email_address,
    }
]
response = goldapi.disambiguate_triples(twitter_example[0], with_diff=False)
disambiguationCallId = response['data']['disambiguateTriples']['disambiguationCallId']
response
```

```
{
   'data': {
      'disambiguateTriples': 
         {
            'errors': None,
            'disambiguationCallId': 'dcca2e44-8d9e-4edc-bcb6-48bb5fafc6bb',
            'entities': []
         }
   }
}
```

### 3. Create Entity

#### Create Entity Input

We're going to create the `CreateEntityInput`, which requires Minimum Disambiguated Triples (MDTs).

MDTs are the required triples you must submit along with the entity you want to create.

This helps us with disambiguation, deduplication, and arbitrary entity submissions with zero triples.

```python
from godel.schema import CreateEntityInput, StatementInputRecordInput

print(repr(CreateEntityInput))
StatementInputRecordInput
```

```
input CreateEntityInput {
  clientMutationId: String
  name: String!
  statements: [StatementInputRecordInput]
}

input StatementInputRecordInput {
  predicateId: UUID!
  objectValue: String
  objectEntityId: UUID
  citationUrls: [String]
  qualifiers: [QualifierInputRecordInput]
}
```

First, create your triples with the `StatementInputRecordInput`'s.

You'll notice that the "CEO of" statement is commented out. Without this, even if you have the email address statement, you will not be able to submit the entity since it does not fulfill the MDTs required. We are using `pandas` here to make working with data easier, but it is not a requirement.

Remove the comment-out of the "CEO of" statement to successfully submit the entity.

```python
# Create triples inputs
statements = []

# Add Template
statements.append(
    StatementInputRecordInput(
        predicate_id = predicates["Is a"]["id"],
        object_entity_id = is_a,
        citation_urls = [],
        qualifiers = [],
    )
)

# Add Name
statements.append(
    StatementInputRecordInput(
        predicate_id = predicates["Name"]["id"],
        object_value = name,
        citation_urls = [],
        qualifiers = [],
    )
)

# Add Email Address
statements.append(
    StatementInputRecordInput(
        predicate_id = predicates["Email Address"]["id"],
        object_value = email_address,
        citation_urls = [],
        qualifiers = [],
    )
)

# Add CEO of
# statements.append(
#     StatementInputRecordInput(
#         predicate_id = predicates["CEO of"]["id"],
#         object_entity_id = ceo_of,
#         citation_urls = [],
#         qualifiers = [],
#     )
# )
```

Now that you have the statements, you can create your `CreateEntityInput`.

```python
# Create Entity Input
create_entity_input = CreateEntityInput(
    statements = statements,
    disambiguation_call_id=disambiguationCallId
)
create_entity_input.__to_json_value__()
```

```
{'name': 'John Doe',
 'statements': [{'predicateId': '94a8d215-ce32-4379-b18e-2aebf0794882',
   'objectEntityId': '0c4e6054-5fd8-48a8-817c-f6611278f755',
   'citationUrls': ['https://golden.com/wiki/johndoe'],
   'qualifiers': []},
  {'predicateId': '0efd0441-1ffc-4e30-8806-e58c434770c8',
   'objectValue': 'john.doe@example.com',
   'citationUrls': ['https://golden.com/wiki/johndoe'],
   'qualifiers': []}]}
```

### WARNING: Running the code below may charge gas fees and stake testnet points with your wallet. You may lose testnet points by submitting incorrect data.

#### Submit entity data to protocol

```python
data = goldapi.create_entity(create_entity_input=create_entity_input)
data
```

```
{'data': {'createEntity': {'entity': {'__typename': 'Entity',
    'id': 'b58235c8-fdcf-4bc9-a6dc-e1749edc4a4f',
    'pathname': '/entity/b58235c8-fdcf-4bc9-a6dc-e1749edc4a4f',
    'name': 'John Doe',
    'description': None,
    'thumbnail': None,
    'goldenId': None,
    'isA': {'nodes': [{'id': '0c4e6054-5fd8-48a8-817c-f6611278f755',
       'name': 'Person'}]},
    'statementsBySubjectId': {'nodes': [{'__typename': 'Statement',
       'id': '1d4175cc-9390-4a78-bc6d-e5d09abd1562',
       'dateAccepted': None,
       'dateRejected': None,
       'userId': '0xb5c46393798dA8f4853BF9028d5c3F63C1a64EC8',
       'validationStatus': 'PENDING',
       'subject': {'id': 'b58235c8-fdcf-4bc9-a6dc-e1749edc4a4f',
        'pathname': '/entity/b58235c8-fdcf-4bc9-a6dc-e1749edc4a4f',
        'name': 'John Doe',
        'thumbnail': None},
       'predicate': {'id': '94a8d215-ce32-4379-b18e-2aebf0794882',
        'name': 'Is a',
        'description': '',
        'label': 'The entity type of this entity',
        'objectType': 'ENTITY',
        'showInInfobox': True},
       'objectValue': None,
       'objectEntity': {'id': '0c4e6054-5fd8-48a8-817c-f6611278f755',
        'pathname': '/entity-type/0c4e6054-5fd8-48a8-817c-f6611278f755',
        'name': 'Person',
        'thumbnail': None},
       'citationsByTripleId': {'nodes': []},
       'qualifiersBySubjectId': {'nodes': []}},
      {'__typename': 'Statement',
       'id': '3be03c2a-0e69-43a6-a606-70f6dd60925e',
       'dateAccepted': None,
       'dateRejected': None,
       'userId': '0xb5c46393798dA8f4853BF9028d5c3F63C1a64EC8',
       'validationStatus': 'PENDING',
       'subject': {'id': 'b58235c8-fdcf-4bc9-a6dc-e1749edc4a4f',
        'pathname': '/entity/b58235c8-fdcf-4bc9-a6dc-e1749edc4a4f',
        'name': 'John Doe',
        'thumbnail': None},
       'predicate': {'id': '3104de39-071c-47b8-86b4-d62ccc4a4fa6',
        'name': 'CEO of',
        'description': 'The organization this individual is charged with the management of – especially an independent legal entity such as a company or nonprofit institution. The CEO of an organization typically reports to the board of directors.',
        'label': 'Organization this person is CEO of.',
        'objectType': 'ENTITY',
        'showInInfobox': True},
       'objectValue': None,
       'objectEntity': {'id': '6086ed2c-3e45-4480-8ad6-aced06fac6fd',
        'pathname': '/entity/6086ed2c-3e45-4480-8ad6-aced06fac6fd',
        'name': 'Golden',
        'thumbnail': 'https://golden-media.s3.amazonaws.com/topics/5253ceef-b9f0-49c5-ae0e-e6a445078551.png'},
       'citationsByTripleId': {'nodes': []},
       'qualifiersBySubjectId': {'nodes': []}},
      {'__typename': 'Statement',
       'id': 'beaf1542-041c-48af-af0f-dcec31e975f4',
       'dateAccepted': None,
       'dateRejected': None,
       'userId': '0xb5c46393798dA8f4853BF9028d5c3F63C1a64EC8',
       'validationStatus': 'PENDING',
       'subject': {'id': 'b58235c8-fdcf-4bc9-a6dc-e1749edc4a4f',
        'pathname': '/entity/b58235c8-fdcf-4bc9-a6dc-e1749edc4a4f',
        'name': 'John Doe',
        'thumbnail': None},
       'predicate': {'id': '0efd0441-1ffc-4e30-8806-e58c434770c8',
        'name': 'Email Address',
        'description': 'The email address associated with this entity that is 1. public (it is openly known the entity is associated with this email address) and 2. official (the entity has formally associated themselves with this phone number by posting it on a website, social media link, etc. that they are in control of).',
        'label': 'Email address associated with an entity.',
        'objectType': 'STRING',
        'showInInfobox': True},
       'objectValue': 'john.doe@example.com',
       'objectEntity': None,
       'citationsByTripleId': {'nodes': []},
       'qualifiersBySubjectId': {'nodes': []}},
      {'__typename': 'Statement',
       'id': 'c1accc97-46c2-454a-b8c7-1b9b37a9e424',
       'dateAccepted': None,
       'dateRejected': None,
       'userId': '0xb5c46393798dA8f4853BF9028d5c3F63C1a64EC8',
       'validationStatus': 'PENDING',
       'subject': {'id': 'b58235c8-fdcf-4bc9-a6dc-e1749edc4a4f',
        'pathname': '/entity/b58235c8-fdcf-4bc9-a6dc-e1749edc4a4f',
        'name': 'John Doe',
        'thumbnail': None},
       'predicate': {'id': 'a27218b8-6a4d-47bb-95b6-5a55334fac1c',
        'name': 'Name',
        'description': '',
        'label': 'The primary name associated with this entity',
        'objectType': 'STRING',
        'showInInfobox': False},
       'objectValue': 'John Doe',
       'objectEntity': None,
       'citationsByTripleId': {'nodes': []},
       'qualifiersBySubjectId': {'nodes': []}}]}}}}
 }
```

```python
created_data_df = pd.DataFrame([data["data"]["createEntity"]["entity"]])
created_data_df
```

|   | \_\_typename | id                                   | pathname                                     | name     | description | thumbnail | goldenId | isA                                                | statementsBySubjectId                                |
| - | ------------ | ------------------------------------ | -------------------------------------------- | -------- | ----------- | --------- | -------- | -------------------------------------------------- | ---------------------------------------------------- |
| 0 | Entity       | 18998a05-d972-44cc-9d45-c75c9477c98d | /entity/18998a05-d972-44cc-9d45-c75c9477c98d | John Doe | None        | None      | None     | {'nodes': \[{'id': '0c4e6054-5fd8-48a8-817c-f66... | {'nodes': \[{'\_\_typename': 'Statement', 'id': '... |

### 4. View Data on dApp

```python
created_entity_id = data["data"]["createEntity"]["entity"]["id"]
created_entity_id
link = f"https://dapp.golden.xyz/entity/{created_entity_id}"
link
```
