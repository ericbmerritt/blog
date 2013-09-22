---
layout: post
title: Distributed Bug Tracking - Again
date: 2007-06-25
comments: false
---

{{ page.title }}
================

This is a bit of a clarification and expansion on a
[previous topic](http://erlangish.blogspot.com/2007/05/distributed-bug-tracking.html)
of this blog. To recap what I was talking about was a distributed
issue tracking system making use of an underlying distributed version
control system for its versioning, but augmented by command line tools
that support necessary issue tracking features; like searching,
merging, etc. Distributed issue tracking is a very new thing and has a
few hurtles to overcome. I am going to talk about what these hurtles
are and offer some ideas on how to overcome them.

In the
[comments](https://www.blogger.com/comment.g?blogID=7012030999962875668&postID=5049029583926631782)
of the last blog post on this topic
[Alex](http://www.geekfire.com/~alex/blog/) and I had a reasonably
long conversation around merging. He ended up posting his thoughts
[here](http://www.geekfire.com/~alex/blog/entries/Ideas-for-a-distributed-bug-tracking-system/Ideas-for-a-distributed-bug-tracking-system.html). Alex
has some interesting and useful ideas, though we differ in some
specifics.

### Merging and Discovery

In the last article I used the term 'merging' in a ambiguous way. I
used it to refer to both merging two issues into one another and
finding the duplicate issues. For the rest of this article I am going
to refer to merging as merging two issues and discovery as finding
issue duplications. This should reduce the ambiguity a bit.

### Merging Multiple Changes To The Same Issue

Merging is actually a pretty strait forward concept. I think you can
treat issues the same way you treat a source file when a merge
conflict occurs. By automatically merging what you can and allowing
the user to resolve conflicts manually you get reasonable merge
behavior with a high probability of a correct result. There is some
overhead for the user but, as with source changes, it shouldn't be
onerous.

### Merging Two Issues Into A Single Issue

This problem is slightly more complex but its really just an extension
of the last topic we talked about. In this case we just apply that
merge algorithm to two disparate issues instead of two versions of a
single issue. There may be some ambiguity around which issue becomes
the canonical issue and how to merge history for these two files,
however, these issues are mostly solved in distributed version control
systems and those solutions would work just fine in this instance as
well.

### Discovery

Discovery is by far the most complex issue here and its a problem that
occurs in any issue tracking system. Unfortunately, in a distributed
issue tracking system the problem has the potential to be much much
worse then in an issue tracking system with a central repository. This
is due to the fact that each and every user has his own canonical
version of the issue repository. For example: User Y sees a bug in the
system and enters Issue X to describe it and User Z sees the exact
same bug at a similar time and enters Issue W to describe it. Because
User X and Z both have canonical versions of the issue repository and
they have yet to sync their repositories there is no way for either
user to detect that a issue has already been created for that bug. So
when they replicate suddenly there are multiple issues in both
repositories.

In more normal issue tracking systems this can be mitigated to some
extent by encouraging your users to search for existing bugs first and
having people familiar with the issue repository reviewing new issues
as they are entered. However, this approach wont work with a
distributed issue tracking system because each user has a private
canonical set until he syncs with some other user. I believe that this
problem will be one of the fundamental problems that will plague new
distributed issue tracking system for some time.

There are ways to mitigate this. There are very good document
similarity algorithms out there and applying them to this problem
wouldn't be too difficult. Unfortunately, the text associated with
issues tends to be very short and this doesn't give these similarity
algorithms much room to work. There are ways we can mitigate these
problems though.

First we can reduce the total document corpus by using attributes of
the issue to subset the issues for similarity searching. For example,
we might only search for similarities within issues that have a
specific component tag. Actually, generalizing this statement we can
just use emantic properties of the issue to subset the issues that we
need to process for the similarity search.

Second we need to give the user a fast and easy way to run through the
output of a similarity search and approve/disapprove the merge. This
should be something that allows the user to view the issues side by
side and hit a single button or key combination to approve or disprove
the merge, then the next set pops up. This cycle would allow the user
to quickly move through all possible matches. If this worked well we
may be able to loosen the similarity constraints a bit to allow for
more matches.

Hopefully, a combination of approaches will make the discovery issue
more tractable.

### Referencing

The third and last major problem (there are undoubtedly others that I
can't think of right now) is simple referencing. There needs to be a
way to reference an issue regardless of the repository its on or where
it was created. The easiest way to do that would be to make use of
simple [UUIDs](http://en.wikipedia.org/wiki/UUID) for issue
identifiers. They are a bit unwieldy but their inherent uniqueness
makes them usable for our purposes. We can reduce the level of pain in
using UUIDs manually by allowing the user to specify the unique part
of a UUID in the tools that support this distributed issue tracking
system. I think monotone and, maybe git, allow something similar for
change set identifiers.
