# Validation

Here's a quickstart for validating your first triple submitted to the Golden protocol!

### 1. Connect to Golden Web3 API

Let's connect the python wrapper to the Golden GraphQL API.

You can run your basic queries against the API, but will be restricted from mutations and queries that require you to connect or authenticate your wallet.

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
JWT TOKEN:  eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJyb2xlIjoidXNlcl9yb2xlIiwidXNlcl9pZCI6IjB4ZDg2YjUzNEMwMjlBYWY4NDI3MmEzYzcyNUY5NDA4ZTAzREVGNGI4MCIsImV4cCI6MTY1Njc0NjkxNCwiaWF0IjoxNjU2NTc0MTE2LCJhdWQiOiJwb3N0Z3JhcGhpbGUiLCJpc3MiOiJwb3N0Z3JhcGhpbGUifQ.uJstgQjI3cPrKMPiE6PsrqQElLA0xhkdJn9qZf9_ar4
```

### 3. Retrieve unvalidated triple

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
{'id': '573e2c32-ae16-4a76-b1d3-85a5775b0a85',
 'citationUrl': None,
 'dateCreated': '2022-01-18T02:28:30.034687',
 'objectEntityId': None,
 'objectValue': 'https://twitter.com/airshibainu2021',
 'subjectId': '79052d9e-705f-4dfb-aca6-d3033bb0bbc7',
 'userId': '0xa6A94aE94Eb5ee16e416165B89BDaE328cc17fe8',
 'predicateId': '9934d828-963f-403a-a0da-7a52e0224ef5'}
```

### 4. Create validation

```python
# Create validation with the triple id and your validation type
triple_id = unvalidated_triple["id"]
validation_type = "ACCEPTED"
triple_id
```

```
'573e2c32-ae16-4a76-b1d3-85a5775b0a85'
```

### WARNING: Running code below may charge gas fees and stake tokens with your wallet. You may lose tokens by submitting the data below.

```python
# Create Validation
goldapi.create_validation(triple_id=triple_id, validation_type=validation_type)
```

```
{'data': {'createValidation': {'validation': {'id': 'def17e91-5bbc-49de-8917-7c0cbb6980b9'}}}}
```

