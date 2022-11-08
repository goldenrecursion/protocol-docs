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

## GraphQL API Tutorial

This is a short tutorial on how to use the disambiguation [GraphQL API](https://dapp.golden.xyz/graphiql) during triple submission. We will cover the basics, so we can successfully determine the subject id of the triples we want to submit.

### Setting the disambiguation target

For example, let's assume we have the following triples on [Apple](https://apple.com/) that we want to submit to the Graph:

* **Name:** Apple
* **Website:** [https://apple.com](https://apple.com)
* **Number of Employees**: 154,000

The above are triples corresponding to predicates already existing in the protocol's [schema](https://dapp.golden.xyz/schema).

### Basic entity disambiguation

If we want to disambiguate those triples, to get a list of existing entities in the graph that might be related, [we can run the following GraphQL query via the API](https://dapp.golden.xyz/graphiql?query=query%20DisambiguationQuery%20\{%20disambiguateTriples\(%20payload:%20\{%20triples:%20\[%20\{%20predicate:%20%22Name%22,%20object:%20%22Apple%22%20}%20\{%20predicate:%20%22Website%22,%20object:%20%22http://apple.com%22%20}%20\{%20predicate:%20%22Number%20of%20Employees%22,%20object:%20%22154000%22%20}%20]%20}%20\)%20\{%20entities%20\{%20id%20name%20date\_created%20distance%20reputation%20}%20}%20}) (_no login required_):

```graphql
query DisambiguationQuery {
  disambiguateTriples(
    payload: {
      triples: [
        { predicate: "Name", object: "Apple" }
        { predicate: "Website", object: "http://apple.com" }
        { predicate: "Number of Employees", object: "154000" }
      ]
    }
  ) {
    entities {
      id
      name
      date_created
      distance
      reputation
    }
  }
}
```

Which at the time of writing these lines, returns the following:

```graphql
{
    "data": {
        "disambiguateTriples": {
        "entities": [
            {
            "id": "debcb513-b842-4645-9856-2f4ea975002b",
            "name": "Apple (company)",
            "date_created": "2022-01-29T16:14:30.067541",
            "distance": 0.2222222222222222,
            "reputation": 1
            },
            {
            "id": "6d471830-4289-45ef-928c-7a3f87d4e031",
            "name": "Apple",
            "date_created": "2022-02-19T14:46:38.274665",
            "distance": 0.463768115942029,
            "reputation": 0.29754714586601044
            },
            {
            "id": "c651ed77-7ad6-48fe-b60a-077d9103f4cb",
            "name": "Apple",
            "date_created": "2022-04-15T11:25:55.464839",
            "distance": 0.5087719298245613,
            "reputation": 0.20715657580048066
            }
        ]
        }
    }
}
```

In this particular response, we've received, as potential disambiguation entities, the following:

1. [Apple Inc](https://dapp.golden.xyz/entity/debcb513-b842-4645-9856-2f4ea975002b)
2. [Apple Swap (Cryptocurrency)](https://dapp.golden.xyz/entity/6d471830-4289-45ef-928c-7a3f87d4e031)
3. [Apple NFT (Cryptocurrency)](https://dapp.golden.xyz/entity/c651ed77-7ad6-48fe-b60a-077d9103f4cb)

The API returns the results ranked by relevance, which, in this case, results in the correct entity being the number 1 result. The disambiguation service compares the submitted triples with already existing values on the graph, and then returns a list of entities that have similar values, ranked by a _distance_ and _reputation_ scores.

The _distance_ score of the response is a value between 0 and 1, where 0 indicates a perfect match between the submitted triples and what's already on the graph, while 1 would, on the contrary, indicate a total mismatch.

The _reputation_ score is a relative score to each particular disambiguation response, where the entity with the most reputation will always have the value **1**, whereas the remaining entities will be a fraction of that value. Conceptually, entity _reputation_ serves as a measure of how much effort has been placed in the creation of that given entity. Entities with more contributions, votes, backlinks, and older creation dates will be ranked higher than their more junior counterparts. We employ this system to discourage the addition of duplicates since, ideally, we would only have 1 entry in the graph for each canonical entity it represents.

Most of the time, out of all the candidate entities for disambiguation, we want to choose the one with the lowest distance ([debcb513-b842-4645-9856-2f4ea975002b](https://dapp.golden.xyz/entity/debcb513-b842-4645-9856-2f4ea975002b) in this case), since it is the closest match to our submission. If the results are within the same distance range (_≈±0.15_) though, it is useful to use _reputation_ (we want the highest value) and _date\_created_ (we want the oldest entity) to break the tie.

### Determining triple submissions after disambiguation

Apart from a list of possible candidate entities, the _disambiguateTriples()_ query can also return a diff listing which triples already exist on any given candidate, and which ones haven't been added. This is useful to inform the user which data has already been submitted and thus, avoid the creation of duplicate triples.

If we are interested in the diff, we can add it as part of the GraphQL request:

```graphql
query DisambiguationQuery {
  disambiguateTriples(
    payload: {
      triples: [
        { predicate: "Name", object: "Apple" }
        { predicate: "Website", object: "http://apple.com" }
        { predicate: "Number of Employees", object: "154000" }
      ]
    }
  ) {
    errors
    entities {
      id
      name
      date_created
      distance
      reputation
      diff {
        matches {
          id
          validation_status
          predicate
          object
        }
        inserts {
          predicate
          object
        }
      }
    }
  }
}
```

The response for the [debcb513-b842-4645-9856-2f4ea975002b](https://dapp.golden.xyz/entity/debcb513-b842-4645-9856-2f4ea975002b) entity, now returns additional information in the _diff_ property:

```graphql
{
    "id": "debcb513-b842-4645-9856-2f4ea975002b",
    "name": "Apple (company)",
    "date_created": "2022-01-29T16:14:30.067541",
    "distance": 0.222222222222222
    "reputation": 1,
    "diff": {
    "matches": [
        {
        "id": "55bc2c88-f81e-4908-9f53-bd4e0442d39c",
        "predicate": "Website",
        "validation_status": "PENDING",
        "object": "http://apple.com"
        },
        {
        "id": "c7e24fef-50a2-47de-9b65-74bf082d1153",
        "validation_status": "PENDING",
        "predicate": "Number of Employees",
        "object": "154000"
        }
    ],
    "inserts": [
        {
        "predicate": "Name",
        "object": "Apple"
        }
    ]
    }
}
```

In this case, we see that 2 of the 3 triples already exist in the graph (triple `55bc2c88-f81e-4908-9f53-bd4e0442d39c`and `c7e24fef-50a2-47de-9b65-74bf082d1153`) and both of them are in `PENDING` status, as the voting on them has not yet reached consensus.

The `‘Name’ → ‘Apple’` triple appears as an insert, since the current value on the entity is different (`‘Name’ → ‘Apple (company)’`).

### Recursive disambiguation

So far, we've only covered the case of predicates that have values as an object. That is, predicates that are not referencing another entity of the graph. But what happens if we have the following example?

* **Founder:** Steve Jobs
* **CEO:** Tim Cook

Both the [Founder](https://dapp.golden.xyz/predicate/71ad3d9e-e211-472b-a16d-861737c57ecd) and [CEO](https://dapp.golden.xyz/predicate/0a87e996-34b4-46ba-909a-70ab67b1f811) predicates take an entity as their object, so we can't perform a submission with just the name of the person.

Thankfully, we can still use the _disambiguateTriples()_ query. In this case, to get the reference for the object we want to submit it as a triple. For example, we could perform a search for Steve Jobs with the following query:

```graphql
query DisambiguationQuery {
  disambiguateTriples(
    payload: { triples: [{ predicate: "Name", object: "Steve Jobs" }] }
  ) {
    errors
    entities {
      id
      name
      date_created
      distance
      reputation
      diff {
        matches {
          id
          validation_status
          predicate
          object
        }
        inserts {
          predicate
          object
        }
      }
    }
  }
}
```

Which yields the following response:

```graphql
{
  "data": {
    "disambiguateTriples": {
      "errors": null,
      "entities": [
        {
          "id": "d8f80edd-a053-4d5d-9a24-eaf0d8fdfbad",
          "name": "Steve Jobs",
          "date_created": "2022-01-29T15:10:01.623848",
          "distance": 0,
          "reputation": 1,
          "diff": {
            "matches": [
              {
                "id": "300c6e27-40bd-49a5-8285-a69d8b39b74d",
                "validation_status": "PENDING",
                "predicate": "Name",
                "object": "Steve Jobs"
              }
            ],
            "inserts": []
          }
        }
      ]
    }
  }
}
```

So, we've found that `‘Name’ → Steve Jobs’` matches the entity [d8f80edd-a053-4d5d-9a24-eaf0d8fdfbad](https://dapp.golden.xyz/entity/d8f80edd-a053-4d5d-9a24-eaf0d8fdfbad) on the graph and we can now use that reference when we submit our triple.

If we have some additional information on the object, we can use those additional triples for a more accurate search. Let's try it with _Tim Cook_:

```graphql
query DisambiguationQuery {
  disambiguateTriples(
    payload: {
      triples: [
        { predicate: "Name", object: "Tim Cook" }
        { predicate: "Twitter URL", object: "https://twitter.com/tim_cook" }
        { predicate: "Date of Birth", object: "1955-02-24" }
      ]
    }
  ) {
    errors
    entities {
      id
      name
      date_created
      distance
      reputation
      diff {
        matches {
          id
          validation_status
          predicate
          object
        }
        inserts {
          predicate
          object
        }
      }
    }
  }
}
```

This returns:

```graphql
{
  "data": {
    "disambiguateTriples": {
      "errors": null,
      "entities": [
        {
          "id": "87047cc6-e3bc-4f7b-8c6e-e880ded391b4",
          "name": "Tim Cook",
          "date_created": "2022-08-29T14:53:50.775486",
          "distance": 0.3333333333333333,
          "reputation": 0.35450469592382217,
          "diff": {
            "matches": [
              {
                "id": "cbd2f700-62b2-4e5b-95ca-a02541c62d6f",
                "validation_status": "PENDING",
                "predicate": "Name",
                "object": "Tim Cook"
              },
              {
                "id": "9efefc66-ff6a-4743-911a-a38785c7e376",
                "validation_status": "PENDING",
                "predicate": "Twitter URL",
                "object": "https://twitter.com/tim_cook"
              }
            ],
            "inserts": [
              {
                "predicate": "Date of Birth",
                "object": "1955-02-24"
              }
            ]
          }
        },
        {
          "id": "68958b31-653b-4071-a47e-e344646a2826",
          "name": "Alain Prost",
          "date_created": "2022-06-20T16:02:04.707419",
          "distance": 0.36007130124777187,
          "reputation": 0.3559179064486544,
          "diff": {
            "matches": [
              {
                "id": "7160725f-fdd0-467e-815a-bb7d9edcad49",
                "validation_status": "PENDING",
                "predicate": "Date of Birth",
                "object": "1955-02-24"
              }
            ],
            "inserts": [
              {
                "predicate": "Name",
                "object": "Tim Cook"
              },
              {
                "predicate": "Twitter URL",
                "object": "https://twitter.com/tim_cook"
              }
            ]
          }
        },
        ...
      ]
    }
  }
}
```

In this case, we've found that _Tim Cook_ on the graph is represented by the entity [87047cc6-e3bc-4f7b-8c6e-e880ded391b4](https://dapp.golden.xyz/entity/87047cc6-e3bc-4f7b-8c6e-e880ded391b4). Additionally, we can also see that we could submit his birthday into the graph, since it is listed as an _insert_, while other persons such as _Alain Prost_ appear on the candidate list, as they share his same birthday, albeit at a higher _distance_ score.

### Summary

In this tutorial, we've covered the main uses of the _disambiguateTriples()_ query on the GraphQL API, which is the main mechanism to find the entity identifiers in the graph when we have some information about an entity, but we can't locate its reference.

Disambiguation is a key process during triple submission, as it prevents the creation of entity duplicates and increases the usefulness of the graph as a source of knowledge.

Finally, we've also covered how the different metrics of the _disambiguateTriples()_ query help us determine which of the results best matches our search, and how we can use the _diff_ it provides to adjust the subsequent data submission to avoid the creation of triple duplicates
