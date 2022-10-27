# Godel Reference

### `GoldenAPI` [¶](broken-reference) <a href="#godel.api.goldenapi" id="godel.api.goldenapi"></a>

#### `add_triple_to_entity(entity_id, predicate_id, citation_url, object_entity_id=None, object_value=None, **kwargs)` [¶](broken-reference) <a href="#godel.api.goldenapi.add_triple_to_entity" id="godel.api.goldenapi.add_triple_to_entity"></a>

_WARNING: To be deprecated in near future version_

Add a triple given the subject enity id, predicate id, object id/value, and citation url

**Parameters:**

| Name               | Type  | Description               | Default    |
| ------------------ | ----- | ------------------------- | ---------- |
| `entity_id`        | `str` | UID of the subject entity | _required_ |
| `predicate_id`     | `str` | UID of the predicate      | _required_ |
| `object_entity_id` | `str` | UID of the object entity  | `None`     |
| `citation_url`     | `str` | citation URL              | _required_ |

**Returns:**

| Name   | Type   | Description    |
| ------ | ------ | -------------- |
| `dict` | `dict` | created triple |

#### `authenticate(user_id, signature, **kwargs)` [¶](broken-reference) <a href="#godel.api.goldenapi.authenticate" id="godel.api.goldenapi.authenticate"></a>

Authenticate your signature with the GraphQL API

**Parameters:**

| Name           | Type  | Description                | Default    |
| -------------- | ----- | -------------------------- | ---------- |
| `user_id(str)` |       | Ether wallet address (Hex) | _required_ |
| `signature`    | `str` | Signed auth message (Hex)  | _required_ |

**Returns:**

| Name   | Type   | Description                   |
| ------ | ------ | ----------------------------- |
| `dict` | `dict` | payload with JWT bearer token |

#### `create_entity(create_entity_input, **kwargs)` [¶](broken-reference) <a href="#godel.api.goldenapi.create_entity" id="godel.api.goldenapi.create_entity"></a>

Create an entity given the MDTs/statement inputs required for entity creation.

**Parameters:**

| Name    | Type                | Description         | Default    |
| ------- | ------------------- | ------------------- | ---------- |
| `input` | `CreateEntityInput` | Create Entity input | _required_ |

**Returns:**

| Name   | Type | Description    |
| ------ | ---- | -------------- |
| `dict` |      | created entity |

#### `create_statement(create_statement_input, **kwargs)` [¶](broken-reference) <a href="#godel.api.goldenapi.create_statement" id="godel.api.goldenapi.create_statement"></a>

Create statement triple given the statement input combinations of subject entity id, predicate id, object id/value, and citation url

**Parameters:**

| Name    | Type                   | Description                   | Default    |
| ------- | ---------------------- | ----------------------------- | ---------- |
| `input` | `CreateStatementInput` | Create statement record input | _required_ |

**Returns:**

| Name   | Type | Description       |
| ------ | ---- | ----------------- |
| `dict` |      | created statement |

#### `entity_by_golden_id(golden_id, **kwargs)` [¶](broken-reference) <a href="#godel.api.goldenapi.entity_by_golden_id" id="godel.api.goldenapi.entity_by_golden_id"></a>

Retrieve entity by golden.com entity ID

**Parameters:**

| Name        | Type  | Description        | Default    |
| ----------- | ----- | ------------------ | ---------- |
| `golden_id` | `str` | must be an integer | _required_ |

**Returns:**

| Name   | Type   | Description   |
| ------ | ------ | ------------- |
| `dict` | `dict` | _description_ |

#### `entity_by_name(name, first=10, **kwargs)` [¶](broken-reference) <a href="#godel.api.goldenapi.entity_by_name" id="godel.api.goldenapi.entity_by_name"></a>

Retrieve entity by name

**Parameters:**

| Name    | Type  | Description                        | Default    |
| ------- | ----- | ---------------------------------- | ---------- |
| `name`  | `str` | name of entity                     | _required_ |
| `first` | `int` | number of results. Defaults to 20. | `10`       |

**Returns:**

| Name   | Type   | Description    |
| ------ | ------ | -------------- |
| `dict` | `dict` | search results |

#### `entity_detail(id, **kwargs)` [¶](broken-reference) <a href="#godel.api.goldenapi.entity_detail" id="godel.api.goldenapi.entity_detail"></a>

Retrieve entity details, includes rich template and statement values

**Parameters:**

| Name | Type  | Description              | Default    |
| ---- | ----- | ------------------------ | ---------- |
| `id` | `str` | id of entity to retrieve | _required_ |

**Returns:**

| Name   | Type   | Description         |
| ------ | ------ | ------------------- |
| `dict` | `dict` | Entity with details |

#### `entity_search(name, first=20, **kwargs)` [¶](broken-reference) <a href="#godel.api.goldenapi.entity_search" id="godel.api.goldenapi.entity_search"></a>

Base entity search with SQL

**Parameters:**

| Name    | Type            | Description                        | Default    |
| ------- | --------------- | ---------------------------------- | ---------- |
| `name`  | `str`           | name of entity searched            | _required_ |
| `first` | `int, Optional` | number of results. Defaults to 20. | `20`       |

**Returns:**

| Name   | Type   | Description    |
| ------ | ------ | -------------- |
| `dict` | `dict` | search results |

#### `entity_with_triples(id)` [¶](broken-reference) <a href="#godel.api.goldenapi.entity_with_triples" id="godel.api.goldenapi.entity_with_triples"></a>

Retrieve entity with both entity to value triples and entity to entity triples

**Parameters:**

| Name        | Type  | Description              | Default    |
| ----------- | ----- | ------------------------ | ---------- |
| `entity_id` | `str` | id of entity to retrieve | _required_ |

**Returns:**

| Name   | Type   | Description         |
| ------ | ------ | ------------------- |
| `dict` | `dict` | Entity with triples |

#### `generate_variables(op, params)` `classmethod` [¶](broken-reference) <a href="#godel.api.goldenapi.generate_variables" id="godel.api.goldenapi.generate_variables"></a>

Helper for retrieving variable names from operation and making variable object for endpoint

**Parameters:**

| Name     | Type        | Description                  | Default    |
| -------- | ----------- | ---------------------------- | ---------- |
| `op`     | `Operation` | sgqlc operation              | _required_ |
| `params` | `dict`      | variable values from methods | _required_ |

**Returns:**

| Name   | Type   | Description            |
| ------ | ------ | ---------------------- |
| `dict` | `dict` | dict of variable names |

#### `get_authentication_message(user_id, **kwargs)` [¶](broken-reference) <a href="#godel.api.goldenapi.get_authentication_message" id="godel.api.goldenapi.get_authentication_message"></a>

Retrieve auth message to sign with your wallet to verify your role and account

**Parameters:**

| Name      | Type  | Description                | Default    |
| --------- | ----- | -------------------------- | ---------- |
| `user_id` | `str` | Ether wallet address (Hex) | _required_ |

**Returns:**

| Name   | Type   | Description                 |
| ------ | ------ | --------------------------- |
| `dict` | `dict` | payload with string message |

#### `get_authentication_token(user_id, wallet_private_key, **kwargs)` [¶](broken-reference) <a href="#godel.api.goldenapi.get_authentication_token" id="godel.api.goldenapi.get_authentication_token"></a>

Convenience method for calling `getAuthenticationMessage` and `authenticate` given a user's wallet address and private key to obtain a JWT bearer token. Private key param is never sent to the API. This package only uses it to generate your signature using web3.py

Calling this method will automatically set self.jwt\_token which will be used in your graphql request headers.

**Parameters:**

| Name           | Type  | Description          | Default    |
| -------------- | ----- | -------------------- | ---------- |
| `user_id(str)` |       | Wallet address (Hex) | _required_ |
| `sign`         | `str` | Wallet private key   | _required_ |

**Returns:**

| Name  | Type  | Description           |
| ----- | ----- | --------------------- |
| `str` | `str` | your JWT bearer token |

#### `get_triple_for_validation(**kwargs)` [¶](broken-reference) <a href="#godel.api.goldenapi.get_triple_for_validation" id="godel.api.goldenapi.get_triple_for_validation"></a>

Get triple for verification

**Returns:**

| Name   | Type   | Description                   |
| ------ | ------ | ----------------------------- |
| `dict` | `dict` | data contain triple to verify |

#### `predicate_by_id(id)` [¶](broken-reference) <a href="#godel.api.goldenapi.predicate_by_id" id="godel.api.goldenapi.predicate_by_id"></a>

Get all predicates

**Returns:**

| Name   | Type   | Description                  |
| ------ | ------ | ---------------------------- |
| `dict` | `dict` | List of available predicates |

#### `predicate_by_name(name, **kwargs)` [¶](broken-reference) <a href="#godel.api.goldenapi.predicate_by_name" id="godel.api.goldenapi.predicate_by_name"></a>

Get predicate given name

**Parameters:**

| Name   | Type  | Description      | Default    |
| ------ | ----- | ---------------- | ---------- |
| `name` | `str` | name of prediate | _required_ |

**Returns:**

| Name   | Type   | Description               |
| ------ | ------ | ------------------------- |
| `dict` | `dict` | predicate object returned |

#### `predicates()` [¶](broken-reference) <a href="#godel.api.goldenapi.predicates" id="godel.api.goldenapi.predicates"></a>

Get all predicates

**Returns:**

| Name   | Type   | Description                  |
| ------ | ------ | ---------------------------- |
| `dict` | `dict` | List of available predicates |

#### `query(query='', headers={})` [¶](broken-reference) <a href="#godel.api.goldenapi.query" id="godel.api.goldenapi.query"></a>

Generic method to query graphql endpoint

#### `set_jwt_token(jwt_token='')` [¶](broken-reference) <a href="#godel.api.goldenapi.set_jwt_token" id="godel.api.goldenapi.set_jwt_token"></a>

Set your token to start accessing the API with your wallet/role

**Parameters:**

| Name        | Type  | Description                | Default |
| ----------- | ----- | -------------------------- | ------- |
| `jwt_token` | `str` | JWT Token. Defaults to "". | `''`    |

#### `templates()` [¶](broken-reference) <a href="#godel.api.goldenapi.templates" id="godel.api.goldenapi.templates"></a>

Get all templates

**Returns:**

| Name   | Type   | Description                 |
| ------ | ------ | --------------------------- |
| `dict` | `dict` | List of available templates |

#### `to_camel_case(snake_str)` `staticmethod` [¶](broken-reference) <a href="#godel.api.goldenapi.to_camel_case" id="godel.api.goldenapi.to_camel_case"></a>

Convert snake str to camel case since all params should eventually adhere to graphql camel case

**Parameters:**

| Name        | Type  | Description  | Default    |
| ----------- | ----- | ------------ | ---------- |
| `snake_str` | `str` | snake string | _required_ |

**Returns:**

| Name  | Type  | Description      |
| ----- | ----- | ---------------- |
| `str` | `str` | lower camel case |

#### `unvalidated_triple()` [¶](broken-reference) <a href="#godel.api.goldenapi.unvalidated_triple" id="godel.api.goldenapi.unvalidated_triple"></a>

Get all templates

**Returns:**

| Name   | Type   | Description                 |
| ------ | ------ | --------------------------- |
| `dict` | `dict` | List of available templates |

### `get_godel_version()` [¶](broken-reference) <a href="#godel.api.get_godel_version" id="godel.api.get_godel_version"></a>

Grabs the version of Godel from builtin importlib library

**Returns:**

| Name  | Type  | Description                                        |
| ----- | ----- | -------------------------------------------------- |
| `str` | `str` | version name, unknown if fails to dynamically pull |
