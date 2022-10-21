---
description: Gain programmatic access to the Golden Protocol
---

# Authentication

Some queries and mutations will require authentication using your user account and wallet address. Note: When using the Python SDK you may retrieve your token programmatically.

## Retrieve JWT token using dApp

1. Log in at[ dapp.golden.xyz](https://dapp.golden.xyz) by connecting your wallet.
2. Navigate to your [profile page](https://dapp.golden.xyz/profile).
3. Click the 'GraphQL Token' toggle. Your token will be shown below.
4. To use this token, add it to your request headers. Example headers: `{ "Authorization": "Bearer <your-token-here>" }`. In [GraphiQL](https://dapp.golden.xyz/graphiql), these headers may be added at the bottom of the page (as shown below).

{% hint style="danger" %}
Your authentication token is session-based and designed to expire periodically. Unlike a traditional API key, it is good practice to retrieve a new JWT token frequently. &#x20;
{% endhint %}

![User profile in dApp](../.gitbook/assets/profile\_token.jpg)

![Example token headers in GraphiQL](../.gitbook/assets/graphiql\_ex.jpg)
