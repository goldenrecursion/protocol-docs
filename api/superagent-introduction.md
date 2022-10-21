---
description: Learn how to use tools to quickly add and validate data to the Golden protocol
---

# Intro to API Submission and Verification

### Getting started: Typical programmatic tasks

To get started you should first decide on what task you are best suited to accomplish. Currently, we have entity creation, triple submission, and triple validation tasks that can be performed through the protocol API or python SDK. To get an idea of the steps involved in these processes a high-level outline is included below.

We encourage all users who are interested in being superagents to join the [#developers channel in our Discord](https://discord.gg/H7pmZYTjkX).

### Entity Creation

To create a new entity using the protocol API your application will need to:

1. Search to ensure this new entity does not already exist.
2. Select the entity type for the new entity.
3. Submit the minimum disambiguation triples required at the time of creation.

You can read more in-depth at:

{% content-ref url="../godel-python-sdk/create-entities.md" %}
[create-entities.md](../godel-python-sdk/create-entities.md)
{% endcontent-ref %}

### Triple Submission

To submit a triple and citation using the protocol API your application will need to:

1.  Search to ensure this new triple does not already exist.

    a. Find the ID for the predicate type that you will be updating\
    b. Find the ID for the entity you will be updating\
    c. Search for that predicate ID on the specific entity ID
2. Use the predicate ID from step 1 to create a new submission. Be sure to include a citation for the new information added.

You can read more in-depth at:

{% content-ref url="../godel-python-sdk/create-triples.md" %}
[create-triples.md](../godel-python-sdk/create-triples.md)
{% endcontent-ref %}

### Validation

To perform a validation vote using the protocol API your application will need to:

1. Get the next unvalidated triple from your validation queue.
2. Inspect the entity, predicate, and object
3.  Use some form of logic system to determine if the listed triple is correct.

    a. This logic will be of your making. You may use whatever resources are available to you, for instance NLP models.\
    b. If you are unable to determine the triple's validity you may skip that validation.
4. Create a vote that either accepts or rejects the validation.

You can read more in-depth at:

{% content-ref url="../godel-python-sdk/validation.md" %}
[validation.md](../godel-python-sdk/validation.md)
{% endcontent-ref %}
