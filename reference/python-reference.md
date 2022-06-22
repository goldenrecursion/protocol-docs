# Python Reference



<details>

<summary>Source code in <code>dapp_api/api.py</code></summary>

```
class GoldenAPI:

    def __init__(self, url: str = "https://dapp.golden.xyz/graphql", jwt_token: str = ""):
        self.url = url
        self.jwt_token = jwt_token
        self.headers = {'Authorization': f'Bearer {jwt_token}'} if jwt_token else {}
        self.endpoint = HTTPEndpoint(self.url, self.headers)

    #####################
    ## Utility Methods ##
    #####################

    def set_jwt_token(self, jwt_token: str = "") -> None:
        """Set your token to start accessing the API with your wallet/role

        Args:
            jwt_token (str): JWT Token. Defaults to "".
        """
        self.jwt_token = jwt_token
        self.headers = {'Authorization': f'Bearer {jwt_token}'} if jwt_token else {}
        self.endpoint = HTTPEndpoint(self.url, self.headers)

    def query(self, query: str = "", headers: dict = {}):
        """ Generic method to query graphql endpoint
        """
        r = requests.post(self.url, json={"query": query}, headers=headers)
        print(r.status_code)
        if r.status_code==400:
            logger.warning(r)
        return r.json()

    @classmethod
    def generate_variables(cls, op: Operation, params: dict) -> dict:
        """Helper for retrieving variable names from operation
        and making variable object for endpoint

        Args:
            op (Operation): sgqlc operation
            params dict: variable values from methods

        Returns:
            dict: dict of variable names
        """
        var_args = op._Operation__args
        variables = {}
        for name in map(lambda x: x.strip("$"), var_args):
            variables[name] = None  # Set placeholder
        for p, v in params.items():
            p = cls.to_camel_case(p)
            if p in variables:
                variables[p] = v
        return variables

    @staticmethod
    def to_camel_case(snake_str: str) -> str:
        """Convert snake str to camel case since all params
        should eventually adhere to graphql camel case

        Args:
            snake_str (str): snake string

        Returns:
            str: lower camel case
        """
        components = snake_str.split('_')
        return components[0] + ''.join(x.title() for x in components[1:])


    ####################
    ## Authentication ##
    ####################

    # Retrieve JWT Token

    def get_authentication_message(self, user_id: str, **kwargs) -> dict:
        """Retrieve auth message to sign with your wallet to verify your role and account

        Args:
            user_id (str): Ether wallet address (Hex)

        Returns:
            dict: payload with string message
        """
        params = locals()
        params.pop("kwargs")
        params.pop("self")
        params.update(kwargs)
        op = GetAuthenticationMessageOperations.mutation.get_authentication_message
        variables = self.generate_variables(op, params)
        data = self.endpoint(op, variables)
        return data

    def authenticate(self, user_id: str, signature: str, **kwargs) -> dict:
        """Authenticate your signature with the GraphQL API

        Args:
            user_id(str): Ether wallet address (Hex)
            signature (str): Signed auth message (Hex)

        Returns:
            dict: payload with JWT bearer token
        """
        params = locals()
        params.pop("kwargs")
        params.pop("self")
        params.update(kwargs)
        op = AuthenticateOperations.mutation.authenticate
        variables = self.generate_variables(op, params)
        data = self.endpoint(op, variables)
        return data

    def get_authentication_token(self, user_id: str, wallet_private_key: str, **kwargs) -> str:
        """ Convenience method for calling `getAuthenticationMessage` and `authenticate`
        given a user's wallet address and private key to obtain a JWT bearer token.
        Private key param is never sent to the API. This package only uses it to generate
        your signature using web3.py

        Calling this method will automatically set self.jwt_token which will be used in
        your graphql request headers.

        Args:
            user_id(str): Wallet address (Hex)
            sign (str): Wallet private key

        Returns:
            str: your JWT bearer token
        """
        try:
            from web3.auto import w3
            from eth_account.messages import encode_defunct
        except ModuleNotFoundError:
            return
        message_response = self.get_authentication_message(user_id=user_id)
        try:
            message_string = message_response["data"]["getAuthenticationMessage"]["string"]
        except:
            return message_response
        message = encode_defunct(text=message_string)
        signed_message = w3.eth.account.sign_message(message, private_key=wallet_private_key)
        signature = signed_message.signature.hex()
        auth_token = self.authenticate(user_id=user_id, signature=signature)
        try:
            self.set_jwt_token(jwt_token=auth_token["data"]["authenticate"]["jwtToken"])
        except:
            return auth_token
        return auth_token

    #############
    ## Queries ##
    #############


    # Entities

    def entity_by_name(self, name: str, first: int= 10, **kwargs) -> dict:
        """Retrieve entity by name

        Args:
            name (str): name of entity
            first (int, optional): number of results. Defaults to 20.

        Returns:
            dict: search results
        """
        params = locals()
        params.pop("kwargs")
        params.pop("self")
        params.update(kwargs)
        op = EntityByNameOperations.query.entity_by_name
        variables = self.generate_variables(op, params)
        data = self.endpoint(op, variables)
        return data

    def entity_search(self, name: str, first: int = 20, **kwargs) -> dict:
        """Base entity search with SQL

        Args:
            name (str): name of entity searched 
            first (int, Optional): number of results. Defaults to 20.

        Returns:
            dict: search results
        """
        params = locals()
        params.pop("kwargs")
        params.pop("self")
        params.update(kwargs)
        op = EntitySearchOperations.query.entity_search
        variables = self.generate_variables(op, params)
        data = self.endpoint(op, variables)
        return data

    # Predicates

    def predicate(self, name: str, **kwargs) -> dict:
        """Get predicate given name

        Args:
            name (str): name of prediate

        Returns:
            dict: predicate object returned
        """
        params = locals()
        params.pop("kwargs")
        params.pop("self")
        params.update(kwargs)
        op = PredicateOperations.query.predicate
        variables = self.generate_variables(op, params)
        data = self.endpoint(op, variables)
        return data

    def predicates(self) -> dict:
        """ Get all predicates

        Returns:
            dict: List of available predicates
        """        
        query = """query MyQuery {
            predicates {
              edges {
                node {
                  id
                  name
                  objectType
                }
              }
            }
        }"""
        variables = {}
        data = self.endpoint(query, variables)
        return data


    # Templates
    def templates(self) -> dict:
        """ Get all templates

        Returns:
            dict: List of available templates
        """
        query = """query MyQuery {
            templates {
              edges {
                node {
                  id
                  entityId 
                  entity {
                    name
                    description
                  }
                }
              }
            }
        }"""
        variables = {}
        data = self.endpoint(query, variables)
        return data


    # Validation

    def get_triple_for_validation(self, **kwargs) -> dict:
        """Get triple for validation

        Returns:
            dict: data contain triple to validate
        """
        op = GetTripleForValidationOperations.query.get_triple_for_validation
        data = self.endpoint(op)
        return data

    def unvalidated_triple(self) -> dict:
        """ Get all templates

        Returns:
            dict: List of available templates
        """
        query = """query MyQuery {
            unvalidatedTriple {
              ... on Statement {
                id
                citationUrl
                dateCreated
                objectEntityId
                subjectId
                userId
                predicateId
              }
            }
        }"""
        variables = {}
        data = self.endpoint(query, variables)
        return data



    ###############
    ## Mutations ##
    ###############

    # Entity Submissions
    def create_entity(self, input: str, **kwargs):

        # TODO: Debug why entity link fragment isn't registered...for now use query
        # data = self.query(query=f"""
        # mutation MyMutation {{
        #   createEntity(input: {{name: "{name}"}}) {{
        #     entity {{
        #       id
        #     }}
        #   }}
        # }}
        # """,
        # headers=self.headers,
        # )
        # return data

        params = locals()
        params.pop("kwargs")
        params.pop("self")
        params.update(kwargs)
        op = CreateEntityOperations.mutation.create_entity
        variables = self.generate_variables(op, params)
        data = self.endpoint(op, variables)
        return data

    # Triples Submissions

    def add_triple_to_entity_by_id(self, entity_id: str, predicate_id: str, object_entity_id: str, citation_url: str, **kwargs) -> dict:
        """Add a triple given the subject enity id, predicate id, subject id, and citation url

        Args:
            entity_id (str): UID of the subject entity
            predicate_id (str): UID of the predicate
            object_entity_id (str): UID of the object entity
            citation_url (str): citation URL

        Returns:
            dict: created triple
        """
        params = locals()
        params.pop("kwargs")
        params.pop("self")
        params.update(kwargs)
        op = AddTripleToEntityByIdOperations.mutation.add_triple_to_entity_by_id
        variables = self.generate_variables(op, params)
        data = self.endpoint(op, variables)
        return data

    def add_triple_to_entity_by_id(self, entity_id: str, predicate_id: str, object_entity_id: str, citation_url: str, **kwargs) -> dict:
        """Add a triple given the subject enity id, predicate id, subject id, and citation url

        Args:
            entity_id (str): UID of the subject entity
            predicate_id (str): UID of the predicate
            object_entity_id (str): UID of the object entity
            citation_url (str): citation URL

        Returns:
            dict: created triple
        """
        params = locals()
        params.pop("kwargs")
        params.pop("self")
        params.update(kwargs)
        op = AddTripleToEntityByIdOperations.mutation.add_triple_to_entity_by_id
        variables = self.generate_variables(op, params)
        data = self.endpoint(op, variables)
        return data

    # TODO: Add value triples to be added in the API
    def add_triple_by_numerical_triple(self, subject_entity_id: str, predicate_id: str, object_numerical: Union[int, float]) -> None:
        """Submit numerical object triple

        Args:
            subject_entity_id (str): UID of subject entity
            predicate_id (str): UID of predicate
            object_numerical (Union[int, float]): _description_
        """        
        pass

    def submit_text_triple(self, subject_entity_id: str, predicate_id: str, object_text: str) -> None:
        """Submit text object triple

        Args:
            subject_entity_id (str): UID of subject entity
            predicate_id (str): UID of predicate
            object_text (str): _description_
        """
        pass

    def submit_date_triple(self, subject_entity_id: str, predicate_id: str, object_date: dict) -> None:
        """Submit date object triple

        Args:
            subject_entity_id (str): UID of subject entity
            predicate_id (str): UID of predicate
            object_date (dict): _description_
        """
        pass

    # Validation

    def create_validation(self, triple_id: str, validation_type: str, **kwargs) -> dict:
        params = locals()
        params.pop("kwargs")
        params.pop("self")
        params.update(kwargs)
        op = CreateValidationOperations.mutation.create_validation
        variables = self.generate_variables(op, params)
        data = self.endpoint(op, variables)
        return data


    # Raw Queries, mostly to be deprecated since they don't come directly from the dApp API schemas

    def submit_entity_triple(self, subject_entity_id: str, predicate_id: str, object_entity_id: str, citation_url: str) -> None:
        """Submit entity object triple

        Args:
            subject_entity_id (str): UID of subject entity
            predicate_id (str): UID of predicate
            object_entity_id (str): 

        Example
        """
        query = f"""mutation AddTripleToEntityById {{
		  createTriple(
		    input: {{
		      triple: {{
		        subjectId: "{subject_entity_id}"
		        predicateId: "{predicate_id}"
		        objectEntityId: "{object_entity_id}"
		        citationUrl: "{citation_url}"
		      }}
		    }}
		  ) {{
		    triple {{
		      id
		    }}
		  }}
        }}"""
        r = requests.post(self.url, json={"query": query})
        print(r.status_code)
        print(r.text)
        return r.json()

    def get_predicates(self) -> dict:
        query = """query MyQuery {
            predicates {
            edges {
              node {
                id
                name
              }
            }
          }
        }"""
        r = requests.post(self.url, json={"query": query})
        print(r.status_code)
        print(r.text)
        return r.json()

    def get_predicate_by_name(self, name: str) -> dict:
        query = f"""query MyQuery {{
            predicateByName(name: "{name}") {{
              id
              name
          }}
        }}"""
        r = requests.post(self.url, json={"query": query})
        print(r.status_code)
        print(r.text)
        return r.json()

    # Triples Retrieval 
    def get_pending_triples(self) -> None:
        """Get all pending triples
        TODO: Impractical when there's more data. Need filter/search.

        """
        query = """query MyQuery {
                  unvalidatedTriple {
                    id
                    name
                  }
                }"""
        r = requests.post(self.url, json={"query": query})
        print(r.status_code)
        print(r.text)
        return r.json()
```

</details>

#### `add_triple_by_numerical_triple(self, subject_entity_id, predicate_id, object_numerical)` [¶](broken-reference) <a href="#dapp_api.api.goldenapi.add_triple_by_numerical_triple" id="dapp_api.api.goldenapi.add_triple_by_numerical_triple"></a>

Submit numerical object triple

**Parameters:**

| Name                | Type                | Description           | Default    |
| ------------------- | ------------------- | --------------------- | ---------- |
| `subject_entity_id` | `str`               | UID of subject entity | _required_ |
| `predicate_id`      | `str`               | UID of predicate      | _required_ |
| `object_numerical`  | `Union[int, float]` | _description_         | _required_ |

<details>

<summary>Source code in <code>dapp_api/api.py</code></summary>

```
def add_triple_by_numerical_triple(self, subject_entity_id: str, predicate_id: str, object_numerical: Union[int, float]) -> None:
    """Submit numerical object triple

    Args:
        subject_entity_id (str): UID of subject entity
        predicate_id (str): UID of predicate
        object_numerical (Union[int, float]): _description_
    """        
    pass
```

</details>

#### `add_triple_to_entity_by_id(self, entity_id, predicate_id, object_entity_id, citation_url, **kwargs)` [¶](broken-reference) <a href="#dapp_api.api.goldenapi.add_triple_to_entity_by_id" id="dapp_api.api.goldenapi.add_triple_to_entity_by_id"></a>

Add a triple given the subject enity id, predicate id, subject id, and citation url

**Parameters:**

| Name               | Type  | Description               | Default    |
| ------------------ | ----- | ------------------------- | ---------- |
| `entity_id`        | `str` | UID of the subject entity | _required_ |
| `predicate_id`     | `str` | UID of the predicate      | _required_ |
| `object_entity_id` | `str` | UID of the object entity  | _required_ |
| `citation_url`     | `str` | citation URL              | _required_ |

**Returns:**

| Type   | Description    |
| ------ | -------------- |
| `dict` | created triple |

<details>

<summary>Source code in <code>dapp_api/api.py</code></summary>

```
def add_triple_to_entity_by_id(self, entity_id: str, predicate_id: str, object_entity_id: str, citation_url: str, **kwargs) -> dict:
    """Add a triple given the subject enity id, predicate id, subject id, and citation url

    Args:
        entity_id (str): UID of the subject entity
        predicate_id (str): UID of the predicate
        object_entity_id (str): UID of the object entity
        citation_url (str): citation URL

    Returns:
        dict: created triple
    """
    params = locals()
    params.pop("kwargs")
    params.pop("self")
    params.update(kwargs)
    op = AddTripleToEntityByIdOperations.mutation.add_triple_to_entity_by_id
    variables = self.generate_variables(op, params)
    data = self.endpoint(op, variables)
    return data
```

</details>

#### `authenticate(self, user_id, signature, **kwargs)` [¶](broken-reference) <a href="#dapp_api.api.goldenapi.authenticate" id="dapp_api.api.goldenapi.authenticate"></a>

Authenticate your signature with the GraphQL API

**Parameters:**

| Name           | Type  | Description                | Default    |
| -------------- | ----- | -------------------------- | ---------- |
| `user_id(str)` |       | Ether wallet address (Hex) | _required_ |
| `signature`    | `str` | Signed auth message (Hex)  | _required_ |

**Returns:**

| Type   | Description                   |
| ------ | ----------------------------- |
| `dict` | payload with JWT bearer token |

<details>

<summary>Source code in <code>dapp_api/api.py</code></summary>

```
def authenticate(self, user_id: str, signature: str, **kwargs) -> dict:
    """Authenticate your signature with the GraphQL API

    Args:
        user_id(str): Ether wallet address (Hex)
        signature (str): Signed auth message (Hex)

    Returns:
        dict: payload with JWT bearer token
    """
    params = locals()
    params.pop("kwargs")
    params.pop("self")
    params.update(kwargs)
    op = AuthenticateOperations.mutation.authenticate
    variables = self.generate_variables(op, params)
    data = self.endpoint(op, variables)
    return data
```

</details>

#### `entity_by_name(self, name, first=10, **kwargs)` [¶](broken-reference) <a href="#dapp_api.api.goldenapi.entity_by_name" id="dapp_api.api.goldenapi.entity_by_name"></a>

Retrieve entity by name

**Parameters:**

| Name    | Type  | Description                        | Default    |
| ------- | ----- | ---------------------------------- | ---------- |
| `name`  | `str` | name of entity                     | _required_ |
| `first` | `int` | number of results. Defaults to 20. | `10`       |

**Returns:**

| Type   | Description    |
| ------ | -------------- |
| `dict` | search results |

<details>

<summary>Source code in <code>dapp_api/api.py</code></summary>

```
def entity_by_name(self, name: str, first: int= 10, **kwargs) -> dict:
    """Retrieve entity by name

    Args:
        name (str): name of entity
        first (int, optional): number of results. Defaults to 20.

    Returns:
        dict: search results
    """
    params = locals()
    params.pop("kwargs")
    params.pop("self")
    params.update(kwargs)
    op = EntityByNameOperations.query.entity_by_name
    variables = self.generate_variables(op, params)
    data = self.endpoint(op, variables)
    return data
```

</details>

#### `entity_search(self, name, first=20, **kwargs)` [¶](broken-reference) <a href="#dapp_api.api.goldenapi.entity_search" id="dapp_api.api.goldenapi.entity_search"></a>

Base entity search with SQL

**Parameters:**

| Name    | Type            | Description                        | Default    |
| ------- | --------------- | ---------------------------------- | ---------- |
| `name`  | `str`           | name of entity searched            | _required_ |
| `first` | `int, Optional` | number of results. Defaults to 20. | `20`       |

**Returns:**

| Type   | Description    |
| ------ | -------------- |
| `dict` | search results |

<details>

<summary>Source code in <code>dapp_api/api.py</code></summary>

```
def entity_search(self, name: str, first: int = 20, **kwargs) -> dict:
    """Base entity search with SQL

    Args:
        name (str): name of entity searched 
        first (int, Optional): number of results. Defaults to 20.

    Returns:
        dict: search results
    """
    params = locals()
    params.pop("kwargs")
    params.pop("self")
    params.update(kwargs)
    op = EntitySearchOperations.query.entity_search
    variables = self.generate_variables(op, params)
    data = self.endpoint(op, variables)
    return data
```

</details>

#### `generate_variables(op, params)` `classmethod` [¶](broken-reference) <a href="#dapp_api.api.goldenapi.generate_variables" id="dapp_api.api.goldenapi.generate_variables"></a>

Helper for retrieving variable names from operation and making variable object for endpoint

**Parameters:**

| Name     | Type        | Description                  | Default    |
| -------- | ----------- | ---------------------------- | ---------- |
| `op`     | `Operation` | sgqlc operation              | _required_ |
| `params` | `dict`      | variable values from methods | _required_ |

**Returns:**

| Type   | Description            |
| ------ | ---------------------- |
| `dict` | dict of variable names |

<details>

<summary>Source code in <code>dapp_api/api.py</code></summary>

```
@classmethod
def generate_variables(cls, op: Operation, params: dict) -> dict:
    """Helper for retrieving variable names from operation
    and making variable object for endpoint

    Args:
        op (Operation): sgqlc operation
        params dict: variable values from methods

    Returns:
        dict: dict of variable names
    """
    var_args = op._Operation__args
    variables = {}
    for name in map(lambda x: x.strip("$"), var_args):
        variables[name] = None  # Set placeholder
    for p, v in params.items():
        p = cls.to_camel_case(p)
        if p in variables:
            variables[p] = v
    return variables
```

</details>

#### `get_authentication_message(self, user_id, **kwargs)` [¶](broken-reference) <a href="#dapp_api.api.goldenapi.get_authentication_message" id="dapp_api.api.goldenapi.get_authentication_message"></a>

Retrieve auth message to sign with your wallet to verify your role and account

**Parameters:**

| Name      | Type  | Description                | Default    |
| --------- | ----- | -------------------------- | ---------- |
| `user_id` | `str` | Ether wallet address (Hex) | _required_ |

**Returns:**

| Type   | Description                 |
| ------ | --------------------------- |
| `dict` | payload with string message |

<details>

<summary>Source code in <code>dapp_api/api.py</code></summary>

```
def get_authentication_message(self, user_id: str, **kwargs) -> dict:
    """Retrieve auth message to sign with your wallet to verify your role and account

    Args:
        user_id (str): Ether wallet address (Hex)

    Returns:
        dict: payload with string message
    """
    params = locals()
    params.pop("kwargs")
    params.pop("self")
    params.update(kwargs)
    op = GetAuthenticationMessageOperations.mutation.get_authentication_message
    variables = self.generate_variables(op, params)
    data = self.endpoint(op, variables)
    return data
```

</details>

#### `get_authentication_token(self, user_id, wallet_private_key, **kwargs)` [¶](broken-reference) <a href="#dapp_api.api.goldenapi.get_authentication_token" id="dapp_api.api.goldenapi.get_authentication_token"></a>

Convenience method for calling `getAuthenticationMessage` and `authenticate` given a user's wallet address and private key to obtain a JWT bearer token. Private key param is never sent to the API. This package only uses it to generate your signature using web3.py

Calling this method will automatically set self.jwt\_token which will be used in your graphql request headers.

**Parameters:**

| Name           | Type  | Description          | Default    |
| -------------- | ----- | -------------------- | ---------- |
| `user_id(str)` |       | Wallet address (Hex) | _required_ |
| `sign`         | `str` | Wallet private key   | _required_ |

**Returns:**

| Type  | Description           |
| ----- | --------------------- |
| `str` | your JWT bearer token |

<details>

<summary>Source code in <code>dapp_api/api.py</code></summary>

```
def get_authentication_token(self, user_id: str, wallet_private_key: str, **kwargs) -> str:
    """ Convenience method for calling `getAuthenticationMessage` and `authenticate`
    given a user's wallet address and private key to obtain a JWT bearer token.
    Private key param is never sent to the API. This package only uses it to generate
    your signature using web3.py

    Calling this method will automatically set self.jwt_token which will be used in
    your graphql request headers.

    Args:
        user_id(str): Wallet address (Hex)
        sign (str): Wallet private key

    Returns:
        str: your JWT bearer token
    """
    try:
        from web3.auto import w3
        from eth_account.messages import encode_defunct
    except ModuleNotFoundError:
        return
    message_response = self.get_authentication_message(user_id=user_id)
    try:
        message_string = message_response["data"]["getAuthenticationMessage"]["string"]
    except:
        return message_response
    message = encode_defunct(text=message_string)
    signed_message = w3.eth.account.sign_message(message, private_key=wallet_private_key)
    signature = signed_message.signature.hex()
    auth_token = self.authenticate(user_id=user_id, signature=signature)
    try:
        self.set_jwt_token(jwt_token=auth_token["data"]["authenticate"]["jwtToken"])
    except:
        return auth_token
    return auth_token
```

</details>

#### `get_pending_triples(self)` [¶](broken-reference) <a href="#dapp_api.api.goldenapi.get_pending_triples" id="dapp_api.api.goldenapi.get_pending_triples"></a>

Get all pending triples TODO: Impractical when there's more data. Need filter/search.

<details>

<summary>Source code in <code>dapp_api/api.py</code></summary>

```
def get_pending_triples(self) -> None:
    """Get all pending triples
    TODO: Impractical when there's more data. Need filter/search.

    """
    query = """query MyQuery {
              unvalidatedTriple {
                id
                name
              }
            }"""
    r = requests.post(self.url, json={"query": query})
    print(r.status_code)
    print(r.text)
    return r.json()
```

</details>

#### `get_triple_for_validation(self, **kwargs)` [¶](broken-reference) <a href="#dapp_api.api.goldenapi.get_triple_for_validation" id="dapp_api.api.goldenapi.get_triple_for_validation"></a>

Get triple for validation

**Returns:**

| Type   | Description                     |
| ------ | ------------------------------- |
| `dict` | data contain triple to validate |

<details>

<summary>Source code in <code>dapp_api/api.py</code></summary>

```
def get_triple_for_validation(self, **kwargs) -> dict:
    """Get triple for validation

    Returns:
        dict: data contain triple to validate
    """
    op = GetTripleForValidationOperations.query.get_triple_for_validation
    data = self.endpoint(op)
    return data
```

</details>

#### `predicate(self, name, **kwargs)` [¶](broken-reference) <a href="#dapp_api.api.goldenapi.predicate" id="dapp_api.api.goldenapi.predicate"></a>

Get predicate given name

**Parameters:**

| Name   | Type  | Description      | Default    |
| ------ | ----- | ---------------- | ---------- |
| `name` | `str` | name of prediate | _required_ |

**Returns:**

| Type   | Description               |
| ------ | ------------------------- |
| `dict` | predicate object returned |

<details>

<summary>Source code in <code>dapp_api/api.py</code></summary>

```
def predicate(self, name: str, **kwargs) -> dict:
    """Get predicate given name

    Args:
        name (str): name of prediate

    Returns:
        dict: predicate object returned
    """
    params = locals()
    params.pop("kwargs")
    params.pop("self")
    params.update(kwargs)
    op = PredicateOperations.query.predicate
    variables = self.generate_variables(op, params)
    data = self.endpoint(op, variables)
    return data
```

</details>

#### `predicates(self)` [¶](broken-reference) <a href="#dapp_api.api.goldenapi.predicates" id="dapp_api.api.goldenapi.predicates"></a>

Get all predicates

**Returns:**

| Type   | Description                  |
| ------ | ---------------------------- |
| `dict` | List of available predicates |

<details>

<summary>Source code in <code>dapp_api/api.py</code></summary>

```
def predicates(self) -> dict:
    """ Get all predicates

    Returns:
        dict: List of available predicates
    """        
    query = """query MyQuery {
        predicates {
          edges {
            node {
              id
              name
              objectType
            }
          }
        }
    }"""
    variables = {}
    data = self.endpoint(query, variables)
    return data
```

</details>

#### `query(self, query='', headers={})` [¶](broken-reference) <a href="#dapp_api.api.goldenapi.query" id="dapp_api.api.goldenapi.query"></a>

Generic method to query graphql endpoint

<details>

<summary>Source code in <code>dapp_api/api.py</code></summary>

```
def query(self, query: str = "", headers: dict = {}):
    """ Generic method to query graphql endpoint
    """
    r = requests.post(self.url, json={"query": query}, headers=headers)
    print(r.status_code)
    if r.status_code==400:
        logger.warning(r)
    return r.json()
```

</details>

#### `set_jwt_token(self, jwt_token='')` [¶](broken-reference) <a href="#dapp_api.api.goldenapi.set_jwt_token" id="dapp_api.api.goldenapi.set_jwt_token"></a>

Set your token to start accessing the API with your wallet/role

**Parameters:**

| Name        | Type  | Description                | Default |
| ----------- | ----- | -------------------------- | ------- |
| `jwt_token` | `str` | JWT Token. Defaults to "". | `''`    |

<details>

<summary>Source code in <code>dapp_api/api.py</code></summary>

```
def set_jwt_token(self, jwt_token: str = "") -> None:
    """Set your token to start accessing the API with your wallet/role

    Args:
        jwt_token (str): JWT Token. Defaults to "".
    """
    self.jwt_token = jwt_token
    self.headers = {'Authorization': f'Bearer {jwt_token}'} if jwt_token else {}
    self.endpoint = HTTPEndpoint(self.url, self.headers)
```

</details>

#### `submit_date_triple(self, subject_entity_id, predicate_id, object_date)` [¶](broken-reference) <a href="#dapp_api.api.goldenapi.submit_date_triple" id="dapp_api.api.goldenapi.submit_date_triple"></a>

Submit date object triple

**Parameters:**

| Name                | Type   | Description           | Default    |
| ------------------- | ------ | --------------------- | ---------- |
| `subject_entity_id` | `str`  | UID of subject entity | _required_ |
| `predicate_id`      | `str`  | UID of predicate      | _required_ |
| `object_date`       | `dict` | _description_         | _required_ |

<details>

<summary>Source code in <code>dapp_api/api.py</code></summary>

```
def submit_date_triple(self, subject_entity_id: str, predicate_id: str, object_date: dict) -> None:
    """Submit date object triple

    Args:
        subject_entity_id (str): UID of subject entity
        predicate_id (str): UID of predicate
        object_date (dict): _description_
    """
    pass
```

</details>

#### `submit_entity_triple(self, subject_entity_id, predicate_id, object_entity_id, citation_url)` [¶](broken-reference) <a href="#dapp_api.api.goldenapi.submit_entity_triple" id="dapp_api.api.goldenapi.submit_entity_triple"></a>

Submit entity object triple

**Parameters:**

| Name                | Type  | Description           | Default    |
| ------------------- | ----- | --------------------- | ---------- |
| `subject_entity_id` | `str` | UID of subject entity | _required_ |
| `predicate_id`      | `str` | UID of predicate      | _required_ |
| `object_entity_id`  | `str` |                       | _required_ |

Example

<details>

<summary>Source code in <code>dapp_api/api.py</code></summary>

```
    def submit_entity_triple(self, subject_entity_id: str, predicate_id: str, object_entity_id: str, citation_url: str) -> None:
        """Submit entity object triple

        Args:
            subject_entity_id (str): UID of subject entity
            predicate_id (str): UID of predicate
            object_entity_id (str): 

        Example
        """
        query = f"""mutation AddTripleToEntityById {{
		  createTriple(
		    input: {{
		      triple: {{
		        subjectId: "{subject_entity_id}"
		        predicateId: "{predicate_id}"
		        objectEntityId: "{object_entity_id}"
		        citationUrl: "{citation_url}"
		      }}
		    }}
		  ) {{
		    triple {{
		      id
		    }}
		  }}
        }}"""
        r = requests.post(self.url, json={"query": query})
        print(r.status_code)
        print(r.text)
        return r.json()
```

</details>

#### `submit_text_triple(self, subject_entity_id, predicate_id, object_text)` [¶](broken-reference) <a href="#dapp_api.api.goldenapi.submit_text_triple" id="dapp_api.api.goldenapi.submit_text_triple"></a>

Submit text object triple

**Parameters:**

| Name                | Type  | Description           | Default    |
| ------------------- | ----- | --------------------- | ---------- |
| `subject_entity_id` | `str` | UID of subject entity | _required_ |
| `predicate_id`      | `str` | UID of predicate      | _required_ |
| `object_text`       | `str` | _description_         | _required_ |

<details>

<summary>Source code in <code>dapp_api/api.py</code></summary>

```
def submit_text_triple(self, subject_entity_id: str, predicate_id: str, object_text: str) -> None:
    """Submit text object triple

    Args:
        subject_entity_id (str): UID of subject entity
        predicate_id (str): UID of predicate
        object_text (str): _description_
    """
    pass
```

</details>

#### `templates(self)` [¶](broken-reference) <a href="#dapp_api.api.goldenapi.templates" id="dapp_api.api.goldenapi.templates"></a>

Get all templates

**Returns:**

| Type   | Description                 |
| ------ | --------------------------- |
| `dict` | List of available templates |

<details>

<summary>Source code in <code>dapp_api/api.py</code></summary>

```
def templates(self) -> dict:
    """ Get all templates

    Returns:
        dict: List of available templates
    """
    query = """query MyQuery {
        templates {
          edges {
            node {
              id
              entityId 
              entity {
                name
                description
              }
            }
          }
        }
    }"""
    variables = {}
    data = self.endpoint(query, variables)
    return data
```

</details>

#### `to_camel_case(snake_str)` `staticmethod` [¶](broken-reference) <a href="#dapp_api.api.goldenapi.to_camel_case" id="dapp_api.api.goldenapi.to_camel_case"></a>

Convert snake str to camel case since all params should eventually adhere to graphql camel case

**Parameters:**

| Name        | Type  | Description  | Default    |
| ----------- | ----- | ------------ | ---------- |
| `snake_str` | `str` | snake string | _required_ |

**Returns:**

| Type  | Description      |
| ----- | ---------------- |
| `str` | lower camel case |

<details>

<summary>Source code in <code>dapp_api/api.py</code></summary>

```
@staticmethod
def to_camel_case(snake_str: str) -> str:
    """Convert snake str to camel case since all params
    should eventually adhere to graphql camel case

    Args:
        snake_str (str): snake string

    Returns:
        str: lower camel case
    """
    components = snake_str.split('_')
    return components[0] + ''.join(x.title() for x in components[1:])
```

</details>

#### `unvalidated_triple(self)` [¶](broken-reference) <a href="#dapp_api.api.goldenapi.unvalidated_triple" id="dapp_api.api.goldenapi.unvalidated_triple"></a>

Get all templates

**Returns:**

| Type   | Description                 |
| ------ | --------------------------- |
| `dict` | List of available templates |

<details>

<summary>Source code in <code>dapp_api/api.py</code></summary>

```
def unvalidated_triple(self) -> dict:
    """ Get all templates

    Returns:
        dict: List of available templates
    """
    query = """query MyQuery {
        unvalidatedTriple {
          ... on Statement {
            id
            citationUrl
            dateCreated
            objectEntityId
            subjectId
            userId
            predicateId
          }
        }
    }"""
    variables = {}
    data = self.endpoint(query, variables)
    return data
```

</details>
