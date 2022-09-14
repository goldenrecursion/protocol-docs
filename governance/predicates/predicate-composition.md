---
description: Learn the specifics of how the Golden protocol defines predicates.
---

# Predicate Composition

Predicates on Golden are made of two parts: the predicate configuration and a predicate’s usage details. Together they make up the full predicate definition.

The predicate configuration is the on-chain representation of the predicate. It defines the data types, allowable values, and more. Once created, the predicate configuration should rarely (if ever) need editing.

The usage details for a predicate shows the appropriate and inappropriate usage of that predicate within the Golden Knowledge Graph. The usage details are stored as Markdown files in a GitHub repository. They are stored in this manner because they are expected to need editing over time, as the community encounters different use cases and differences in how a predicate can and should be applied.

<figure><img src="https://lh4.googleusercontent.com/ahkNSya8wM82VwzWRCJSPFp3pztVGo9qacLKeU1jd5rE6Km64lPs6tEng1cPIInURRv6JgwFx7rTQcI8S-J7WxBu6Xllhyfh1B-3l-tub1dQVk99wU5WJlMy3BNZd4h2VyTZ5aDyc6FZmBho4u8XnK0_cKZcvkemWG33YdqyjcvUop9Yp-_NxWXsbw" alt=""><figcaption></figcaption></figure>
