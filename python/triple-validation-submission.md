# Triple Validation Submission

### 1. Connect to Golden Web3 API

Let's connect the python wrapper to the Golden GraphQL API.

You can run your basic queries against the API, but will be restricted from mutations and queries that require you to connect or authenticate your wallet.

You have to specify your wallet address and private key.

```python
local_addresses = [
    {"0x019235718q0879saf908d87gd8gsdg": "987g76sa6g9d6gs98ad76gg7sda687gd6gd6sadg867790n8vc"}
]

```

### 2. Authenticate and set JWT

This will set the JWT token key in your GoldenAPI object so you should have permission to hit all endpoints after running the below.

```python
from dapp_api import GoldenAPI
from web3 import Web3

DAPP_URL = "https://dapp.golden.xyz/graphql"

gapi_connections = []
for user_id, private_key in local_addresses:
    url = DAPP_URL
    gapi = GoldenAPI(url=url)
    user_id = Web3.toChecksumAddress(user_id)
    
    # Retrieve one-off nonce from GraphQL API
    message_response = gapi.get_authentication_message(user_id=user_id)
    message_response

    # Sign and verify nonce with your wallet's private key (KEEP THIS SECURE)
    from web3.auto import w3
    from eth_account.messages import encode_defunct

    message_string = message_response["data"]["getAuthenticationMessage"]["string"]
    message = encode_defunct(text=message_string)
    signed_message = w3.eth.account.sign_message(message, private_key=private_key)
    signature = signed_message.signature.hex()

    # Authenticate with Golden's API and you'll recieve a jwt bearer token
    auth_response = gapi.authenticate(
        user_id=user_id,
        signature=signature
    )

    jwt_token = auth_response["data"]["authenticate"]["jwtToken"]
    print(auth_response)

    # Set JWT token to verify your wallet/role and unlock permissions to the rest of the API
    gapi.set_jwt_token(jwt_token=jwt_token)

    print("JWT TOKEN: ", jwt_token)
    
    gapi_connections.append(gapi)
```

```
{'data': {'authenticate': {'jwtToken': 'eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJyb2xlIjoidXNlcl9yb2xlIiwidXNlcl9pZCI6IjB4ZjM5RmQ2ZTUxYWFkODhGNkY0Y2U2YUI4ODI3Mjc5Y2ZmRmI5MjI2NiIsImV4cCI6MTY1NDg4NDc3MiwiaWF0IjoxNjU0NzExOTcyLCJhdWQiOiJwb3N0Z3JhcGhpbGUiLCJpc3MiOiJwb3N0Z3JhcGhpbGUifQ.F6O5EkWaUpn78u9hC1iH-V19VdWUUtltqfzT4WF2-Ag'}}}
JWT TOKEN:  eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJyb2xlIjoidXNlcl9yb2xlIiwidXNlcl9pZCI6IjB4ZjM5RmQ2ZTUxYWFkODhGNkY0Y2U2YUI4ODI3Mjc5Y2ZmRmI5MjI2NiIsImV4cCI6MTY1NDg4NDc3MiwiaWF0IjoxNjU0NzExOTcyLCJhdWQiOiJwb3N0Z3JhcGhpbGUiLCJpc3MiOiJwb3N0Z3JhcGhpbGUifQ.F6O5EkWaUpn78u9hC1iH-V19VdWUUtltqfzT4WF2-Ag
```

```python
results = gapi.entity_search(name="Bernadine")
results
```

```
{'data': {'entityByName': {'nodes': []}}}
```

### 3. Get Predicates and Templates

Can't really create entities and triples without knowing what predicates and templates exist.

```python
import pandas as pd

predicates = {}
for p in gapi.predicates()["data"]["predicates"]["edges"]:
    p = p["node"]
    predicates[p["name"]] = {"id": p["id"], "objectType": p["objectType"]} 
predicates_df = pd.DataFrame(predicates).transpose()
predicates_df
```



|                        | id                                   | objectType |
| ---------------------- | ------------------------------------ | ---------- |
| CEO                    | 0a87e996-34b4-46ba-909a-70ab67b1f811 | ENTITY     |
| YouTube channel        | 12acb8fe-0573-4ca8-8cc1-180cc6ba3486 | ANY\_URI   |
| Total funding amount   | 13a7e8b6-7270-4c99-81e9-9d752e0c295c | FLOAT      |
| Whitepaper             | 14fa743c-8161-42e8-a92f-5c29c70e87f8 | ANY\_URI   |
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
| Email                  | 86a5c5d2-6d05-4788-9984-0efae5384784 | STRING     |
| Linkedin URL           | 8c4d6279-199f-4e46-9ef7-8702bad1e152 | ANY\_URI   |
| Apple App Store Link   | 92ae90d8-d4f6-476b-9409-89b7d1b846c0 | ANY\_URI   |
| Is a                   | 94a8d215-ce32-4379-b18e-2aebf0794882 | ENTITY     |
| Twitter URL            | 9934d828-963f-403a-a0da-7a52e0224ef5 | ANY\_URI   |
| Name                   | a27218b8-6a4d-47bb-95b6-5a55334fac1c | STRING     |
| Golden ID              | bb463b8b-b76c-4f6a-9726-65ab5730b69b | INTEGER    |
| Phone number           | c090be24-6c35-45d8-8a81-32e57a3d48dd | STRING     |
| Instagram              | db592366-1c4c-4087-821e-44699ddd29b6 | ANY\_URI   |
| Github URL             | e3d0cfb4-3ec1-4ef2-ae08-93fa07aa27dc | ANY\_URI   |
| Founder of             | e4f94b98-c56a-4bd2-a9fd-5fd11603e7e8 | ENTITY     |
| Community Forum        | f348c532-bffd-4ad1-b79c-34258d05c1cd | ANY\_URI   |
| Facebook URL           | fa39c1f2-bf06-45e9-8995-da919472deb8 | ANY\_URI   |
| Crunchbase URL         | facb73aa-82db-45ff-bd87-5ce7983d8ca2 | ANY\_URI   |
| Google Play Store Link | fb06b903-52af-4a79-9126-f2589c2ca881 | ANY\_URI   |

```python
templates = {}
for t in gapi.templates()["data"]["templates"]["edges"]:
    t = t["node"]
    templates[t["entity"]["name"]] = {"id": t["id"], "entityId": t["entityId"], "entityDescription": t["entity"]["description"]} 
pd.DataFrame(templates).transpose()
```



|         | id                                   | entityId                             | entityDescription |
| ------- | ------------------------------------ | ------------------------------------ | ----------------- |
| Person  | 0dea0f27-ce0c-4b4f-8ddb-5ff6adb57e12 | 0c4e6054-5fd8-48a8-817c-f6611278f755 | None              |
| Company | 9553f193-a46a-4b46-9c0f-5287289644a6 | 0a9fcc89-e14b-47af-85c3-8465ca607c29 | None              |

#### Validate Data

```python
from tqdm import tqdm
from random import choice

num_unvalidated_triples = 1000
for i in tqdm(range(num_unvalidated_triples)):
    data = gapi.unvalidated_triple()
    #print(data)
    for gapi_i in gapi_connections:
        validation_options = ["ACCEPTED", "SKIPPED", "REJECTED"]
        validation_type = choice(validation_options)
        created_validation = gapi_i.create_validation(triple_id=data["data"]["unvalidatedTriple"]["id"], validation_type=validation_type)
        #print(created_validation)
        break
```

