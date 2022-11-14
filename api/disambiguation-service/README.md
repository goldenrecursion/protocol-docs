---
description: >-
  Golden protocol service to disambiguate and deduplicate triple submissions.
  Includes step-by-step tutorial on disambiguation of triples using the GraphQL
  API.
---

# Disambiguation Service

## Overview

The entity disambiguation service offers a method of querying the knowledge graph in order to identify specific entities based on input triple(s). This is an important service for those looking to accurately link new triple submissions to existing entities or prevent duplicate new entities from being created. This service is available through the GraphQL API and the Python SDK.&#x20;

## GraphQL API reference and technical information

The full API reference is available through the [GraphQL API visual interface](https://dapp.golden.xyz/graphiql?query=query%20DisambiguationQuery%20\{%20disambiguateTriples\(%20payload:%20\{%20triples:%20\[%20\{%20predicate:%20%22Name%22,%20object:%20%22Apple%22%20}%20\{%20predicate:%20%22Website%22,%20object:%20%22http://apple.com%22%20}%20\{%20predicate:%20%22Number%20of%20Employees%22,%20object:%20%22154000%22%20}%20]%20}%20\)%20\{%20entities%20\{%20id%20name%20date\_created%20distance%20reputation%20}%20}%20}) (GraphiQL). To view these docs click 'Docs' in the upper right corner of GraphiQL and search for `disambiguateTriples`. A JWT authentication token is not required to run this service. &#x20;



### Usage Guides

<table data-view="cards"><thead><tr><th></th><th></th><th data-hidden data-card-target data-type="content-ref"></th></tr></thead><tbody><tr><td><h2>Python SDK disambiguation tutorial</h2></td><td>A full walkthrough on how to use the Python SDK (<code>godel</code>) to disambiguate entities and triples.</td><td><a href="python-sdk-disambiguation-tutorial.md">python-sdk-disambiguation-tutorial.md</a></td></tr><tr><td><h2>GraphQL API disambiguation tutorial</h2></td><td>Learn how to use the disambiguation service through the GraphQL API</td><td><a href="graphql-api-disambiguation-tutorial.md">graphql-api-disambiguation-tutorial.md</a></td></tr></tbody></table>
