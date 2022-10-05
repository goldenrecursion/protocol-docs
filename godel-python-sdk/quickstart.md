# Quickstart

## Installation

Godel requires Python 3.7+

```
pip install godel

# Add extras to install web3, jupyter, data tools, etc.
pip install godel[web3,data-tools]
```

## GraphQL Examples

Query against the protocol GraphQL API to retrieve and explore data in the knowledge graph.

```python
from godel import GoldenAPI

goldapi = GoldenAPI()

# Operations that do not require authentication
# Retrieve existing triple predicates
goldapi.predicates()

# Retrieve existing entity templates 
goldapi.templates()

# With signed authentication
# Find your JSON Web Token(JWT) at "https://dapp.golden.xyz/profile" after signing in
goldapi = GoldenAPI(jwt_token=<your_token>)

# Search and query for specific entities
goldapi.entity_search(name="Golden")
```
