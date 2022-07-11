# Quickstart

## Installation

Godel requires Python 3.8+

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

# Retrieve existing triple predicates
goldapi.predicates()

# Retrieve existing entity templates 
goldapi.templates()

# Search and query for specific entities
goldapi.entity_search(name="Golden")
```
