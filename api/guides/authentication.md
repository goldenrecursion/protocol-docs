# Authentication

Some queries and mutations will require authentication using your user account and wallet address. Note: When using the Python SDK you may retrieve your token programmatically.&#x20;

## Retrieve token using dApp

1. Log in at[ dapp.golden.xyz](https://dapp.golden.xyz) by connecting your wallet.
2. Navigate to your [profile page](https://dapp.golden.xyz/profile).
3. Click the 'GraphQL Token' toggle. Your token will be shown below.&#x20;
4. To use this token, add it to your request headers.  Example headers: `{ "Authorization": "Bearer <your-token-here>" }`. In [GraphiQL](https://dapp.golden.xyz/graphiql), these headers may be added at the bottom of the page (as shown below).

{% hint style="info" %}
To expire and refresh your authentication token click the 'Logout' button and reconnect your wallet to log back in. Your authentication token is session-based and designed to periodically expire.&#x20;
{% endhint %}

![User profile in dApp](../../.gitbook/assets/profile\_token.jpg)

![Example token headers in GraphiQL](../../.gitbook/assets/graphiql\_ex.jpg)

