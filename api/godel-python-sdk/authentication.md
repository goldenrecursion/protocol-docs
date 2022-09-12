# Authentication

### Authentication options

To authenticate your account with godel you have two options, you may retrieve your token from your profile page in the dApp or you may programmatically generate a key using your wallet address and secret keys.&#x20;

## Retrieve JWT Token from GUI

Follow the directions at https://docs.golden.xyz/guides/authentication to skip the programmatic authentication and plainly retrieve your JWT token from https://dapp.golden.xyz.

## Programmatic Authentication

### Prerequisite

[QuickStart](https://docs.golden.xyz/godel-python-sdk/quickstart)

This guide requires installation of [Web3.py](https://github.com/ethereum/web3.py).

You can do this with `pip install godel[web3]` and comes pre-installed if using the godel docker image.

### 1. Connect to Golden Web3 API

Let's connect the python wrapper to the Golden GraphQL API.

You can run your basic queries against the API without authenticating, but will be restricted from mutations and certain queries until you connect using your USER\_ID (your wallet address) and PRIVATE\_KEY (wallet secret).

Note: If you encounter errors with connection, ensure that the wallet being used has been connected to your profile at

```python
USER_ID = "0xd0587b87a875784568gf967679a58f578"
PRIVATE_KEY = "65687a467467m357ghf476f5846578678a5858a658758ut58587674674x67567"
```

```python
from godel import GoldenAPI

goldapi = GoldenAPI()
```

### 2. Authenticate and set JWT

This will set the JWT token key in your GoldenAPI object so you should have permission to hit all endpoints after running the below.

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

print("Your JWT Token:\n", jwt_token)
```

#### Convenience Method for Authentication

Runs the above authentication process in a secure and convenient method.

```python
goldapi.get_authentication_token(
    user_id=USER_ID,
    wallet_private_key=PRIVATE_KEY
)
```

### 3. Test with Graphql Calls

Once authenticated, you'll now have permission to search, disambiguate, validate, and submit data to the Golden Protocol.

```python
goldapi.entity_search("golden")
```

```
{'data': {'entityByName': {'nodes': [{'id': '5c0d11ad-b6e7-4d08-a0f1-6a224d93f3a1',
     'name': 'Golden',
     'description': 'Golden is a company started in San Francisco, USA. The company is building an encyclopedia and knowledge tool. The founder and CEO of Golden is Jude Gomila.',
     'thumbnail': 'https://golden-media.s3.amazonaws.com/topics/5253ceef-b9f0-49c5-ae0e-e6a445078551.png',
     'goldenId': None,
     'pathname': '/entity/5c0d11ad-b6e7-4d08-a0f1-6a224d93f3a1'},
    {'id': '7baeabfe-5abe-4f91-a280-1a8d238544c2',
     'name': 'Golden State Warriors',
     'description': 'The Golden State Warriors of the National Basketball Association are a professional basketball team based in Oakland, California that competes in the Pacific Division of the Western Conference. Founded in 1946, the Warriors have won three NBA championships.',
     'thumbnail': 'https://golden-storage-production.s3.amazonaws.com/topic_images/b49f887f78404364ac63b084db7e7d79.png',
     'goldenId': '34936',
     'pathname': '/entity/7baeabfe-5abe-4f91-a280-1a8d238544c2'},
    {'id': '63d7e9ed-3d0c-429b-9fc4-4a6669f62426',
     'name': 'Golden Eagle Award (Russia)',
     'description': 'Russian film and television award',
     'thumbnail': 'https://golden-storage-production.s3.amazonaws.com/topic_images/72ff934c054640b59d496605e2e1a6b2.png',
     'goldenId': '838491',
     'pathname': '/entity/63d7e9ed-3d0c-429b-9fc4-4a6669f62426'},
    {'id': '3bb86c99-f521-48fa-ae03-7f4630f64767',
     'name': 'Golden Star Resources',
     'description': '',
     'thumbnail': None,
     'goldenId': '2273691',
     'pathname': '/entity/3bb86c99-f521-48fa-ae03-7f4630f64767'},
    {'id': '6bc05265-e744-4e87-a177-98f47ee0910b',
     'name': 'Golden Light',
     'description': None,
     'thumbnail': None,
     'goldenId': '4430269',
     'pathname': '/entity/6bc05265-e744-4e87-a177-98f47ee0910b'},
    {'id': 'ad0d5905-092c-41fa-8626-eafa0282b090',
     'name': 'Golden Goose Deluxe Brand',
     'description': None,
     'thumbnail': 'https://golden-storage-production.s3.amazonaws.com/topic_images/7042a9df023a461c8aef08c385f1ffda.png',
     'goldenId': '5048379',
     'pathname': '/entity/ad0d5905-092c-41fa-8626-eafa0282b090'},
    {'id': 'eddceeea-2f56-43d1-bfab-02b49749a016',
     'name': 'Golden Link Plus',
     'description': 'Company',
     'thumbnail': 'https://golden-storage-production.s3.amazonaws.com/topic_images/c08a826d4ff94cb98cb5655363ff3b47.png',
     'goldenId': '5385158',
     'pathname': '/entity/eddceeea-2f56-43d1-bfab-02b49749a016'},
    {'id': 'a1c8c3e8-3252-4930-89ff-ee1e07ae5ad4',
     'name': 'Golden Triangle Angel Network',
     'description': '',
     'thumbnail': 'https://golden-storage-production.s3.amazonaws.com/topic_images/3fe466e1d9c24f71a4f9b4163ad16eda.jpeg',
     'goldenId': '5864791',
     'pathname': '/entity/a1c8c3e8-3252-4930-89ff-ee1e07ae5ad4'},
    {'id': '4010b371-a216-4dc7-9243-daa863fb1204',
     'name': 'Golden Ratio Coin',
     'description': 'Golden Ratio Holdings is a blockchain-based real estate marketplace.',
     'thumbnail': 'https://golden-storage-production.s3.amazonaws.com/topic_images/9abd07c7683f482c854f10c3661bac37.png',
     'goldenId': '11794084',
     'pathname': '/entity/4010b371-a216-4dc7-9243-daa863fb1204'},
    {'id': '25c2fd7e-5d52-47a9-a666-72187c5e163e',
     'name': 'Golden Ratio Per Liquidity',
     'description': None,
     'thumbnail': 'https://golden-storage-production.s3.amazonaws.com/topic_images/0b57ae51e5c34f71aded47f5011a08d6.png',
     'goldenId': '11794461',
     'pathname': '/entity/25c2fd7e-5d52-47a9-a666-72187c5e163e'},
    {'id': 'cdd785ae-3923-46e9-bb8b-80deba364a3b',
     'name': 'Golden Ocean Management AS',
     'description': None,
     'thumbnail': None,
     'goldenId': '12665977',
     'pathname': '/entity/cdd785ae-3923-46e9-bb8b-80deba364a3b'},
    {'id': '843cf991-c25e-40b6-bc4a-154f1a6923bf',
     'name': 'Golden Ocean Shipping Co Pte. Ltd.',
     'description': None,
     'thumbnail': None,
     'goldenId': '12665978',
     'pathname': '/entity/843cf991-c25e-40b6-bc4a-154f1a6923bf'},
    {'id': '838dcb8c-13f5-45d8-a4b5-27a9e990c5df',
     'name': 'Golden Bros',
     'description': 'Offical Link WEBSITE: http://goldenbros.netmarble.com Facebook: http://facebook.com/GoldenBrosNM Twitter: https://twitter.com/GoldenBrosNM Discord: http://discord.gg/goldenbros',
     'thumbnail': 'https://golden-storage-production.s3.amazonaws.com/topic_images/5ba2730ede9641f08fc1fd1779eea48f.png',
     'goldenId': '12766503',
     'pathname': '/entity/838dcb8c-13f5-45d8-a4b5-27a9e990c5df'},
    {'id': 'bd22f60d-7754-40c7-be0a-80403c34ed2b',
     'name': 'Golden Yachts',
     'description': 'Yachts Fleet',
     'thumbnail': 'https://golden-storage-production.s3.amazonaws.com/topic_images/27d0bc749f484e55b97a61bbd2adc223.png',
     'goldenId': '12797348',
     'pathname': '/entity/bd22f60d-7754-40c7-be0a-80403c34ed2b'},
    {'id': '07747065-fadb-47f5-9915-d0a5ec63e8c4',
     'name': 'Golden Rooster Awards',
     'description': None,
     'thumbnail': None,
     'goldenId': '12858451',
     'pathname': '/entity/07747065-fadb-47f5-9915-d0a5ec63e8c4'},
    {'id': '39277dfa-09e8-43a5-9067-6a83613a93fa',
     'name': 'California Golden Seals',
     'description': 'Former hockey team of the national hockey league',
     'thumbnail': 'https://golden-storage-production.s3.amazonaws.com/topic_images/6d7359c3eed14ac7b125f99525745538.png',
     'goldenId': '129536',
     'pathname': '/entity/39277dfa-09e8-43a5-9067-6a83613a93fa'},
    {'id': 'f82ec64d-8ad1-480b-bee9-48656d61615b',
     'name': 'Golden Lion',
     'description': 'The highest prize given to a film at the venice film festival',
     'thumbnail': None,
     'goldenId': '595284',
     'pathname': '/entity/f82ec64d-8ad1-480b-bee9-48656d61615b'},
    {'id': '5394c0d0-be65-46a2-b6a1-2efb7e510320',
     'name': 'Golden Dragon (company)',
     'description': '',
     'thumbnail': 'https://golden-media.s3.amazonaws.com/topics/300px-Golden_Dragon_logo_2.jpg',
     'goldenId': '1647119',
     'pathname': '/entity/5394c0d0-be65-46a2-b6a1-2efb7e510320'},
    {'id': '049b461d-ef7f-433d-9603-3046120c7e1e',
     'name': 'Golden Trailer Awards',
     'description': 'Annual awards show that honors achievements in motion picture marketing, especially film trailers.',
     'thumbnail': 'https://golden-storage-production.s3.amazonaws.com/topic_images/07ac8509b021459683eab3be2b704fe3.png',
     'goldenId': '2264985',
     'pathname': '/entity/049b461d-ef7f-433d-9603-3046120c7e1e'},
    {'id': '0892f3d1-6c1a-443a-b143-624107ef5394',
     'name': "California Golden Bears women's basketball",
     'description': None,
     'thumbnail': 'https://golden-media.s3.amazonaws.com/topics/300px-California_Golden_Bears_logo.svg.jpg',
     'goldenId': '3682108',
     'pathname': '/entity/0892f3d1-6c1a-443a-b143-624107ef5394'}]}},
 'meta': {'graphqlQueryCost': 2}}
```

```python
```
