---
description: >-
  A short guide on subscribing to realtime events emitted by the Golden protocol
  data verification process.
---

# Realtime Events API

## Overview

By using the real-time events API it is possible to subscribe to notification events from the triple verification process. This feature is designed to speed up the production of information complete entities, which increases the density and value of the knowledge graph.

**Use accepted triples as context to find further entity information**: The most common use case involves listening for events containing URL-based predicates. An acceptance event allows for scripts and systems to be run in order to identify additional information that can be collected from the newly verified URL. Eg. from a 'website URL' predicate it may be possible to find data on social URLs, company location, funding, etc.  &#x20;

&#x20;

## Event Types

Currently, the following events are emitted:

* `StatementPending`: whenever a statement gets submitted to the protocol
* `StatementAcceptConsensus`: whenever a statement gets accepted via a consensus vote
* `StatementRejectConsensus`: whenever a statement gets rejected via a consensus vote

## Prerequisites

* The events API requires authentication. Follow the steps from [Godel Authentication](https://docs.golden.xyz/godel-python-sdk/authentication) to get a JWT token.
* Install the Python dependencies
  * `pip install websockets gcl`
* Websocket URL: `wss://events.api.golden.xyz/graphql/realtime`

## How to subscribe to events

The events are exposed via an AWS AppSync web socket endpoint.  If you want to understand the protocol details, please visit [https://aws.amazon.com/blogs/mobile/appsync-websockets-python/](https://aws.amazon.com/blogs/mobile/appsync-websockets-python/).\
\
Below you can find an example python script that connects to the real-time web socket endpoint and listens for events.&#x20;

```python
import asyncio
from typing import Any, Optional, Dict

from gql import Client, gql
from gql.transport.appsync_websockets import AppSyncWebsocketsTransport
from gql.transport.appsync_auth import AppSyncAuthentication
from urllib.parse import urlparse

# Fill with your JWT TOKEN
AUTH_HEADER = 'Bearer jwtTokenToBeFilled'
WSS_URL = 'wss://events.api.golden.xyz/graphql/realtime'

class GoldenAuthentication(AppSyncAuthentication):
    def __init__(self, host: str, auth_header: str) -> None:
        self._host = host
        self._auth_header = auth_header

    def get_headers(
        self, data: Optional[str] = None, headers: Optional[Dict[str, Any]] = None
    ) -> Dict[str, Any]:
        return {"host": self._host, "Authorization": self._auth_header}

async def main():
    host = str(urlparse(WSS_URL).netloc)
    transport = AppSyncWebsocketsTransport(
      url=WSS_URL, auth=GoldenAuthentication(host, AUTH_HEADER)
    )

    async with Client(transport=transport) as session:
        subscription = gql(
            """
              subscription SubscribeToEvents {
                event {
                  eventId
                  statement {
                    id
                    subjectId
                    predicateId
                    objectValue
                    objectEntityId
                  }
                }
              }
            """
        )

        print("Waiting for messages...")
        async for result in session.subscribe(subscription):
            print(result["event"])


asyncio.run(main())
```

If you fill the AUTH\_HEADER variable with your JWT token and execute the above script you will get events such as the following.&#x20;

```
{'eventId': 'StatementAcceptConsensus', 'statement': {'id': '78881237-69b3-4515-9ea9-1a8bb1a3ed42', 'subjectId': 'b6532cd7-31c0-4ded-a689-40ffa8702442', 'predicateId': '2f30a94e-cd5e-496f-bec8-01bfb01da128', 'objectValue': '1891-03-04', 'objectEntityId': None}}
{'eventId': 'StatementAcceptConsensus', 'statement': {'id': '5510eb4a-53f0-4d64-b829-e01ccd24dda8', 'subjectId': '006a5719-730a-4aaa-bf85-8e9d403641ac', 'predicateId': '896c9d73-a08b-44ae-8e4e-2a02e5c1e546', 'objectValue': 'https://penmypaperservice.edublogs.org/', 'objectEntityId': None}}
{'eventId': 'StatementAcceptConsensus', 'statement': {'id': 'e9363da1-30e9-4c6b-8e36-ac8de12f5b80', 'subjectId': '5054dca4-44c9-4dfe-9204-98988a27863a', 'predicateId': '71ad3d9e-e211-472b-a16d-861737c57ecd', 'objectValue': None, 'objectEntityId': '63066b35-8f02-4d86-bfe2-37938bd42fee'}}
{'eventId': 'StatementAcceptConsensus', 'statement': {'id': 'c93a6c34-91b6-4ab6-accf-0fc2dd283999', 'subjectId': 'd60b61ec-7dd4-4161-b738-27ede96e91a3', 'predicateId': '2f30a94e-cd5e-496f-bec8-01bfb01da128', 'objectValue': '1827-11-13', 'objectEntityId': None}}Re{'eventId': 'StatementAcceptConsensus',
```

#### Return type details

```json
{
   'eventId': 'StatementAcceptConsensus', // Event that was emitted (accept or reject)
   'statement': {
        'id': '78881237-69b3-4515-9ea9-1a8bb1a3ed42', // ID of the statement
        'subjectId': 'b6532cd7-31c0-4ded-a689-40ffa8702442', // ID of the subject
        'predicateId': '2f30a94e-cd5e-496f-bec8-01bfb01da128', // The ID of the predicate
        'objectValue': '1891-03-04', // The object value will populate here
        'objectEntityId': None // If the object is an entity the ID will populate here 
    }
}
```

### You can play with the above script using [this Colab notebook](https://colab.research.google.com/drive/1T7xzkNZswT-3uOqCF7MMdO12IqydG5XP?usp=sharing).

##
