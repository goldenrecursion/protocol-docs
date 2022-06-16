# Authentication

You'll need to retrieve an API key in any of the following ways below.

## Manually with Browser

1. Log in at [Golden](https://dapp.golden.xyz) by connecting your wallet.
2. Inspect your browser and refresh the page.
3. Go the `network` tab in the inspector.
4. Look for a call to the `/graphql` endpoint and click on it
5. Check out the request headers and find the "authorization" key.
6. Some queries will require authentication given your user account and wallet address.
7. Copy and paste the bearer token that looks like "Bearer ..." and paste it in the GraphiQL console request headers at the bottom

![](<../.gitbook/assets/examplejwt (2).png>)
