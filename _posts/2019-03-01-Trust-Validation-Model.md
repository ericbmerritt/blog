---
bg: 'movement.jpg'
layout: post
title: Trust Validation Model
tags: [authority,trust]
comments: true
---

Whether or not we trust a source of information depends to a large
extent on the source of that information. If the source of information
is trusted, then we trust if it isn't then we don't. In other words,
trust is a personal thing based on who the consumer of data have
decided are their ultimate sources of trust. Trust then flows out from
those sources through a graph of connections to the media item
currently being consumed.

Unfortunately, understanding the source of information in the current
digital landscape is challenging. Below we define a model for defining
and understanding the flow of trust across a graph of cited media
references. The eventual goal would be to implement this on top of a
block chain solution like [Ethereum](www.ethereum.org)


Definitions
=========

Objects
-------

Articles

:   A work produced by an Author

Roles
-----

Consumer

:   An individual or entity that consumes an article

Author

:   An individual or entity that authors an article

Relationships
-------------

Authorship

:   The relationship between an Author and the Article authored

Observation

:   The relationship between two Consumers for trust delegation

Criticism

:   A negative relationship between two Articles

Citation

:   A positive relationship between two Articles

Consumption

:   The relationship between a Consumer and an Article

Introduction
============

We define information in the system by a set of relationships between
Articles, Authors, and Consumers. Those relationships are established
for by citations and criticisms between articles, consumption of
Articles by Consumers and Observation relationships among Consumers.
Trust is given by a Consumer to another individual or an Author through
the Consumption of Articles, those Authorship relationships between
Articles and Authors and the Citation and Criticism relationships
between Articles.

Trust Example
=============

![Trust Graph](/assets/images/trust-model.png)

The Trust Graph provides a visual representation of a group
of historical authors connected via their works. A consumer of articles
has shown over time that he has a high level of trust for Dante
Alighieri, that indicates that he will also have some trust level with
Ernest Hemingway and to a lesser extent Leo Tolstoy. The chain of
relationships that define this trust model is as follows:

1.  Dante Alighieri authored 'The Divine Comedy'.

2.  The Divine Comedy cites 'The Sun Also Rises' authored by Ernest
    Hemingway

3.  'The Sun Also Rises' cites 'War and Peace' authored by Leo Tolstoy

For every step away from the trusted author the level of trust for the
connected author is reduced. Leo Tolstoy will not be as trusted as
Ernest Hemingway and Ernest Hemingway will not be as trusted as Dante
Alighieri.

Distrust Example
================

Distrust or lack of trust works the same way as Trust work but in a
negative way. A consumer of articles that has a high trust level for
Virginia Woolf will have a low trust level with Edgar Allen Poe. The
chain of relationships that define distrust are as follows:

1.  Virginia Woolf authored 'Mrs. Dalloway'

2.  Mrs. Dalloway criticises 'Angel of the Odd' authored by Edgar Allan
    Poe

Delegated Trust
===============

Trust can also derive from another consumer of articles. Referring to
Figure 1, Will Huntington is Consumer. He has an Observer relationship
with Fox News. In this model, Will Huntington inherits the trust and
distrust relationships that Fox News has created. In Figure 1, Will
Huntington has a Trust Relationship with Edgar Allen Poe, through his
observer relationship with Fox News. To follow another relationship
chain:

1.  Will Huntington observes Fox News

2.  Fox News is a Consumer of 'The Divine Comedy' authored by Dante
    Alighieri

3.  'The Divine Comedy' cites 'The Balloon Hoax' authored by Edgar Allan
    Poe

Further Work
============

In the examples above we have provided simplistic relationship chains.
In a functioning ecosystem of articles citations and criticisms, trust
and distrust will be much harder to validate. An algorithm must be
derived to define the trust/distrust relationship for each article.

Further work must also be done to leverage Ethereum token model so that
citations and criticisms have some cost associated. Otherwise, Authors
in the system have some ability to manipulate the trust relationships of
their consumers.

There might be an opportunity to include global factors into the trust
metric as well. If not directly as part of the trust metric for a
particular author, then as information for the consumer on how his trust
metric for that author looks compared to the trust metrics for that
author for some subset of the population. This may allow the system to
get around some of the problems of consumer who only consume information
that agrees with their world view.
