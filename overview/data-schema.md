# Data Schema

{% embed url="https://dapp.golden.xyz/graphiql" %}

Golden's GraphQL API is introspective meaning you can retrieve details about the GraphQL schema itself

Try the following in the GraphiQL explorer to retrieve details abou the schema and explicit types:

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

Introspection to actually used to generate python modules for our easy to use python wrapper.

{% content-ref url="../python/" %}
[python](../python/)
{% endcontent-ref %}
