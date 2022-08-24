# Create Entities

A short guide to adding Entities with Minimum Disambiguation Triples (MDTs) to the Golden decentralized knowledge graph using the godel SDK. Please see the [Golden Protocol FAQ Guide](https://www.notion.so/goldenhq/Golden-Protocol-FAQ-78ae2357b9af44aeaa655cb1b1966ee4) and the [Adding Structured Data Guide](https://www.notion.so/goldenhq/Adding-Structured-Data-Guide-ae657337bf4f4e54ae4402df083c76ac) to learn more about Golden, data submission, and rewards.

Note: Attribution and eligibility for testnet points on triple submissions will be assigned by the earliest timestamped transaction.

## Prerequisite

[Godel Authentication](https://docs.golden.xyz/godel-python-sdk/authentication)

This guide requires you install Godel's data-tools.

You can do this with `pip install godel[data-tools].`This comes pre-installed if using the godel docker image.

### 1. Connect to Golden Web3 API

Let's connect the python wrapper to the Golden GraphQL API.

Make sure you ran through the prerequisites for this guide and have learned to authenticate and retrieve your JWT token in Godel.

```python
from godel import GoldenAPI

JWT_TOKEN = "ey098sd908v79899789877986567967845jh567hj5679568df678678daf6786789s569ghm567457hm8g567n8678fb8790678sd56756n456h8d4f5gn865648"
goldapi = GoldenAPI(jwt_token=JWT_TOKEN)
```

&#x20;We are using `pandas` here to make working with data easier, but it is not a requirement. &#x20;

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

### 2. Get Predicates and Templates

You can run the code below to get the list of accepted predicates into the knowledge graph, its data types, and submit templates for different kinds of entities.

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

### 4. Create Entity

#### Source Data

We need source data on the entity you would like to submit

```python
name = "John Doe"
is_a = "0c4e6054-5fd8-48a8-817c-f6611278f755"  # Persion Template Entity Id
ceo_of = "20ab9281-fd5f-4717-ab73-ecd24fff66fe"  # Huel and Sons Entity ID
email_address = "john.doe@example.com"
citation_urls = ["https://golden.com/wiki/johndoe"]
```

#### Create Entity Input

We're going to create the `CreateEntityInput`, which requires Minimum Disambiguated Triples (MDTs).

MDTs specify the required triples you must submit along with the entity you want to create.

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

You'll notice that the "CEO of" statement is commented out. Without this, even if you have the email address statement, you will not be able to submit the entity since it does not fulfill the MDTs required.

Remove the comment-out of the "CEO of" statement to successfully submit the entity.

```python
# Create triples inputs
statements = []

# Add Template
statements.append(
    StatementInputRecordInput(
        predicate_id = predicates["Is a"]["id"],
        object_entity_id = is_a,
        citation_urls = citation_urls,
        qualifiers = [],
    )
)

# Add Email Address
statements.append(
    StatementInputRecordInput(
        predicate_id = predicates["Email address"]["id"],
        object_value = email_address,
        citation_urls = citation_urls,
        qualifiers = [],
    )
)

# Add CEO of
# statements.append(
#     StatementInputRecordInput(
#         predicate_id = predicates["CEO of"]["id"],
#         object_entity_id = ceo_of,
#         citation_urls = citation_urls,
#         qualifiers = [],
#     )
# )
```

Now that you have the statements, you can create your `CreateEntityInput`.

```python
# Create Entity Input
create_entity_input = CreateEntityInput(
    name = name,
    statements = statements
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
GraphQL query failed with 1 errors

{'errors': [{'extensions': {'messages': [], 'exception': {}},
   'message': 'All required statements must be provided:\n- entity type "person" must have a statement from at least one of these predicates: "website", "angelListUrl", "linkedInUrl", "founderOf", "ceoOf"',
   'locations': [{'line': 2, 'column': 1}],
   'path': ['createEntity'],
   'stack': ['Error: All required statements must be provided:',
    '- entity type "person" must have a statement from at least one of these predicates: "website", "angelListUrl", "linkedInUrl", "founderOf", "ceoOf"',
    '    at checkMinimumRequiredStatements (/home/andrew/golden/dapp/app/db/entityMinimumRequiredStatements.ts:74:11)',
    '    at createEntityWrapper (/home/andrew/golden/dapp/db/graphql/plugins/CreateEntityPlugin.server.ts:17:33)',
    '    at resolve (/home/andrew/golden/dapp/node_modules/graphile-utils/src/makeWrapResolversPlugin.ts:180:18)',
    '    at resolve (/home/andrew/golden/dapp/node_modules/@graphile/operation-hooks/lib/OperationHooksCorePlugin.js:122:38)',
    '    at runMicrotasks (<anonymous>)',
    '    at processTicksAndRejections (node:internal/process/task_queues:96:5)',
    '    at async /home/andrew/golden/dapp/node_modules/postgraphile/src/postgraphile/withPostGraphileContext.ts:227:16',
    '    at async withAuthenticatedPgClient (/home/andrew/golden/dapp/node_modules/postgraphile/src/postgraphile/withPostGraphileContext.ts:169:20)',
    '    at async /home/andrew/golden/dapp/node_modules/postgraphile/src/postgraphile/http/createPostGraphileHttpRequestHandler.ts:926:26',
    '    at async Promise.all (index 0)']}],
 'data': {'createEntity': None}}
```

```python
created_data_df = pd.DataFrame([data["data"]["createEntity"]["entity"]])
created_data_df
```

|   | \_\_typename | id                                   | pathname                                     | name     | description | thumbnail | goldenId | isA                                                | statementsBySubjectId                                |
| - | ------------ | ------------------------------------ | -------------------------------------------- | -------- | ----------- | --------- | -------- | -------------------------------------------------- | ---------------------------------------------------- |
| 0 | Entity       | 18998a05-d972-44cc-9d45-c75c9477c98d | /entity/18998a05-d972-44cc-9d45-c75c9477c98d | John Doe | None        | None      | None     | {'nodes': \[{'id': '0c4e6054-5fd8-48a8-817c-f66... | {'nodes': \[{'\_\_typename': 'Statement', 'id': '... |

### 6. View Data on dApp

```python
created_entity_id = data["data"]["createEntity"]["entity"]["id"]
created_entity_id
link = f"https://dapp.golden.xyz/entity/{created_entity_id}"
link
```

```
'https://dapp.golden.xyz/entity/18998a05-d972-44cc-9d45-c75c9477c98d'
```
