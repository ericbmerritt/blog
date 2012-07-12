Commit Hygiene and Git
======================

I have a very strong standard for commits when it comes to git. In
general, commits should contain one unit of change and one unit of
change only. When looking at a Git log you should see a very clear,
linear history of change. A history where each commit contains a
single change has a good short commit line explaining what the commit
contains and a complete commit detail containing a description of the
whys of the commit.

I get a fair amount of flack on this from many of my peers. They tend
to see Git as simple a place to store code or as an audit trail, like
most non-distributed version control system. This opinion just does
not serve in most cases. Commit hygiene is an import part of
development using Git. In the same way that readability and factoring
is important to code. It takes work to do it right and the benefits
may be intangible, but in the long run its well worth it.

Why is it Important?
--------------------

It takes time and effort to clean up your patches and get them into a
publishable, well factored state. Effort that many people don't want
to spend. If thats the case why do it?

### Do it so git revert and git bisect will work correctly

In the alternate case, where change is spread willy nilly accross
multiple commits git bisect will not work correctly. That is, you will
not be able to test each patch on its own, nor will you be able to
identify easily all the patches related to the problem. You will end
up digging through the commits in this specific deployment looking for
each thing that might have caused the problem and reverting that. Of
course, because change is spread willy nilly around you will end up
reverting things you do not wish to revert and that will cause other
problems. In reality what you will probably do, is spend some number
of hours trying to get a fix in place that will allow things to run,
push those fixes and hopefully come back at some point in the future
to revist your fixes.

Lets say you roll out a new set of patches one of those patches
contain an error. In the case of well defined well factored patches
where related change is part of the same patch it is fairly trivial to
run git bisect against your repo find the offending patch, then do git
revert on said patch and redeploy your system.

### Do it so code reviews are easier

A single patch focused on a single set of change is much easier to
review and comment on then a single change spread over a number of
patches or a single patch that contains a large number of unrelated
changes.

Knowing what reason generated a patch (a ticket, a story, a customer
request) and being able to tie the change that accomplishes that
reason to the reason itself goes a long way towards making your patch
comprehensible to the reviewer.

### Do it so that the change going on in the system is obvious

The other reason to factor your changes before they make it into
production is simple higene. That a person looking at change sets in
linear temporal order has a good idea of what is changing in each
patch. That is, that each patch presents a good self contained step in
the march of code over time.

This unfortunately, is much harder to justify then the first
point. Much like refactoring its one of those things that you can
point to and say "This is a Good Idea", while beeing unable to give
hard and fast reasons for that. There is no way to say that a clean
commit history is going to save you 20% of your time on maintenance,
or that its going to help you get to your next milestone 5% more
quickly.

In my opinion, it will help you in maintaining your code base, in
understanding why a particular change occured and what the steps
leading up to that change will be. Also the discipline of patch
higheene will help you focus on the change that needs to occur for a
particular. I consider all these useful results for very little cost.

What is One Unit of Change?
---------------------------

What exactly is one unit of change? The answer is of course, it
depends. The rule of thumb is that if its directly related to the
change you are actively working on, that is it can be tied directly to
what you are holding in your head, then its probably part of the same
change.

However, if you see a bug or refactoring opportunity while doing other
things, that's not part of the change. If you have two things in your
head and are kind of working on them at the same time because they are
somehow related in your mind those also are not part of the same
change.

Much like with refactoring you will, over time, develop a sense for
what a good patch should look like and what you should be publishing.

When to Practice Commit Hygiene
-------------------------------

There is one right answer to when to practice commit hygiene. That is
always 'before the code is published'. Its very painful for your
consumers if the git history changes after they have pulled from your
repo. To avoid this, you want to make sure that you don't refactor
after you have pushed to the canonical repo.

One thing to be aware of is that the word publish can have several
meanings. In the case where you have a canonical repo where people
expect to find the latest released code, in time you publish there it
locks you from further refactoring. But lets take a slightly more
nebulous case. Lets say that you and a peer are working together on a
piece of task generating commits for the purpose of sharing code. Many
of these commits are going to be temp commits, or commits that only
exist so that the in-process code can be shared. In this case, you
also don't want to refactor while you are in the process of
building. You have a consumer after all, your peer. However, once you
and your peer have your work done but before you publish your work to
your canonical repo or your peers it would be a very good idea to for
you and/or your peer to sit down and work on refactoring the commit
stack, practicing a bit of commit hygiene before you publish your
work. You should consider that the last step in your build process. As
a note, once this in done you should do any future work on top of that
newly refactored branch.

How to Practice Commit Hygiene
------------------------------

The key is the
[Interactive Rebase](http://book.git-scm.com/4_interactive_rebasing.html)
in git. This is the tool you will use to clean up and manipulate your
git history. I wont go into too much detail here, but I will refer you
to many other good discussions on the subject.

1. http://codeutopia.net/blog/2009/12/10/git-interactive-rebase-tips/
2. http://help.github.com/rebase/
3. http://gitready.com/advanced/2009/02/10/squashing-commits-with-rebase.html
4. http://blogs.gnome.org/alexl/2009/10/12/the-gospel-of-git-interactive-rebase/

Conclusion
----------

In the end the thing that must be kept in mind is that git is a
developer's tool more than anything else. Not an audit trail, not a
place to stick all the crap that we produce. It is a tool, but you
need to follow some guidelines for that tool to be as useful as it can
be.
