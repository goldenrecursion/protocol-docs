# Create Entities and Triples

A short guide to adding data to the Golden decentralized knowledge graph using the `godel` SDK. Please see the [Golden Protocol FAQ Guide](https://www.notion.so/goldenhq/Golden-Protocol-FAQ-78ae2357b9af44aeaa655cb1b1966ee4) and the [Adding Structured Data Guide](https://www.notion.so/goldenhq/Adding-Structured-Data-Guide-ae657337bf4f4e54ae4402df083c76ac) to learn more about Golden, data submission, and rewards.&#x20;

> Note: Attribution and eligibility for testnet points on triple submissions will be assigned by the earliest timestamped transaction. &#x20;

### 1. Connect to Golden Web3 API

Let's connect the python wrapper to the Golden GraphQL API.

You can run your basic queries against the API without authenticating, but will be restricted from mutations and certain queries until you connect using your `USER_ID` (your wallet address) and `PRIVATE_KEY` (wallet secret).

{% hint style="info" %}
If you encounter errors with connection, ensure that the wallet being used has been connected to your profile at [Golden.com](https://golden.com) and that you have submitted at least one triple.
{% endhint %}

```python
USER_ID = "0x878sd7b9n90n9ew8e8ew8er9r9ter98bb"
PRIVATE_KEY = "987b67b679987osdafbkj987231ijhdfv98kj4i987by71oiuf8vusda98aiou98"
```

```python
from godel import GoldenAPI

goldapi = GoldenAPI()
```

### 2. Authenticate and set JWT

This will set the JWT token key in your GoldenAPI object so you should have permission to hit all endpoints after running the below.

This step requires installation of [Web3.py](https://github.com/ethereum/web3.py)

You can do this with `pip install godel[web3]` and comes pre-installed if using the godel docker image.

```python
# Retrieve one-off nonce from GraphQL API
message_response = goldapi.get_authentication_message(user_id=USER_ID)
message_response

# Sign and verify nonce with your wallet's private key (KEEP THIS SECURE)
from web3.auto import w3
from eth_account.messages import encode_defunct

message_string = message_response["data"]["getAuthenticationMessage"]["string"]
message = encode_defunct(text=message_string)
signed_message = w3.eth.account.sign_message(message, private_key=PRIVATE_KEY)
signature = signed_message.signature.hex()

# Authenticate with Golden's API and you'll recieve a jwt bearer token
auth_response = goldapi.authenticate(
    user_id=USER_ID,
    signature=signature
)

jwt_token = auth_response["data"]["authenticate"]["jwtToken"]

# Set JWT token to verify your wallet/role and unlock permissions to the rest of the API
goldapi.set_jwt_token(jwt_token=jwt_token)

print("JWT TOKEN: ", jwt_token)
```

```
JWT TOKEN:  eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJyb2xlIjoidXNlcl9yb2xlIiwidXNlcl9pZCI6IjB4ZDg2YjUzNEMwMjlBYWY4NDI3MmEzYzcyNUY5NDA4ZTAzREVGNGI4MCIsImV4cCI6MTY1Njc0NDIyMiwiaWF0IjoxNjU2NTcxNDI0LCJhdWQiOiJwb3N0Z3JhcGhpbGUiLCJpc3MiOiJwb3N0Z3JhcGhpbGUifQ.e7zPPoU2MekR9Mw7CzaEm4h7Ibk7VenwEFBemew43hU
```

```python
import pandas as pd
# Test with search
results = goldapi.entity_search(name="Golden")
results_df = pd.DataFrame(results["data"]["entityByName"]["nodes"])
results_df
```

|    | id                                   | name                                       | description                                       | thumbnail                                         | goldenId | pathname                                     |
| -- | ------------------------------------ | ------------------------------------------ | ------------------------------------------------- | ------------------------------------------------- | -------- | -------------------------------------------- |
| 0  | 5c0d11ad-b6e7-4d08-a0f1-6a224d93f3a1 | Golden                                     | Golden is a company started in San Francisco, ... | https://golden-media.s3.amazonaws.com/topics/5... | None     | /entity/5c0d11ad-b6e7-4d08-a0f1-6a224d93f3a1 |
| 1  | 7baeabfe-5abe-4f91-a280-1a8d238544c2 | Golden State Warriors                      | The Golden State Warriors of the National Bask... | https://golden-storage-production.s3.amazonaws... | 34936    | /entity/7baeabfe-5abe-4f91-a280-1a8d238544c2 |
| 2  | 63d7e9ed-3d0c-429b-9fc4-4a6669f62426 | Golden Eagle Award (Russia)                | Russian film and television award                 | https://golden-storage-production.s3.amazonaws... | 838491   | /entity/63d7e9ed-3d0c-429b-9fc4-4a6669f62426 |
| 3  | 3bb86c99-f521-48fa-ae03-7f4630f64767 | Golden Star Resources                      |                                                   | None                                              | 2273691  | /entity/3bb86c99-f521-48fa-ae03-7f4630f64767 |
| 4  | 6bc05265-e744-4e87-a177-98f47ee0910b | Golden Light                               | None                                              | None                                              | 4430269  | /entity/6bc05265-e744-4e87-a177-98f47ee0910b |
| 5  | ad0d5905-092c-41fa-8626-eafa0282b090 | Golden Goose Deluxe Brand                  | None                                              | https://golden-storage-production.s3.amazonaws... | 5048379  | /entity/ad0d5905-092c-41fa-8626-eafa0282b090 |
| 6  | eddceeea-2f56-43d1-bfab-02b49749a016 | Golden Link Plus                           | Company                                           | https://golden-storage-production.s3.amazonaws... | 5385158  | /entity/eddceeea-2f56-43d1-bfab-02b49749a016 |
| 7  | a1c8c3e8-3252-4930-89ff-ee1e07ae5ad4 | Golden Triangle Angel Network              |                                                   | https://golden-storage-production.s3.amazonaws... | 5864791  | /entity/a1c8c3e8-3252-4930-89ff-ee1e07ae5ad4 |
| 8  | 4010b371-a216-4dc7-9243-daa863fb1204 | Golden Ratio Coin                          | Golden Ratio Holdings is a blockchain-based re... | https://golden-storage-production.s3.amazonaws... | 11794084 | /entity/4010b371-a216-4dc7-9243-daa863fb1204 |
| 9  | 25c2fd7e-5d52-47a9-a666-72187c5e163e | Golden Ratio Per Liquidity                 | None                                              | https://golden-storage-production.s3.amazonaws... | 11794461 | /entity/25c2fd7e-5d52-47a9-a666-72187c5e163e |
| 10 | cdd785ae-3923-46e9-bb8b-80deba364a3b | Golden Ocean Management AS                 | None                                              | None                                              | 12665977 | /entity/cdd785ae-3923-46e9-bb8b-80deba364a3b |
| 11 | 843cf991-c25e-40b6-bc4a-154f1a6923bf | Golden Ocean Shipping Co Pte. Ltd.         | None                                              | None                                              | 12665978 | /entity/843cf991-c25e-40b6-bc4a-154f1a6923bf |
| 12 | 838dcb8c-13f5-45d8-a4b5-27a9e990c5df | Golden Bros                                | Offical Link WEBSITE: http://goldenbros.netmar... | https://golden-storage-production.s3.amazonaws... | 12766503 | /entity/838dcb8c-13f5-45d8-a4b5-27a9e990c5df |
| 13 | bd22f60d-7754-40c7-be0a-80403c34ed2b | Golden Yachts                              | Yachts Fleet                                      | https://golden-storage-production.s3.amazonaws... | 12797348 | /entity/bd22f60d-7754-40c7-be0a-80403c34ed2b |
| 14 | 07747065-fadb-47f5-9915-d0a5ec63e8c4 | Golden Rooster Awards                      | None                                              | None                                              | 12858451 | /entity/07747065-fadb-47f5-9915-d0a5ec63e8c4 |
| 15 | 39277dfa-09e8-43a5-9067-6a83613a93fa | California Golden Seals                    | Former hockey team of the national hockey league  | https://golden-storage-production.s3.amazonaws... | 129536   | /entity/39277dfa-09e8-43a5-9067-6a83613a93fa |
| 16 | f82ec64d-8ad1-480b-bee9-48656d61615b | Golden Lion                                | The highest prize given to a film at the venic... | None                                              | 595284   | /entity/f82ec64d-8ad1-480b-bee9-48656d61615b |
| 17 | 5394c0d0-be65-46a2-b6a1-2efb7e510320 | Golden Dragon (company)                    |                                                   | https://golden-media.s3.amazonaws.com/topics/3... | 1647119  | /entity/5394c0d0-be65-46a2-b6a1-2efb7e510320 |
| 18 | 049b461d-ef7f-433d-9603-3046120c7e1e | Golden Trailer Awards                      | Annual awards show that honors achievements in... | https://golden-storage-production.s3.amazonaws... | 2264985  | /entity/049b461d-ef7f-433d-9603-3046120c7e1e |
| 19 | 0892f3d1-6c1a-443a-b143-624107ef5394 | California Golden Bears women's basketball | None                                              | https://golden-media.s3.amazonaws.com/topics/3... | 3682108  | /entity/0892f3d1-6c1a-443a-b143-624107ef5394 |

### 3. Get Predicates and Templates

Can't really create entities and triples without knowing what predicates and templates exist.

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

We're going to create entities with an example JSON source data.

```python
source_data = {
   "Name":{
      "0":"HydraDX (HDX)",
      "1":"Brokoli Network",
      "2":"Huobi",
      "3":"Kava",
      "4":"Axelar"
   },
   "Description":{
      "0":"HydraDX is a basilisk - cross-chain liquidity protocol on Kusama founded by Mattia Gagliardi.",
      "1":"Brokoli is a DeFi platform that allows users to act on climate Digital NFT forest.",
      "2":"A cryptocurrency exchange based in Singapore with its own native stablecoin called HUSD. ",
      "3":" Kava is a platform providing for accessing Decentralized Financial (DeFi) apps and services. ",
      "4":"Axelar is a universal interoperability platform that connects blockchains through a decentralized network and an SDK of protocols and APIs. "
   },
   "Thumbnail":{
      "0":"https://golden-storage-production.s3.amazonaws.com/topic_images/9fdf98be50324fd4b0c668640d33cc8d.png",
      "1":"https://golden-storage-production.s3.amazonaws.com/topic_images/291ed726246f4a6f93e667354d813ea0.png",
      "2":"https://golden-storage-production.s3.amazonaws.com/topic_images/b7c7d455aeed4bc6808ac585590bda9f.png",
      "3":"https://golden-storage-production.s3.amazonaws.com/topic_images/9446149fd249469b9474b1fa07214039.png",
      "4":"https://golden-storage-production.s3.amazonaws.com/topic_images/54a75bd2baed4878939bd401e62117ce.png"
   },
   "Url":{
      "0":"https://golden.com/wiki/HydraDX_(HDX)-GZZ3PRJ",
      "1":"https://golden.com/wiki/Brokoli_Network-BYMAZVM",
      "2":"https://golden.com/wiki/Huobi-PNX9AZ",
      "3":"https://golden.com/wiki/Kava-39W65V5",
      "4":"https://golden.com/wiki/Axelar-EKWD8JM"
   },
   "Website URL":{
      "0":"https://hydradx.io/, http://hydradx.io/, https://hydradx.substack.com/",
      "1":"https://brokoli.network, https://brokolinetwork.medium.com/, https://brokoli.network/metrics",
      "2":"https://www.hbg.com/, https://www.huobi.vc, https://huobi.com/, https://huobi.com, https://www.huobi.com, https://www.hbtc.finance/en-us/, https://www.htokens.finance/en-us/, https://www.huobi.co.th/, https://www.huobi.io/",
      "3":"http://kava.io, https://www.kava.io, https://app.kava.io/",
      "4":"https://axelar.network/, https://satellite.axelar.network/"
   },
   "Industry":{
      "0":"Decentralized cryptocurrency exchange, Blockchain, Blockchain and cryptocurrency, Decentralized Finance (DeFi)",
      "1":"Cryptocurrency, Blockchain and cryptocurrency, Blockchain, Non-fungible token (NFT)",
      "2":"Cryptocurrency, Blockchain, Cryptocurrency exchange, Crypto-collateralized, Asset management, Decentralized Finance (DeFi), Financial technology, Blockchain and cryptocurrency, Bitcoin, Marketplace, Distributed ledger",
      "3":"Decentralized Finance (DeFi), Financial services, Cryptocurrency, Blockchain, Payment system, Payment, Blockchain and cryptocurrency, Decentralized autonomous organization (DAO)",
      "4":"Decentralized autonomous organization (DAO), Blockchain, Blockchain interoperability, Cross-Chain Technology, Non-fungible token (NFT)"
   },
   "Location":{
      "0":"California, United States",
      "1":"None",
      "2":"Singapore, Seychelles, Beijing, London",
      "3":"San Francisco, Incline Village, Nevada, United States",
      "4":"Toronto, Canada"
   }
}
```

```python
blockchains = pd.DataFrame(source_data)
entities = [(name, description, thumbnail, website_url, golden_url) for name, description, thumbnail, website_url, golden_url in zip(blockchains['Name'], blockchains['Description'], blockchains['Thumbnail'], blockchains['Website URL'], blockchains["Url"])]
blockchains
```

|   | Name            | Description                                       | Thumbnail                                         | Url                                              | Website URL                                       | Industry                                          | Location                                          |
| - | --------------- | ------------------------------------------------- | ------------------------------------------------- | ------------------------------------------------ | ------------------------------------------------- | ------------------------------------------------- | ------------------------------------------------- |
| 0 | HydraDX (HDX)   | HydraDX is a basilisk - cross-chain liquidity ... | https://golden-storage-production.s3.amazonaws... | https://golden.com/wiki/HydraDX\_(HDX)-GZZ3PRJ   | https://hydradx.io/, http://hydradx.io/, https... | Decentralized cryptocurrency exchange, Blockch... | California, United States                         |
| 1 | Brokoli Network | Brokoli is a DeFi platform that allows users t... | https://golden-storage-production.s3.amazonaws... | https://golden.com/wiki/Brokoli\_Network-BYMAZVM | https://brokoli.network, https://brokolinetwor... | Cryptocurrency, Blockchain and cryptocurrency,... | None                                              |
| 2 | Huobi           | A cryptocurrency exchange based in Singapore w... | https://golden-storage-production.s3.amazonaws... | https://golden.com/wiki/Huobi-PNX9AZ             | https://www.hbg.com/, https://www.huobi.vc, ht... | Cryptocurrency, Blockchain, Cryptocurrency exc... | Singapore, Seychelles, Beijing, London            |
| 3 | Kava            | Kava is a platform providing for accessing De...  | https://golden-storage-production.s3.amazonaws... | https://golden.com/wiki/Kava-39W65V5             | http://kava.io, https://www.kava.io, https://a... | Decentralized Finance (DeFi), Financial servic... | San Francisco, Incline Village, Nevada, United... |
| 4 | Axelar          | Axelar is a universal interoperability platfor... | https://golden-storage-production.s3.amazonaws... | https://golden.com/wiki/Axelar-EKWD8JM           | https://axelar.network/, https://satellite.axe... | Decentralized autonomous organization (DAO), B... | Toronto, Canada                                   |

```python
from godel.schema import CreateEntityInput, QualifierInputRecordInput, StatementInputRecordInput

create_entity_inputs = []

for ent in entities:
    name, description, thumbnail, website_url, golden_url = ent
    
    # Create triples inputs (statements and qualifiers)
    statement_input_record_inputs = []

    # Add Template
    statement_input_record_inputs.append(
        StatementInputRecordInput(
            predicate_id = predicates["Is a"]["id"],
            object_entity_id = templates["Company"]["entityId"],
            citation_url = golden_url,
            qualifiers = []
        )
    )
    
    # Add Website
    for wurl in website_url.split(","):
        statement_input_record_inputs.append(
            StatementInputRecordInput(
                predicate_id = predicates["Website"]["id"],
                object_value = wurl,
                citation_url = golden_url,
                qualifiers = []
            )
        )

    # Add Description
    statement_input_record_inputs.append(
        StatementInputRecordInput(
            predicate_id = predicates["Description"]["id"],
            object_value = description,
            citation_url = golden_url,
            qualifiers = []
        )
    )

    # Add Thumbnail
    statement_input_record_inputs.append(
        StatementInputRecordInput(
            predicate_id = predicates["Thumbnail"]["id"],
            object_value = thumbnail,
            citation_url = golden_url,
            qualifiers = []
        )
    )
    
    # Create Entity Input
    create_entity_input = CreateEntityInput(
        name = name,
        statements = statement_input_record_inputs
    )
    create_entity_inputs.append(create_entity_input)
```

### WARNING: Running code below may charge gas fees and stake testnet points with your wallet. You will lose testnet points by submitting the data below.

```python
from tqdm import tqdm

created_data = []
for create_entity_input in tqdm(create_entity_inputs):
    data = goldapi.create_entity(input=create_entity_input.__to_json_value__())
    created_data.append(data)
```

```
100%|██████████| 5/5 [00:02<00:00,  2.08it/s]
```

```python
created_data_df = pd.DataFrame([d["data"]["createEntity"]["entity"] for d in created_data])
created_data_df
```

|   | \_\_typename | id                                   | name            | description                                       | thumbnail                                         | isA                               | statementsBySubjectId                                |
| - | ------------ | ------------------------------------ | --------------- | ------------------------------------------------- | ------------------------------------------------- | --------------------------------- | ---------------------------------------------------- |
| 0 | Entity       | b355c9e6-a2c4-4ccc-a984-989df851344d | HydraDX (HDX)   | HydraDX is a basilisk - cross-chain liquidity ... | https://golden-storage-production.s3.amazonaws... | {'nodes': \[{'name': 'Company'}]} | {'nodes': \[{'\_\_typename': 'Statement', 'dateRe... |
| 1 | Entity       | a0d0afdc-7c2f-43c6-83e9-9e0ca8de31ac | Brokoli Network | Brokoli is a DeFi platform that allows users t... | https://golden-storage-production.s3.amazonaws... | {'nodes': \[{'name': 'Company'}]} | {'nodes': \[{'\_\_typename': 'Statement', 'dateRe... |
| 2 | Entity       | 378684b2-c964-47a5-9321-1d79ea06ecd0 | Huobi           | A cryptocurrency exchange based in Singapore w... | https://golden-storage-production.s3.amazonaws... | {'nodes': \[{'name': 'Company'}]} | {'nodes': \[{'\_\_typename': 'Statement', 'dateRe... |
| 3 | Entity       | 6c10d1b1-e1f2-43bf-85c4-af5dd33af555 | Kava            | Kava is a platform providing for accessing De...  | https://golden-storage-production.s3.amazonaws... | {'nodes': \[{'name': 'Company'}]} | {'nodes': \[{'\_\_typename': 'Statement', 'dateRe... |
| 4 | Entity       | 2ee34015-a9b1-4f3d-aaa5-92a73145bd62 | Axelar          | Axelar is a universal interoperability platfor... | https://golden-storage-production.s3.amazonaws... | {'nodes': \[{'name': 'Company'}]} | {'nodes': \[{'\_\_typename': 'Statement', 'dateRe... |

### 5. Add triples

The examples below will not submit triples and are just examples of submitting entity to entity triple statements to the protocol via. the API.

```python
entity_triples = predicates_df[predicates_df.objectType=="ENTITY"]
entity_triple_choices = list(entity_triples.id)
entity_triples
```

|            | id                                   | objectType |
| ---------- | ------------------------------------ | ---------- |
| CEO        | 0a87e996-34b4-46ba-909a-70ab67b1f811 | ENTITY     |
| CEO of     | 3104de39-071c-47b8-86b4-d62ccc4a4fa6 | ENTITY     |
| Founder    | 71ad3d9e-e211-472b-a16d-861737c57ecd | ENTITY     |
| Is a       | 94a8d215-ce32-4379-b18e-2aebf0794882 | ENTITY     |
| Founder of | e4f94b98-c56a-4bd2-a9fd-5fd11603e7e8 | ENTITY     |

```python
triple_entity_id = 'f9caddca-f842-11ec-b3cf-0242ac130003'
object_entity_id = '0741de5e-f843-11ec-b3cf-0242ac130003'
predicate_id = '0a87e996-34b4-46ba-909a-70ab67b1f811'
    
goldapi.add_triple_to_entity(
    entity_id=triple_entity_id, 
    predicate_id=predicate_id, 
    object_entity_id=object_entity_id, 
    citation_url="https://golden.com"
)
```

### 6. View Data

```python
data_idx = 0
created_entity_id = created_data[data_idx]["data"]["createEntity"]["entity"]["id"]
created_entity_id
link = f"https://dapp.golden.xyz/entity/{created_entity_id}"
link
```

```
'https://dapp.golden.xyz/entity/b355c9e6-a2c4-4ccc-a984-989df851344d'
```
