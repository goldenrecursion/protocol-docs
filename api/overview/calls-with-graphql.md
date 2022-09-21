# GraphQL Examples

Make sure you've authenticated with your wallet in the previous step.

## The GraphQL Endpoint

The Golden GraphQL API has a single endpoint that will always remain constant: [**`https://dapp.golden.xyz/graphql`**](https://dapp.golden.xyz/graphql)**``**

## Queries and Mutations

Queries and mutations are the two operations allowed in Golden's GraphQL API for retrieving and submitting data to Golden's protocol.

Here are some example queries and mutations:

```graphql
// Example for retrieving predicates

query MyQuery {
  predicates {
    edges {
      node {
        id
        name
      }
    }
  }
}
```

```graphql
// Example for submitting validation

mutation MyMutation {
  createValidation(
    input: {tripleId: "0a87e666-34b4-46ba-999a-70ab81b1f781", validationType: ACCEPTED}
  ) {
    validation {
      id
    }
  }
}
```

##
