---
layout: post
title: Team Development with Github
---

{{ page.title }}
================

The process of collaboration and development on a project is
important. The means by which you get your code into a deliverable
state matters and matters a lot. While the method of delivery and what
constitutes a deliverable state changes from company to company, team
to team and even project to project but there is always a point at
which you want 'make something available' ie, publishing

How to Publish Code - The Model
-------------------------------

1. There is a single repository that serves as the Canonical
   Repository, the main store of code that will be delivered. This
   should always be the one under your Organization account.
2. Each Developer has a Fork of this repository that they use for
   development. This is where they push their work on a daily
   basis. It is also the point of collaboration with other Developers
   when working on a single user story.
3. When code is complete the Developer organizes his code into a
   presentable, consumable form and creates a
   [Pull Request](https://help.github.com/articles/using-pull-requests)
   against Canonical. If multiple Developers are involved in
   a single User Store one of the Developers pulls all the related
   code into a single branch and creates the pull request.
4. Another team member accepts the pull request, assigns it to
   himself, then reviews, compiles and tests the code related to that
   pull request.
5. If any stage of the review fails then the reviewer lets the
   original Developer know in the pull request comments and waits
   until the Developer address the changes.
6. When the reviewer and Developer have agreed that the code is
   acceptable. The reviewer merges the Pull Request with
   Canonical.

#### The Canonical Repository

The Canonical Repository is the repository we deliver code from, it
serves as the central reference point, the tip of development for the
project. No one pushes Work In Progress (WIP) code to this repository
and no single person owns it. It exists to hold the history of the
project that is that is currently under development and the point of
coordination for accepted code in the team.

There is a bit of a mind hack going on here that I encourage you keep
intact. That is the 'sacredness' of canonical. It should never become
something that a Developer pushes too as part of his daily
workflow. It should always carry the sense that things that go into it
are important. In my experience this vastly reduces the screw-ups,
bugs, bad pushes etc that can be so painful to a team. It also helps
encourage both the Developer and the code reviewer to take their job
seriously without forcing a lot of process onto them. That lack of
strict painful process is what makes this approach so powerful.

#### The Developer's Repositories

Developers work in Forks of the Canonical Repository. They create
branches, work on experimental changes, code to meet User Story
requirements of the system and collaborate with each other through
these Forks. They use these forks as the point of coordination with
other Developers that they are working on a User Story with. This type
of Code sharing (during the development process) should happen via
direct pulling from the forks of their peers. That is, Developers
wanting to use code from a peer should use `git fetch` or `git pull`
to gather the code they are interested in using from their peers
forks. They shouldn't use Pull Requests. Pull Requests are a 'heavy
weight' construct that should really only be used to get code into
canonical.

There is a second big mind hack going on here. That is the fact that
nothing in the Developer repo matters until the Developer says it
matters by creating a pull request. We want the Developer to be
productive, we want him to use the tools that work for him and the
process that is most comfortable for him. So we place no restrictions
on how he edits, manages and commits code in his own repository. The
only thing that matters is that the code submits for inclusion in
canonical via a Pull Request meets the standards the team has set,
through peer reviews, automated test suites and the like.

We want him to explore freely. We want him to be the most productive
that he can be using the tools that he is comfortable with. We don't
want him to worry about what impact that exploration will have on
peers or if someone is going to be looking over his shoulder trying to
validate the quality of code that may never actually get into
canonical.

How Code Gets Into Canonical
----------------------------

We don't impose tools, or process on Developers in their own
repo. They can code using any process they would like, using any
editor, compiler, platform etc. It doesn't matter in the least as long
as the *output* meets the team's standards. Code that exists in the
Developer's repo is simple potential it doesn't matter until it is
resolved by creating a Pull Request. It has no impact on the world (ie
the project/team/organization) until the Developer feels that it is
ready, until he explicitly converts it from Potential to Realized via
a Pull Request.

Realizing potential code. There are always two parties to the
process. The Developer or Developers producing the code and the
Developer that's going to review/validate that code. Lets get started
with the Developer's side.

#### The Developer's Responsibility

The Developer puts the completed code onto a dedicated branch in his
repository and refactors that to meet the standards of the project. He
should ensure that the following invariants hold.

* The commits are small, self contained and well named. If you have
  not already, take a quick look at my previous posting on
  [Git Commit Hygiene](http://blog.ericbmerritt.com/2011/09/21/commit-hygiene-and-git.html).
* The code follows the coding conventions of the project.
* That the code is good, in the eyes of the code reviewer. Functions
  are not too long, modules are focused, etc. Basically, that the code
  follows normal practice for producing good well engineered code.
* The code compiles on the target platform without warnings.
* All the tests in the system pass. (This should be a project
  invariant).

Once the Developer creates a Pull Request, a team mate is selected to
do the review. There are a ton of ways to select a reviewer to handle
shepherding the code into canonical. In my experience the best way to
do it is just let the code reviewer self select. As long is its not
always the same person stepping up things should be fine. If you have
put together a good mature team this approach is, by far, the best.You
could also just to randomly assign a team mate, have the Developer
producing the code to pick the team mate that is going to review. In
the end how the Reviewer is selected matters a lot less then the fact
that you have a reviewer.

The final responsibility of the Developer is to make sure make sure
the code gets reviewed. The longer code sits out without review the
more likely it is that bit rot will occur, that merging into the
Canonical branch will become painful etc. So the Developer needs to do
what is necessary to make sure that code does not sit out in a Pull
Request for more then a few days without any action. This may require
him to respond quickly to comments from his reviewer or it may require
him to go stand over his reviewers shoulder while the reviewer is
doing the Pull Request. It depends on where the problem lies.

#### The Reviewer Responsibility

The Reviewer's job is to review the Pull Request to validate that it meets
the project standard. This comprises a few things.

* Look at the code in the Pull Request to make sure it does what its
  purported to do and meets the standard of the organization.
* Make sure that the Developer's changes rebase or merge cleanly with
  the Canonical branch that is being targeted.
* Make sure that the code compiles cleanly without warnings and all
  related tests pass (this should actually be done by an automated
  tool).

If any of these steps fail the Reviewer should push back on the
original Developer to make fix the code. This may take several
iterations. *That is completely fine*. This is normal development, we
are Engineers and we want to create a well engineered, maintainable
product. The above steps are the *minimal* steps that need to be done
ensure this standard. Sub-par code should never make it into Canonical
simply because the Developer is annoyed with the process or the or
timelines are tight. You invariably pay more in the long run by giving
in to these pressures then you gain in the short run by letting bad
code pass.

When the change is reviewed and accepted the reviewer signs merges the
Pull Request.

#### Once The Change is in Canonical

Once the code is in Canonical and out of the Developer's repository,
history revision, commit amends and the like should all stop. At that
point your team depending on the code and the order of the code and
changes to history become painful. If there is a bug or a fix that
needs to be made it should go into a new commit and go through the
process previously outlined above.

Wrapping Up
-----------

This process is actually very smooth, but it does assumes a few things:

* You have decided to adopt the process as a team.
* You have some automated testing.
* You have some standards for the codebase that should be applied.

If these things are true then you are golden. It may take a little bit
of time to get a feel for the process and work around the little
hiccups that will inevitably occur. However, the process should be
flowing smoothly after a few weeks.

Things to Watch Out For
-----------------------

This is not intended to be a rigid process. The only place that real
process actually comes into play in the transition from the
Developer's repository to the Canonical repository. This is true by
design. Its purpose is to encourage the wild and wooly exchange and
growth of ideas, creativity and productivity in the project wherever
that is possible while still providing a enough rigidity and
discipline where it is required. Getting that balance right means that
you get the most creativity and productivity possible from your
Developers and the most maintainable, well engineered code possible
while at the same time keep the team happy. This is what this process
seeks to encourage.

The big thing to watch out for is the urge to bypass the
process. Sometimes when you are in a tight situation it can be
tempting to want to push code directly to Canonical, bypassing the
review. For example, you might have a bug in production, lets say your
system is down and its costing the company a million dollars a
minute. Pressure is high and you may have a huge urge to tell the devs
(or yourself if you are the dev) that the process would only slow you
down and the code needs to get out *right now*. This is almost
invariably a bad decision. This process should only take a few minutes
assuming the code is good. The likelihood that the code is bad in that
situation is high and its much cheaper timewise to catch those
problems in the review cycle then to deploy the code and realize in
production that you fixed one bug but introduced another.
