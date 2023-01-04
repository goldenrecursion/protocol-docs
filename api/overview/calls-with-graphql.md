# GraphQL Examples

Make sure you've authenticated with your wallet as shown in [authentication.md](../authentication.md "mention").

## GraphQL API Endpoints

The Golden GraphQL API has a **production** endpoint at: [**`https://dapp.golden.xyz/graphql`**](https://dapp.golden.xyz/graphql)

The Golden GraphQL API has a **testing** endpoint at: [**`https://sandbox.dapp.golden.xyz/graphql`**](https://dapp.golden.xyz/graphql)`.` See more at [testing-sandbox.md](../testing-sandbox.md "mention").

## Queries and Mutations

Queries and mutations are the two operations allowed in Golden's GraphQL API for retrieving and submitting data to Golden's protocol.

Here are some example queries and mutations:

```graphql
// Example for finding your account information
// useful for keeping track of your points available to perform actions

query UserInfo {
  currentUser {
    pointsAvailableForActions
    pendingPoints
    stakedPoints
    remainingSkips
  }
}
```

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
