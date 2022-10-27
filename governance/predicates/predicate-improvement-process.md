# Predicate Improvement Process

<figure><img src="../../.gitbook/assets/image (1).png" alt=""><figcaption></figcaption></figure>

The process for adding, updating, or removing predicates on Golden has three key steps: submission of a proposal, discussion of the proposal, and voting on the proposal.

### Vision

Any user will be able to submit a Predicate Improvement Proposal (PIP). When each PIP is submitted it will be checked against the minimum requirements for a predicate proposal, and if so will be confirmed as valid and given an ID for use in discussion and voting. The community will be able to discuss the PIP on the Golden Forum, and then vote after a period of time on the final acceptance.

### Long-term vision vs immediate process

In the long-term, both the submission & confirmation and the voting steps will be done with a comprehensive smart contract. Until that has been established, Golden will use alternate methods.

**Submission:** In the near-term, all submissions should be posted in the Predicate category in the Golden forum. The submission should include the data required from the [Predicate Improvement Process - requirements document](https://www.notion.so/Predicate-Improvement-Process-requirements-c2927fdce8ec40219473ef74237845e6). In the long-term, users will be able to submit PIPs directly to a smart contract, which will validate the input and confirm submission with a PIP ID to be used in the Discussion and Vote processes.

**Discussion:** All discussions on Predicate Improvement Proposals will take place in the Golden forum. These discussions should stay focused on holistic consideration for including a predicate in the Golden Knowledge Graph data schema. Once a proposal has reached the voting stage, the forum discussion will be closed and implementation questions should be asked on Discord.

**Vote:** In the near-term, voting will be conducted on [Snapshot](https://snapshot.org), a voting tool built on IPFS. In the long-term voting will be done with smart contracts and weighted by the number of Golden tokens held by voters. Changes to our voting infrastructure and process will be announced on Discord and the Golden forum.

## Requirements

### Required in all submissions

* Field Name
* Field type:
  * “entity”
  * “text”
  * “integer”
  * “date”
  * “url”
* Number of values accepted:
  * “single”
  * “multiple”
* Tooltip description:
  * short description of the predicate (300 characters or less)
* Full description:
  * full description of the predicate (expected to be between a couple sentences or a couple paragraphs, depending on complexity)
* Examples of proper use:
  * text explaining acceptable usage of the predicate
* Examples of incorrect use:
  * text explaining incorrect usage of the predicate

### Where values must be in specified list

If the predicate will only allow values from a specified list of allowed values (enumerated list, or enum), the following fields must be specified.

* Enum = “true”
* Values:
  * A list of allowed values. This will either be a list of text values or Golden entity IDs.

### URLs in specified formats

If the predicate aims to capture data on urls in specified formats, it is best to capture this using the “text” field type and the following fields.

* Format as URL = “true”
* URL format: “\<url string>”
  * example: “[https://open.spotify.com/artist/{:id}”](https://open.spotify.com/artist/%7B:id%7D%E2%80%9D) to generate the url for an artist on Spotify given the artist ID.

## Discussion topics

There are a number of considerations that may or may not be relevant to each Predicate Improvement Proposal. This section aims to share a number of questions that should be considered in the discussion process for each PIP.

### Questions to address

* What is the estimated cardinality of this predicate?
  1. If multiple values are allowed, how many different values can this predicate take on? (approximation)
  2. How many different entities will this predicate apply to? (approximation)
* What is the estimated frequency of new values or changes for each entity?
* What (if any) other schemas does this predicate map to? ([Schema.org](http://schema.org), Wikidata, etc.)
* Are there any PII (Personally Identifiable Information) concerns with this field?
* Are there any concerns about the ability for an average user to verify data in this field?
* What is the likelihood that data in this field will become stale? How long will the data in this field be relevant?
