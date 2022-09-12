# Resource Limitation

Resource limitations are necessary to protect Golden's GraphQL API from excessive calls.

## Rate Limiting

The following standards are applied to all GraphQL API calls:

* Up to 100 calls can be made per 5 minutes
* Calls cannot request more than 100,000 nodes
