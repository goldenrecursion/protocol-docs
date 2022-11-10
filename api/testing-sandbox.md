---
description: Safely test submissions and verifications to the Golden protocol
---

# Testing Sandbox

The Golden protocol offers an isolated testing environment that closely reflects the production knowledge graph, verification, and submission processes. This environment allows for the creation and testing of tools without the risk of losing testnet points or appearing as a bad actor on the protocol. Compared to the production environment we have made a few changes to this sandbox, namely:

* **Instant feedback on verification voting**: verification consensus has been modified to only require 1 vote and triples retrieved from the verification queue are pulled from an `accepted` or `rejected` statuses. This allows you to check out the accuracy of your automated verification tools before staking on production.
* **Rolling graph data**: data displayed on the testing environment will not be updated in live time. This data will be pulled from the production environment on a regular basis.

## Usage

To use the sandbox simply replace the production API endpoint with the testing endpoint and use the [overview](../overview/ "mention") or [godel-python-sdk](../godel-python-sdk/ "mention") as normal.&#x20;

{% hint style="info" %}
**Testing API endpoint**: [`https://sandbox.dapp.golden.xyz/`](https://sandbox.dapp.golden.xyz/)&#x20;
{% endhint %}

Production API endpoint for reference: [`https://dapp.golden.xyz/`](https://sandbox.dapp.golden.xyz/)&#x20;



Setting up the [godel-python-sdk](../godel-python-sdk/ "mention") to use the testing sandbox:

```python
from godel import GoldenAPI

JWT_TOKEN = #YOUR_JWT_TOKEN_HERE
API_URL = "https://sandbox.dapp.golden.xyz/graphql" # <== Testing sandbox endpoint 
goldapi = GoldenAPI(url=API_URL)
goldapi.set_jwt_token(jwt_token=JWT_TOKEN)
```

#### Visual interface

The sandbox also offers the GraphiQL visual interface where you can build and test queries through the browser. See: [https://sandbox.dapp.golden.xyz/graphiql](https://sandbox.dapp.golden.xyz/graphiql)

