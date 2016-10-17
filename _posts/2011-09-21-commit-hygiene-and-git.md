---
bg: 'apollo.png'
layout: post
title: Commit Hygiene and Git
tags: [philosophy,git]
---

We have a very strong standard for organization when it comes to
commits in git. In general, commits should contain change focused only
on a single thing. That change should be well described in the commit
message in both the short commit description and the long. When you
look at the output of 'git log' the commits displayed there should
present a linear history of change that resulted in the current point
of development. At the root of things, the commit history is a
consumable part of your commit output.

A good commit should have all of the following features.

- It's focused on a single point of change
- The first line of the commit message accurately summarizes the change
- The long description goes into the detail as to why the change was
  made and further detail on what changes were made. If the first
  line is sufficient then this line is optional

Why is it Important?
--------------------

It takes time and effort to clean up your patches and get them into a
publishable, well-factored state. That is the effort that many people
don't want to expend. If that's the case why do it?

### Do it so code reviews are easier

A single patch focused on a single unit of change is much easier to
review and discuss than a single change spread over some
patches or a single patch that contains a large number of unrelated
changes.

Knowing why a request was created (a ticket, a story, a customer
request) and being able to tie that to a commit that solves the
problem goes a long way towards making your patch comprehensible to
the reviewer.

Having commit focused on a single change helps the reviewer decide if
the change represented by the commit is a reasonable approach for
solving that problem.

### Do it so that the change going on in the system is obvious

The other reason to factor your changes before they make it into
production is simple; consumability. We want to make sure that a
person looking at the history of change in a repository has a good
idea of the rate of change and the reason for change.

This will help in maintaining our code base. It will help us
understand why a particular change occurred and what the steps leading
up to that change will be. Also, the discipline of patch hygiene will
help you focus on the change that needs to occur for to solve just
problem at hand. Saving a lot of wasted time and effort on things that
don't matter.

### Do it so git revert and git bisect will work correctly

`git bisect` and `git revert` are very helpful in finding problems and
fixing those problems while retaining and forward moving consumable
history in git. Using these tools, we will be able to test each
patch on its own, and easily identify all the
patches related to the problem, then revert those patches.

What is One 'Unit' of Change?
---------------------------

What exactly is one 'unit' of change? Like is so often the case, the
answer is 'it depends'. The rule of thumb is that if it's directly
related to the problem (story, task, etc.) you are actively working on
and it can be tied directly to what you are holding in your head, then
its probably a 'Unit' of change.

However, if you see a bug or refactoring opportunity while doing other
things, that's not part of that change and should be a separate
commit. If you have two things in your head and are working on
them at the same time because they are somehow related than those also
are not part of the same change.


When to Practice Commit Hygiene
-------------------------------

There is one right answer to when to practice commit hygiene. That is
always 'before the code is published'. During development commit in a
way that makes sense to you, but before you submit a code review use
git's interactive rebase functionality to clean up the commits and get
them ready for consumption.


How to Practice Commit Hygiene
------------------------------

The key is the
[Interactive Rebase](http://book.git-scm.com/4_interactive_rebasing.html)
functionality in git. This is the tool you will use to clean up and
manipulate your git history. Following are some links that might help you.

1. http://codeutopia.net/blog/2009/12/10/git-interactive-rebase-tips/
2. http://help.github.com/rebase/
3. http://gitready.com/advanced/2009/02/10/squashing-commits-with-rebase.html
4. http://blogs.gnome.org/alexl/2009/10/12/the-gospel-of-git-interactive-rebase/

Conclusion
----------

Git is more a developer's tool more than anything else. To get the
most out of it, we need to follow a few simple guidelines and leverage
some of it's more advanced features.