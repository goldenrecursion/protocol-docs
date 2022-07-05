# Data Schema

Golden's GraphQL API is introspective, meaning it can be used to retrieve details about the GraphQL schema itself.

{% embed url="https://dapp.golden.xyz/graphiql" %}

Try the following queries in the [GraphiQL](https://dapp.golden.xyz/graphiql) explorer to retrieve details about the schema and explicit types:

```graphql
query {
  __schema {
    types {
      name
      description
      fields {
        name
      }
    }
  }
}
```

```graphql
query {
  __type(name: "Entity") {
    name
    kind
    description
    fields {
      name
    }
  }
}
```
