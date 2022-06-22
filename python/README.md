# Python

The Golden community highly encourages data-oriented developers to efficiently and optimally submit data to the knowledge graph protocol.

In order to help agents and developers programmatically submit data via. the GraphQL API, Golden's has provided an official open source python wrapper for its API.

{% embed url="https://github.com/goldenrecursion/dapp-api" %}

The following pages will provide additional guides and walkthroughs for ingesting data in the form of python code to help developers submit accurate and valuable data to Golden's protocol.

## Install

Use pip to install the python wrapper package.

```
pip install git+ssh://git@github.com/goldenrecursion/dapp-api
```

### Try It Out

```python
# Import package
from dapp_api import GoldenAPI

# Connect to API (no authentication in this example)
url = "https://dapp.golden.xyz/graphql"
gapi = GoldenAPI(url=url)

print(gapi.predicates())
```
