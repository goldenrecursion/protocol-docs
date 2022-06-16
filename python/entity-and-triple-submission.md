# Entity and Triple Submission

This is an end-to-end example of submitting entities and triples with Golden's GraphQL API using the python wrapper.

### 1. Connect to Golden Web3 API

Let's connect the python wrapper to the Golden GraphQL API.

You can run your basic queries against the API, but will be restricted from mutations and queries that require you to connect or authenticate your wallet.

You have to specify your wallet address and private key.

```python
USER_ID = "0x019235718q0879saf908d87gd8gsdg"
PRIVATE_KEY = "987g76sa6g9d6gs98ad76gg7sda687gd6gd6sadg867790n8vc"

DAPP_URL = "https://dapp.golden.xyz/graphql"
```

```python
from dapp_api import GoldenAPI

url = DAPP_URL
gapi = GoldenAPI(url=url)
```

### 2. Authenticate and set JWT

This will set the JWT token key in your GoldenAPI object so you should have permission to hit all endpoints after running the below.

```python
# Retrieve one-off nonce from GraphQL API
message_response = gapi.get_authentication_message(user_id=USER_ID)
message_response

# Sign and verify nonce with your wallet's private key (KEEP THIS SECURE)
from web3.auto import w3
from eth_account.messages import encode_defunct

message_string = message_response["data"]["getAuthenticationMessage"]["string"]
message = encode_defunct(text=message_string)
signed_message = w3.eth.account.sign_message(message, private_key=PRIVATE_KEY)
signature = signed_message.signature.hex()

# Authenticate with Golden's API and you'll recieve a jwt bearer token
auth_response = gapi.authenticate(
    user_id=USER_ID,
    signature=signature
)

jwt_token = auth_response["data"]["authenticate"]["jwtToken"]

# Set JWT token to verify your wallet/role and unlock permissions to the rest of the API
gapi.set_jwt_token(jwt_token=jwt_token)

print("JWT TOKEN: ", jwt_token)
```

```
JWT TOKEN:  eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJyb2xlIjoidXNlcl9yb2xlIiwidXNlcl9pZCI6IjB4ZDg2YjUzNEMwMjlBYWY4NDI3MmEzYzcyNUY5NDA4ZTAzREVGNGI4MCIsImV4cCI6MTY1NTU2OTkwMSwiaWF0IjoxNjU1Mzk3MTAxLCJhdWQiOiJwb3N0Z3JhcGhpbGUiLCJpc3MiOiJwb3N0Z3JhcGhpbGUifQ.G3tOM1hFVHLWjm1s2-3h4eYuKZjz05aISnNStn_7p20
```

```python
results = gapi.entity_search(name="Bitcoin")
results
```

```
{'data': {'entityByName': {'nodes': [{'name': 'CEX.IO Bitcoin Exchange',
     'description': 'CEX.IO is a cryptocurrency exchange platform that supports a solid range of cryptocurrencies and has different deposit and withdrawal methods that its traders can utilize when it comes to buying or selling digital assets through the usage of FIAT currency.',
     'thumbnail': 'https://golden-media.s3.amazonaws.com/topics/300px-CEX.IO_logo.jpg',
     'goldenId': None,
     'pathname': '/entity/a7cfbdfa-1e31-400c-906f-5a0d642ff2ab'},
    {'name': 'Bitcoin Suisse AG',
     'description': 'Bitcoin Suisse AG is a Swiss-regulated financial intermediary specializing in crypto-financial services, based in Zug, Switzerland.',
     'thumbnail': 'https://golden-storage-production.s3.amazonaws.com/topic_images/0140f5c8819b4c42a16b36963ef2c146.png',
     'goldenId': None,
     'pathname': '/entity/7703add9-2c62-4a34-9518-f3c2b2939b02'},
    {'name': 'Bitcoin Magazine (company)',
     'description': 'Bitcoin Magazine is a news publication dedicated to Bitcoin and its industry.  The magazine was founded by Mihai Alisie and Vitalik Buterin.',
     'thumbnail': 'https://golden-storage-production.s3.amazonaws.com/topic_images/e4504d1dd493470aaf8082fc34d75000.png',
     'goldenId': None,
     'pathname': '/entity/bc53fb27-c807-439c-a4c6-757c3962101d'},
    {'name': 'Bitcoin SV',
     'description': 'Bitcoin SV (BSV) is a hard fork of the Bitcoin Cash blockchain from 2018, which was in turn forked from Bitcoin blockchain the prior year.',
     'thumbnail': 'https://golden-storage-production.s3.amazonaws.com/topic_images/199d34cda2934ee8bd6f9c8f277dc5b9.png',
     'goldenId': None,
     'pathname': '/entity/796fba78-b80f-4711-b6d1-ab8f3a7304ff'},
    {'name': 'Bitcoin Cash',
     'description': 'Bitcoin Cash is a peer-to-peer electronic cash hard-forked from the Bitcoin blockchain.',
     'thumbnail': 'https://golden-storage-production.s3.amazonaws.com/topic_images/baea7bb5ac9742f791adf754cadd12aa.png',
     'goldenId': '5288266',
     'pathname': '/entity/cf3665ee-f7e9-4fb5-820e-80d6996af467'},
    {'name': 'Bitcoin God',
     'description': None,
     'thumbnail': 'https://golden-media.s3.amazonaws.com/topic_images/92ef32965599422c8fd9f44cbd127588.png',
     'goldenId': '5289832',
     'pathname': '/entity/a16dc72f-dfd9-4921-9f71-a94922438370'},
    {'name': 'Unboxcoin Bitcoin Exchange',
     'description': 'Cryptocurrency company',
     'thumbnail': 'https://golden-media.s3.amazonaws.com/topic_images/164ed7ffd81248aa9c4700d8b9b2b62d.jpeg',
     'goldenId': '5297304',
     'pathname': '/entity/4bf4099b-4e41-4ac5-a550-e4a8e7df61c8'},
    {'name': 'Malairte Bitcoin',
     'description': 'Cryptocurrency company',
     'thumbnail': 'https://golden-media.s3.amazonaws.com/topic_images/e3d3beb883734fcb8696badc361f3b7e.jpeg',
     'goldenId': '5297536',
     'pathname': '/entity/ad774abe-727f-4302-bc05-2329bf1924d2'},
    {'name': 'Bitcoin Red',
     'description': None,
     'thumbnail': 'https://golden-storage-production.s3.amazonaws.com/topic_images/91e47eff3b7947cbaa916d06efdd8610.png',
     'goldenId': '5311370',
     'pathname': '/entity/d59c50f6-3892-4d28-a21a-5a06408d0544'},
    {'name': 'Bitcoin Exchange Nepal',
     'description': '',
     'thumbnail': None,
     'goldenId': '5352252',
     'pathname': '/entity/4adc9d16-4d99-4bd9-a62f-fb247d556d8f'},
    {'name': 'Bitcoin Standard Hashrate Token',
     'description': 'The Bitcoin Standard Hashrate Token (BTCST) was launched on Binance Smart Chain (BSC) on Dec. 13, 2020. It is collateralized by Bitcoin_x0019_s (BTC) hashrate, with each token representing 0.1 TH/s of Bitcoin mining power at an efficiency of 60 W/TH. ',
     'thumbnail': 'https://golden-storage-production.s3.amazonaws.com/topic_images/d99237c237c9442a8699316fbd0b6172.png',
     'goldenId': None,
     'pathname': '/entity/adf2099b-746d-4cdc-a5a9-e8274b99d472'},
    {'name': 'Bitcoin Cash',
     'description': 'Bitcoin Cash is a peer-to-peer electronic cash hard-forked from the Bitcoin blockchain.',
     'thumbnail': 'https://golden-storage-production.s3.amazonaws.com/topic_images/baea7bb5ac9742f791adf754cadd12aa.png',
     'goldenId': None,
     'pathname': '/entity/8c7a7178-7a74-4a00-83fd-fd164d37838e'},
    {'name': 'SpectroCoin (Bitcoin)',
     'description': 'All in one solution for cryptocurrencies',
     'thumbnail': 'https://golden-storage-production.s3.amazonaws.com/topic_images/3dc75d1ffa84485bb1b7ec33c4a3ce50.png',
     'goldenId': '5609385',
     'pathname': '/entity/255ad4cd-1621-4cea-8502-1933a0624efa'},
    {'name': 'Bitcoin Pulse',
     'description': None,
     'thumbnail': 'https://golden-storage-production.s3.amazonaws.com/topic_images/63d68904e8ee420cacb758f3f3f47f7a.png',
     'goldenId': '5610117',
     'pathname': '/entity/0f2ed6aa-5df9-4c11-b5ef-022d047b338c'},
    {'name': 'Bitcoin Gold',
     'description': 'Bitcoin Gold is a cryptocurrency, as well as a project creating a hard fork Bitcoin with new proof-of-work algorithm to make Bitcoin mining decentralized again.',
     'thumbnail': 'https://golden-media.s3.amazonaws.com/topic_images/f8d79fc7fac24ec2ad577dd797173207.jpeg',
     'goldenId': None,
     'pathname': '/entity/d3766355-9a60-47b4-8dfb-c0bc1751f47e'},
    {'name': 'Mercado Bitcoin',
     'description': None,
     'thumbnail': None,
     'goldenId': '7682939',
     'pathname': '/entity/d7318270-1122-4031-9cc2-93064cc2d56e'},
    {'name': 'Bitcoin SV',
     'description': 'Bitcoin SV (BSV) is a hard fork of the Bitcoin Cash blockchain from 2018, which was in turn forked from Bitcoin blockchain the prior year.',
     'thumbnail': 'https://golden-storage-production.s3.amazonaws.com/topic_images/199d34cda2934ee8bd6f9c8f277dc5b9.png',
     'goldenId': '11529301',
     'pathname': '/entity/456ba6de-ae38-4a09-95c5-04a3a3139a96'},
    {'name': 'Bitcoin Gold BTG',
     'description': None,
     'thumbnail': 'https://golden-storage-production.s3.amazonaws.com/topic_images/bbd4c982ae264432a69b8581a65c0696.png',
     'goldenId': '11652445',
     'pathname': '/entity/ee279ac1-b4a3-4014-8966-ed51e79a7f17'},
    {'name': 'Bitcoin Atom',
     'description': 'Bitcoin Atom (BCA) is a cryptocurrency . Users are able to generate BCA through the process of mining.',
     'thumbnail': 'https://golden-storage-production.s3.amazonaws.com/topic_images/b4779062813d490d93bd2027ea040a9f.png',
     'goldenId': '11662726',
     'pathname': '/entity/044d51f8-4c89-466b-b551-f6f6831fcf33'},
    {'name': 'Mercado Bitcoin',
     'description': 'Mercado Bitcoin is a Brazil-based company founded in 2013.',
     'thumbnail': 'https://golden-storage-production.s3.amazonaws.com/topic_images/d3995446cd844ee1b5a78e3f5c08705a.png',
     'goldenId': None,
     'pathname': '/entity/e29fe1af-8ddc-4939-90f6-2e9069692b18'}]}}}
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

.dataframe tbody tr th:only-of-type { vertical-align: middle; } .dataframe tbody tr th { vertical-align: top; } .dataframe thead th { text-align: right; }

|         | id                                   | entityId                             | entityDescription |
| ------- | ------------------------------------ | ------------------------------------ | ----------------- |
| Person  | 0dea0f27-ce0c-4b4f-8ddb-5ff6adb57e12 | 0c4e6054-5fd8-48a8-817c-f6611278f755 | None              |
| Company | 9553f193-a46a-4b46-9c0f-5287289644a6 | 0a9fcc89-e14b-47af-85c3-8465ca607c29 | None              |

### 4. Create Entity

We're going to create entities with an example JSON source data.

```python
source_data = {'Name': {
    0: 'HydraDX (HDX)',
    1: 'Brokoli Network',
    2: 'Huobi',
    3: 'Kava',
    4: 'Axelar'},
    'Description': {0: 'HydraDX is a basilisk - cross-chain liquidity protocol on Kusama founded by Mattia Gagliardi.',
    1: 'Brokoli is a DeFi platform that allows users to act on climate Digital NFT forest.',
    2: 'A cryptocurrency exchange based in Singapore with its own native stablecoin called HUSD. ',
    3: ' Kava is a platform providing for accessing Decentralized Financial (DeFi) apps and services. ',
    4: 'Axelar is a universal interoperability platform that connects blockchains through a decentralized network and an SDK of protocols and APIs. '},
    'Thumbnail': {0: 'https://golden-storage-production.s3.amazonaws.com/topic_images/9fdf98be50324fd4b0c668640d33cc8d.png',
    1: 'https://golden-storage-production.s3.amazonaws.com/topic_images/291ed726246f4a6f93e667354d813ea0.png',
    2: 'https://golden-storage-production.s3.amazonaws.com/topic_images/b7c7d455aeed4bc6808ac585590bda9f.png',
    3: 'https://golden-storage-production.s3.amazonaws.com/topic_images/9446149fd249469b9474b1fa07214039.png',
    4: 'https://golden-storage-production.s3.amazonaws.com/topic_images/54a75bd2baed4878939bd401e62117ce.png'},
    'Url': {0: 'https://golden.com/wiki/HydraDX_(HDX)-GZZ3PRJ',
    1: 'https://golden.com/wiki/Brokoli_Network-BYMAZVM',
    2: 'https://golden.com/wiki/Huobi-PNX9AZ',
    3: 'https://golden.com/wiki/Kava-39W65V5',
    4: 'https://golden.com/wiki/Axelar-EKWD8JM'},
    'Website URL': {0: 'https://hydradx.io/, http://hydradx.io/, https://hydradx.substack.com/',
    1: 'https://brokoli.network, https://brokolinetwork.medium.com/, https://brokoli.network/metrics',
    2: 'https://www.hbg.com/, https://www.huobi.vc, https://huobi.com/, https://huobi.com, https://www.huobi.com, https://www.hbtc.finance/en-us/, https://www.htokens.finance/en-us/, https://www.huobi.co.th/, https://www.huobi.io/',
    3: 'http://kava.io, https://www.kava.io, https://app.kava.io/',
    4: 'https://axelar.network/, https://satellite.axelar.network/'},
    'Industry': {0: 'Decentralized cryptocurrency exchange, Blockchain, Blockchain and cryptocurrency, Decentralized Finance (DeFi)',
    1: 'Cryptocurrency, Blockchain and cryptocurrency, Blockchain, Non-fungible token (NFT)',
    2: 'Cryptocurrency, Blockchain, Cryptocurrency exchange, Crypto-collateralized, Asset management, Decentralized Finance (DeFi), Financial technology, Blockchain and cryptocurrency, Bitcoin, Marketplace, Distributed ledger',
    3: 'Decentralized Finance (DeFi), Financial services, Cryptocurrency, Blockchain, Payment system, Payment, Blockchain and cryptocurrency, Decentralized autonomous organization (DAO)',
    4: 'Decentralized autonomous organization (DAO), Blockchain, Blockchain interoperability, Cross-Chain Technology, Non-fungible token (NFT)'},
    'Location': {0: 'California, United States',
    1: None,
    2: 'Singapore, Seychelles, Beijing, London',
    3: 'San Francisco, Incline Village, Nevada, United States',
    4: 'Toronto, Canada'},
    'Unnamed: 7': {
        0: None, 1: None, 2: None, 3: None, 4: None}
}
```

```python
blockchains = pd.DataFrame(source_data)
```

```python
# Retrieve data from exported golden data
entities = [(name, description, thumbnail, website_url, golden_url) for name, description, thumbnail, website_url, golden_url in zip(blockchains['Name'], blockchains['Description'], blockchains['Thumbnail'], blockchains['Website URL'], blockchains["Url"])]
```

```python
from dapp_api.schema import CreateEntityInput, QualifierInputRecordInput, StatementInputRecordInput

create_entity_inputs = []

for ent in entities:
    name, description, thumbnail, website_url, golden_url = ent
    
    # Create triples inputs (statements and qualifiers)
    statement_input_record_inputs = []

    # Add Template
    qualifier_input_record_input = QualifierInputRecordInput(
        predicate_id = predicates["Is a"]["id"],
        object_entity_id = templates["Company"]["entityId"],
        citation_url = golden_url,
    )

    statement_input_record_inputs.append(
        StatementInputRecordInput(
            predicate_id = predicates["Is a"]["id"],
            object_entity_id = templates["Company"]["entityId"],
            citation_url = golden_url,
            qualifiers = [qualifier_input_record_input]
        )
    )
    
    # Add Website
    qualifier_input_record_input = QualifierInputRecordInput(
        predicate_id = predicates["Website"]["id"],
        object_value = website_url,
        citation_url = golden_url,
    )

    statement_input_record_inputs.append(
        StatementInputRecordInput(
            predicate_id = predicates["Website"]["id"],
            object_value = website_url,
            citation_url = golden_url,
            qualifiers = [qualifier_input_record_input]
        )
    )


    # Add Description
    qualifier_input_record_input = QualifierInputRecordInput(
        predicate_id = predicates["Description"]["id"],
        object_value = description,
        citation_url = golden_url,
    )

    statement_input_record_inputs.append(
        StatementInputRecordInput(
            predicate_id = predicates["Description"]["id"],
            object_value = description,
            citation_url = golden_url,
            qualifiers = [qualifier_input_record_input]
        )
    )

    # Add Thumbnail
    qualifier_input_record_input = QualifierInputRecordInput(
        predicate_id = predicates["Thumbnail"]["id"],
        object_value = thumbnail,
        citation_url = golden_url,
    )

    statement_input_record_inputs.append(
        StatementInputRecordInput(
            predicate_id = predicates["Thumbnail"]["id"],
            object_value = thumbnail,
            citation_url = golden_url,
            qualifiers = [qualifier_input_record_input]
        )
    )
    
    # Create Entity Input
    create_entity_input = CreateEntityInput(
        name = name,
        statements = statement_input_record_inputs
    )
    create_entity_inputs.append(create_entity_input)
```

```python
from tqdm import tqdm

created_data = []
for create_entity_input in tqdm(create_entity_inputs):
    data = gapi.create_entity(input=create_entity_input.__to_json_value__())
    created_data.append(data)
```

```
100%|██████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████| 5/5 [00:02<00:00,  2.49it/s]
```

```python
data_idx = 0
created_entity_id = created_data[data_idx]["data"]["createEntity"]["entity"]["id"]
created_entity_id
```

```
'cd2e1b6a-2fdf-4252-9591-62e90e11f5cd'
```

```python
```

### 5. Add triples

```python
entity_triples = predicates_df[predicates_df.objectType=="ENTITY"]
entity_triple_choices = list(entity_triples.id)
entity_triples
```

.dataframe tbody tr th:only-of-type { vertical-align: middle; } .dataframe tbody tr th { vertical-align: top; } .dataframe thead th { text-align: right; }

|            | id                                   | objectType |
| ---------- | ------------------------------------ | ---------- |
| CEO        | 0a87e996-34b4-46ba-909a-70ab67b1f811 | ENTITY     |
| CEO of     | 3104de39-071c-47b8-86b4-d62ccc4a4fa6 | ENTITY     |
| Founder    | 71ad3d9e-e211-472b-a16d-861737c57ecd | ENTITY     |
| Is a       | 94a8d215-ce32-4379-b18e-2aebf0794882 | ENTITY     |
| Founder of | e4f94b98-c56a-4bd2-a9fd-5fd11603e7e8 | ENTITY     |

```python
from random import choice

num_triples_to_generate = 100000
for i in tqdm(range(num_triples_to_generate)):
    triple_entity_id = choice(created_data)["data"]["createEntity"]["entity"]["id"]
    object_entity_id = choice(created_data)["data"]["createEntity"]["entity"]["id"]
    while triple_entity_id == object_entity_id:
        object_entity_id = choice(created_data)["data"]["createEntity"]["entity"]["id"]
    predicate_id = choice(entity_triple_choices)

    triple_entity_id, object_entity_id, predicate_id
    
    try:
        gapi.add_triple_to_entity_by_id(
            entity_id=triple_entity_id, 
            predicate_id=predicate_id, 
            object_entity_id=object_entity_id, 
            citation_url=golden_url)
    except:
        continue
```

```
  0%|                                                                                                                                                                                                                                                                                 | 22/100000 [00:07<10:31:14,  2.64it/s]
```

### 6. View Data

```python
data_idx = 0
created_entity_id = created_data[data_idx]["data"]["createEntity"]["entity"]["id"]
created_entity_id
link = f"{url}/entity/{created_entity_id}"
link
```

```
'https://dapp.golden.xyz/graphql/entity/cd2e1b6a-2fdf-4252-9591-62e90e11f5cd'
```

```python
from random import choice
created_entity_id = choice(created_data)["data"]["createEntity"]["entity"]["id"]
created_entity_id
link = f"{url}/entity/{created_entity_id}"
link
```

```
'https://dapp.golden.xyz/graphql/entity/ba414398-f82b-44e8-81fd-ef4104668074'
```
