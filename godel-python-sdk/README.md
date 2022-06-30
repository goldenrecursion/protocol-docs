---
description: Golden's Python SDK for its Decentralized Canonical Knowledge Graph
---

# Godel (Python SDK)

The Golden community highly encourages data-oriented developers to efficiently and optimally submit data to the knowledge graph protocol.

In order to help agents and developers programmatically submit data via. the GraphQL API, Golden's has provided an official open source Python SDK called Godel that provides tools like an API wrapper and web3 utililty modules.

{% embed url="https://github.com/goldenrecursion/godel" %}

The following pages will provide additional guides and walkthroughs for ingesting data in the form of python code to help developers submit accurate and valuable data to Golden's protocol.

{% content-ref url="validating-your-first-triple.md" %}
[validating-your-first-triple.md](validating-your-first-triple.md)
{% endcontent-ref %}

{% content-ref url="create-entities-and-triples.md" %}
[create-entities-and-triples.md](create-entities-and-triples.md)
{% endcontent-ref %}

## Overview

[Golden's protocol](https://golden.xyz/) provides users with the opportunity to query from and contribute towards building the world's most extensive decentralized graph of canonical knowledge.

Godel is Golden's open-source Python SDK that will provide developers and agents with the tools and means to build applications and extend Golden's protocol.

Godel's repository includes a python package and software to help provide developers with an API wrapper, data processing framework, and web3 utility toolkit for programmatic data query and ingest.

## Documentation

Godel guides and documentation can be found in [Golden's API Docs](https://golden-1.gitbook.io/api-docs/bIDcMAB995oqcXDPByjt/).

## Installation

Godel requires Python 3.8+

```
pip install godel

# Add extras to install web3, jupyter, data tools, etc.
pip install godel[web3,data-tools]
```

## Quickstart

Query against the protocol GraphQL API to retrieve and explore data in the knowledge graph.

```python
from godel import GoldenAPI

goldapi = GoldenAPI()

# Retrieve existing triple predicates
goldapi.predicates()

# Retrive existing entity templates 
goldapi.templates()

# Search and query for specific entities
goldapi.entity_search(name="Golden")
```

## Docker Setup

Run `docker-compose build` and then `docker-compose up`.

Go to `localhost:8888` to run our tutorials in jupyter lab.

Images are built with data processing and compute in mind to aid users in their validation and ingest queries.

## Contact

For all things related to `godel` and development, please contact the maintainer Andrew Chang at andrew@golden.co or [@achang1618](https://twitter.com/achang1618) for any quesions or comments.

For all other support, please reach out to support@golden.co.

Follow [@golden](https://twitter.com/Golden) to keep up with additional news!

## License

This project is licensed under the terms of the Apache 2.0 license