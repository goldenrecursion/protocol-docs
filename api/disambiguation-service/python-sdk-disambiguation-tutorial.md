---
description: Learn to use the disambiguation service through the Golden python SDK.
---

# Python SDK Disambiguation Tutorial

## Overview of`disambiguate_triples()` tutorial <a href="#godel-disambiguate_triples-api-tutorial" id="godel-disambiguate_triples-api-tutorial"></a>

This is a short guide to triple disambiguation on the Golden protocol. Disambiguation is a core step in the data submission process, as any triple needs to point to an existing entity reference in the protocol. Good disambiguation prevents the creation of entity and triple duplicates.

In this tutorial, we will cover an example of how we can go from a raw dataset to a list of disambiguated triples ready for submission to the protocol.

```python
# Necessary imports for the tutorial
from csv import DictReader
import io
from typing import List, Dict

from godel import GoldenAPI
from godel.models import DisambiguationTripleDict
```

For the sake of this example, let's say we have obtained this small dataset about tech companies. We have our data in CSV format, where each row corresponds to a different company and each column to a different triple.

First, let's take a look at the data, and parse it into python objects.

```python
sample_dataset = \
"""name,num_employees,website,founder_name,ceo_name,ceo_twitter_url
Twitter,3898,https://twitter.com,Jack Dorsey,Elon Musk,https://twitter.com/elonmusk
Dropbox,2667,http://www.dropbox.com/,Drew Houston,Drew Houston,https://twitter.com/drewhouston
Meta,70000,http://meta.com/,Mark Zuckerberg,Mark Zuckerberg,https://twitter.com/finkd
Apple,154000,http://apple.com/,Sateve Jobs,Tim Cook,https://twitter.com/tim_cook
"""
print(sample_dataset)
```

```python
name,num_employees,website,founder_name,ceo_name,ceo_twitter_url
Twitter,3898,https://twitter.com,Jack Dorsey,Elon Musk,https://twitter.com/elonmusk
Dropbox,2667,http://www.dropbox.com/,Drew Houston,Drew Houston,https://twitter.com/drewhouston
Meta,70000,http://meta.com/,Mark Zuckerberg,Mark Zuckerberg,https://twitter.com/finkd
Apple,154000,http://apple.com/,Sateve Jobs,Tim Cook,https://twitter.com/tim_cook
```



```python
reader = DictReader(io.StringIO(sample_dataset), dialect='unix')
parsed_data = []
for row in reader:
    parsed_data.append(row)
parsed_data
```

```python
[{'name': 'Twitter',
  'num_employees': '3898',
  'website': 'https://twitter.com',
  'founder_name': 'Jack Dorsey',pythpyth
  'ceo_name': 'Elon Musk',
  'ceo_twitter_url': 'https://twitter.com/elonmusk'},
 {'name': 'Dropbox',
  'num_employees': '2667',
  'website': 'http://www.dropbox.com/',
  'founder_name': 'Drew Houston',
  'ceo_name': 'Drew Houston',
  'ceo_twitter_url': 'https://twitter.com/drewhouston'},
 {'name': 'Meta',
  'num_employees': '70000',
  'website': 'http://meta.com/',
  'founder_name': 'Mark Zuckerberg',
  'ceo_name': 'Mark Zuckerberg',
  'ceo_twitter_url': 'https://twitter.com/finkd'},
 {'name': 'Apple',
  'num_employees': '154000',
  'website': 'http://apple.com/',
  'founder_name': 'Sateve Jobs',
  'ceo_name': 'Tim Cook',
  'ceo_twitter_url': 'https://twitter.com/tim_cook'}]
```

Now that we've loaded this data, we need to transform it into a set of predicates that exist on the [protocol's schema](https://dapp.golden.xyz/schema). We can get a full list of available predicates on [https://dapp.golden.xyz/schema](https://notebooks.githubusercontent.com/view/ipynb?browser=chrome\&color\_mode=dark\&commit=154ee51bc77e23e5318a9ce45d750037d2666a38\&device=unknown\_device\&enc\_url=68747470733a2f2f7261772e67697468756275736572636f6e74656e742e636f6d2f676f6c64656e726563757273696f6e2f676f64656c2f313534656535316263373765323365353331386139636534356437353030333764323636366133382f7475746f7269616c732f446973616d62696775617465253230547269706c65732e6970796e62\&logged\_in=true\&nwo=goldenrecursion%2Fgodel\&path=tutorials%2FDisambiguate+Triples.ipynb\&platform=mac\&repository\_id=508994387\&repository\_type=Repository\&version=107) or use the API itself to browse through the full list of implemented predicates:

```python
goldapi = GoldenAPI()
predicates = goldapi.predicates()
predicates["data"]["predicates"]["edges"][:4]
```

```python
[{'node': {'id': '03744719-b7f6-4316-b105-93a94f3b53cc',
   'name': 'SIC Code',
   'objectType': 'INTEGER'}},
 {'node': {'id': '047cb849-0d0e-4c24-a4a4-51e53336b1ea',
   'name': 'Duplicate of',
   'objectType': 'ENTITY'}},
 {'node': {'id': '08219f1e-6c57-4fad-99a3-a998038e663d',
   'name': 'NAICS Code',
   'objectType': 'INTEGER'}},
 {'node': {'id': '09dec055-52d8-42e6-9fc0-56ecac599a8b',
   'name': 'Spotify Artist ID',
   'objectType': 'STRING'}}]
```

For this particular dataset, the mapping between the CSV columns and the Protocol's predicates would be as follows:

* **name**: Name
* **num\_employees**: Number of Employees
* **website**: Website
* **founder\_name**: Founder -> Name
* **ceo\_name**: CEO -> Name

Note that some predicates, such as _Founder_ and _CEO_ do not take a value, but a reference to another entity of the protocol. Since our dataset does not have such references, but triples related to those entities, we will need to disambiguate those first, and once we have a valid id reference, disambiguate again. The API itself does have support for disambiguating recursively, so you'll be covered as long as the data is formatted correctly.

With the code below, we define the mapping between the dataset column names and the protocol's predicates. Also, we define a function that will create nested triple structures, for when we need to disambiguate recursively.

```python
column_predicate_mapping = {
    "name": ["Name"],
    "num_employees": ["Number of Employees"],
    "website": ["Website"],
    "founder_name": ["Founder", "Name"],
    "ceo_name": ["CEO", "Name"],
    "ceo_twitter_url": ["CEO", "Twitter URL"],
}

def protocol_predicate_mapper(
    input_data: List[Dict[str, str]],
    column_mappings: Dict[str, List[str]]
) -> List[DisambiguationTripleDict]:
    # hoa
    transformed_data = []
    for entity in input_data:
        protocol_triples = {}
        for k, v in entity.items():
            triples = protocol_triples
            sub_predicates = column_mappings[k]
            while sub_predicates:
                predicate, *sub_predicates = sub_predicates
                if sub_predicates:
                    triples[predicate] = triples.get(predicate,{})
                    triples = triples[predicate]
                else:
                    triples[predicate] = v
        transformed_data.append(protocol_triples)
    return transformed_data
```

And now, let's test out the automated mapping onto our dataset:

```python
transformed_data = protocol_predicate_mapper(parsed_data, column_predicate_mapping)
transformed_data
```

```python
[{'Name': 'Twitter',
  'Number of Employees': '3898',
  'Website': 'https://twitter.com',
  'Founder': {'Name': 'Jack Dorsey'},
  'CEO': {'Name': 'Elon Musk', 'Twitter URL': 'https://twitter.com/elonmusk'}},
 {'Name': 'Dropbox',
  'Number of Employees': '2667',
  'Website': 'http://www.dropbox.com/',
  'Founder': {'Name': 'Drew Houston'},
  'CEO': {'Name': 'Drew Houston',
   'Twitter URL': 'https://twitter.com/drewhouston'}},
 {'Name': 'Meta',
  'Number of Employees': '70000',
  'Website': 'http://meta.com/',
  'Founder': {'Name': 'Mark Zuckerberg'},
  'CEO': {'Name': 'Mark Zuckerberg',
   'Twitter URL': 'https://twitter.com/finkd'}},
 {'Name': 'Apple',
  'Number of Employees': '154000',
  'Website': 'http://apple.com/',
  'Founder': {'Name': 'Sateve Jobs'},
  'CEO': {'Name': 'Tim Cook', 'Twitter URL': 'https://twitter.com/tim_cook'}}]
```

As you can see, for most of the columns, we've just had to rename them, so their name would match an existing predicate. For predicates that referenced other entities, such as CEO, since we didn't have the graph reference, but did have triples on the underlying object which we could use for disambiguation, we nest a dictionary inside, which will be disambiguated automatically when calling the API.

For example, let's disambiguate the triples on Twitter:

```python
twitter_triples = transformed_data[0]
response = goldapi.disambiguate_triples(twitter_triples, with_diff=False)
response["data"]["disambiguateTriples"]["entities"]
```

```python
[{'name': 'Twitter',
  'distance': 0,
  'id': 'e2c2c291-164a-443b-a034-ff6fc29003fe',
  'reputation': 0.5186093326169795}]
```

In this particular case, the `disambiguate_triple()` API call has found a perfect match (`distance=0`) on the entity with [id=e2c2c291-164a-443b-a034-ff6fc29003fe](https://dapp.golden.xyz/entity/e2c2c291-164a-443b-a034-ff6fc29003fe), so it would be pretty safe to assume that the triples would need to be submitted against that entity id.

If we want to further inquire about which triples, for the disambiguated entity, have already been submitted, and which ones haven't, we can turn on the `with_diff=True`flag:

```python
response = goldapi.disambiguate_triples(twitter_triples, with_diff=True)
disambiguation_candidate = response["data"]["disambiguateTriples"]["entities"][0]
disambiguation_candidate
```

```python
{'name': 'Twitter',
 'distance': 0,
 'id': 'e2c2c291-164a-443b-a034-ff6fc29003fe',
 'reputation': 0.5186093326169795,
 'diff': {'matches': [{'id': 'f717813c-ba53-4a9f-bfcd-9ac757b1cef1',
    'validation_status': 'PENDING',
    'predicate': 'Name',
    'object': 'Twitter'},
   {'id': 'fde62f90-b0ad-47ae-93e1-7b667eea0006',
    'validation_status': 'PENDING',
    'predicate': 'Number of Employees',
    'object': '3898'},
   {'id': '719f92d0-d746-4c44-ae87-07efddb4c498',
    'validation_status': 'PENDING',
    'predicate': 'Website',
    'object': 'https://twitter.com'},
   {'id': '908cfa7c-7547-440d-81d0-24e417de5c3b',
    'validation_status': 'PENDING',
    'predicate': 'Founder',
    'object': 'ce74eba3-90cf-466f-9193-dea711000b3e'},
   {'id': 'a7a082b6-56ba-4eec-b4c2-0147dead7445',
    'validation_status': 'PENDING',
    'predicate': 'CEO',
    'object': 'a73fb5d5-baac-4d33-a979-4b3863ab651d'}],
  'inserts': [],
  'updates': []}}
```

In this case, all triples have already been submitted to the graph, albeit their status may not be `ACCCEPTED` yet, so there's nothing that would need to be submitted.

Another thing that I would like to put attention to is how the _Founder_ ([_id=ce74eba3-90cf-466f-9193-dea711000b3e_](https://dapp.golden.xyz/entity/ce74eba3-90cf-466f-9193-dea711000b3e)) triple and the _CEO_ ([_id=a73fb5d5-baac-4d33-a979-4b3863ab651d_](https://dapp.golden.xyz/entity/a73fb5d5-baac-4d33-a979-4b3863ab651d)) triple of the response have been disambiguated to actual Protocol entities. The `disambiguate_triples()` API call recursively calls itself and picks up the best candidate within the distance threshold specified in the call (.3) by default.

It is possible to filter the triple search space by setting the `validation_status` flag. For example, `validation_status=ACCEPTED` will only search within _ACCEPTED_ candidate triples. By default, we search all triples, unless _INVALID_ or _REJECTED_.

Another concept of the `disambiguation_triple()` API is the entity _reputation_ metric. This is a number between 0 and 1, relative to other reputations in the same response, being 1 preferable to 0 (unlike distance, which the lower the number, the better match). Entity reputation is a rough approximation of how much effort people have put into a given entity. The higher the reputation, the more contributions that entity has received, and the higher value they are perceived to be. Within 2 entities similarly distanced, you would usually favor that with the higher reputation.

To end the tutorial, let's write a full loop on the dataset, where for each row we pick the highest reputation entity, along with the list of triples that are still missing from the system:

```python
disambiguated_entities = []
for triples in transformed_data:
    response = goldapi.disambiguate_triples(triples, distance_threshold=.3, with_diff=True)
    disambiguation_candidate = sorted(response["data"]["disambiguateTriples"]["entities"], key=lambda e: -e["reputation"])[0]
    disambiguated_entities.append([
        disambiguation_candidate["id"],
        disambiguation_candidate["name"],
        disambiguation_candidate["diff"]["inserts"],
    ])
disambiguated_entities
```

```python
[['e2c2c291-164a-443b-a034-ff6fc29003fe', 'Twitter', []],
 ['9cfc518f-83d6-447c-bc8e-aeb381b18056',
  'Dropbox',
  [{'object': 'http://www.dropbox.com/', 'predicate': 'Website'}]],
 ['6f2df1d3-3d38-485c-9236-db4d9312d6a9',
  'Meta',
  [{'object': '70000', 'predicate': 'Number of Employees'},
   {'object': 'http://meta.com/', 'predicate': 'Website'}]],
 ['debcb513-b842-4645-9856-2f4ea975002b',
  'Apple (company)',
  [{'object': 'Apple', 'predicate': 'Name'},
   {'object': 'http://apple.com/', 'predicate': 'Website'}]]]
```

As we can see, we've been able to disambiguate every entity, along with a subset of triples that are still missing from the graph, which we could then proceed to submit, as shown in the Create Triples tutorial in this same folder.
