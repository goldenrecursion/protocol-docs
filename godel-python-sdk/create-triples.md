# Create Triples

A short guide to adding triples to the Golden decentralized knowledge graph using the godel SDK. Please see the [Golden Protocol FAQ Guide](https://www.notion.so/goldenhq/Golden-Protocol-FAQ-78ae2357b9af44aeaa655cb1b1966ee4) and the [Adding Structured Data Guide](https://www.notion.so/goldenhq/Adding-Structured-Data-Guide-ae657337bf4f4e54ae4402df083c76ac) to learn more about Golden, data submission, and rewards.

Note: Attribution and eligibility for testnet points on triple submissions will be assigned by the earliest timestamped transaction.

## Prerequisites

[Godel Authentication](https://docs.golden.xyz/godel-python-sdk/authentication)

This guide requires you install Godel's data-tools.

You can do this with `pip install godel[data-tools].` This comes pre-installed if using the godel docker image.

### 1. Connect to Golden Web3 API

Let's connect the python wrapper to the Golden GraphQL API.

Make sure you have satisfied the prerequisites for this guide and have learned to authenticate and retrieve your JWT token in Godel.

```python
from godel import GoldenAPI

JWT_TOKEN = "ey098sd908v79899789877986567967845jh567hj5679568df678678daf6786789s569ghm567457hm8g567n8678fb8790678sd56756n456h8d4f5gn865648"
goldapi = GoldenAPI(jwt_token=JWT_TOKEN)
```

Test that you can hit the API with `entity_search()`, and we'll save the results so we can use the resulting entity as our subject entity for this guide. We are using `pandas` here to make working with data easier, but it is not a requirement. &#x20;

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

### 2. Get Predicates

You can run the code below to get the list of accepted predicates available for the protocol.

```python
import pandas as pd

predicates = {}
for p in goldapi.predicates()["data"]["predicates"]["edges"]:
    p = p["node"]
    predicates[p["name"]] = {"id": p["id"], "objectType": p["objectType"]} 
predicates_df = pd.DataFrame(predicates).transpose()
predicates_df
```

|                        | id                                   | objectType |
| ---------------------- | ------------------------------------ | ---------- |
| CEO                    | 0a87e996-34b4-46ba-909a-70ab67b1f811 | ENTITY     |
| Email address          | 0efd0441-1ffc-4e30-8806-e58c434770c8 | STRING     |
| YouTube channel        | 12acb8fe-0573-4ca8-8cc1-180cc6ba3486 | ANY\_URI   |
| Total funding amount   | 13a7e8b6-7270-4c99-81e9-9d752e0c295c | FLOAT      |
| Whitepaper             | 14fa743c-8161-42e8-a92f-5c29c70e87f8 | ANY\_URI   |
| Full address           | 1551ee2a-f6a0-4a4b-b322-d98d3a696cf3 | STRING     |
| Pinterest              | 1d7d64c5-c4a1-4889-91c4-2d2da0424dcc | ANY\_URI   |
| Source Code            | 1e49b96d-f641-4226-91f0-ed42e6de742e | ANY\_URI   |
| Contact URL            | 27897e2f-5d08-40fe-904d-0b0647fa2ff4 | ANY\_URI   |
| CEO of                 | 3104de39-071c-47b8-86b4-d62ccc4a4fa6 | ENTITY     |
| Doctoral Thesis URL    | 33461e27-5454-43c3-b300-88c02a96c280 | ANY\_URI   |
| Reddit URL             | 36d1a264-da26-4a1a-8f0e-726543749a5f | ANY\_URI   |
| Website                | 42cb158b-e836-45ed-9b56-034668b8f05a | ANY\_URI   |
| Start time             | 4b4ff1c9-a053-4bc3-87ef-0713453f9992 | DATE\_TIME |
| Coinmarketcap URL      | 5387126a-fa27-4a42-8c7f-bf813a6a893d | ANY\_URI   |
| Pitchbook URL          | 5bbcbd49-c255-4a6b-b84a-dc076849650d | ANY\_URI   |
| Thumbnail              | 60627261-4e6c-4ebf-8879-914576ade417 | ANY\_URI   |
| Telegram               | 68d490c8-d8d3-4efe-9670-390df48e1ad6 | ANY\_URI   |
| End time               | 6b95b113-e331-41bb-8e31-45b198a41ea8 | DATE\_TIME |
| Founder                | 71ad3d9e-e211-472b-a16d-861737c57ecd | ENTITY     |
| Medium                 | 71f46d7f-6667-4600-90bf-eb82fbba8e17 | ANY\_URI   |
| Tiktok                 | 7e593c0c-457a-464d-9dd2-8e1fc5a8b116 | ANY\_URI   |
| Angellist URL          | 7f15d788-5df1-4ff3-a5e5-4c9e8e2c57af | ANY\_URI   |
| Description            | 7f3869c1-7dc9-4486-9045-6bade487a49d | STRING     |
| Linkedin URL           | 8c4d6279-199f-4e46-9ef7-8702bad1e152 | ANY\_URI   |
| Apple App Store Link   | 92ae90d8-d4f6-476b-9409-89b7d1b846c0 | ANY\_URI   |
| Is a                   | 94a8d215-ce32-4379-b18e-2aebf0794882 | ENTITY     |
| Twitter URL            | 9934d828-963f-403a-a0da-7a52e0224ef5 | ANY\_URI   |
| Date of incorporation  | 9cb6d628-a0f8-48b0-9828-253596b6ad00 | DATE\_TIME |
| Name                   | a27218b8-6a4d-47bb-95b6-5a55334fac1c | STRING     |
| Wikidata ID            | b996dfba-6f3b-458e-bb98-61939160fd88 | STRING     |
| Golden ID              | bb463b8b-b76c-4f6a-9726-65ab5730b69b | INTEGER    |
| Phone number           | c090be24-6c35-45d8-8a81-32e57a3d48dd | STRING     |
| Discord URL            | c094b0f7-d34c-4f5e-86b3-801da1c82091 | ANY\_URI   |
| Instagram              | db592366-1c4c-4087-821e-44699ddd29b6 | ANY\_URI   |
| Github URL             | e3d0cfb4-3ec1-4ef2-ae08-93fa07aa27dc | ANY\_URI   |
| Founder of             | e4f94b98-c56a-4bd2-a9fd-5fd11603e7e8 | ENTITY     |
| Community Forum        | f348c532-bffd-4ad1-b79c-34258d05c1cd | ANY\_URI   |
| Founded date           | fa1a5ac7-480c-4e44-a545-b0f3dd9d24bf | DATE\_TIME |
| Facebook URL           | fa39c1f2-bf06-45e9-8995-da919472deb8 | ANY\_URI   |
| Crunchbase URL         | facb73aa-82db-45ff-bd87-5ce7983d8ca2 | ANY\_URI   |
| Google Play Store Link | fb06b903-52af-4a79-9126-f2589c2ca881 | ANY\_URI   |

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
