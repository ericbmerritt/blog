---
layout: post
title: Team Development with Git
---

{{ page.title }}
================

The process of collaboration and development on a project is
important. The means by which you get that code into a deliverable
state matters and matters a lot. While method of delivery and what
constitutes a deliverable state changes from company to company, team
to team and even project to project, there is always a point at which
you want 'make something available'.

The Model
---------

1. There is a single repository that serves as the Canonical
   Repository (the main store of code that will be delivered).
2. Each developer has a clone of this repository that they use for
   development on their local workstations. If they are not on the
   same network then they may have a remote clone that serves as a
   means of collaboration with their peers (ie GitHub).
3. When code is complete in the Developer's workstation the Developer
   announces the fact.
4. Another team member pulls the code, reviews, compiles and tests it.
5. If any stage of the review fails, then the review pushes back to
   the originating developer.
6. If it passes it is signed off and pushed to the target branch of
   the Canonical Repository.

#### The Canonical Repository

The Canonical Repository is the repository we deliver code from, it
serves as the central reference point, the tip of development for the
project. No one pushes Work In Progress (WIP) code to this repository
and no single person owns it. It exists to hold the history of the
product that is currently in production or that will be in production
and it is the point that Developers rebase their work in while
developing their code.

There is a bit of a mind hack going on here that I encourage you keep
intact. That is the 'sacredness' of canonical. It should never become
something that a developer pushes too as part of his daily
workflow. It should always carry the sense that things that go into it
are important. In my experience this vastly reduces the screw-ups,
bugs, bad pushes etc that can be so painful to a team. It also helps
encourage both the developer and the code reviewer to take their job
seriously without forcing a lot of process onto them. That lack of
strict painful process is what makes this approach so powerful.

#### The Developers Repositories

Developers work in clones of the Canonical Repository. They create
branches, work on experimental changes, code to meet the requirements
of the system and collaborate with each other through these clones. If
they are all on the same network then they setup the
[Git Server](http://tumblr.intranation.com/post/766290565/how-set-up-your-own-private-git-server-linux)
on their development boxes and push and pull clones directly to and
from their team mates. If they are in different parts of the world or
simply on different networks they may have to use some intermediary
like Github to share their code.  In that case, they replicate to
their github clone and collaboration goes on through github. Of the
two approaches I prefer addressing my peers peers repositories
directly. It removes an unnecessary step from the process and makes it
that much easier and less error prone.

There is a second big mind hack going on here. That is the fact that
nothing in the developers repo matters at all until the developer says
it matters. We want the developer to be productive, we want him to use
the tools that work for him and the process that is most comfortable
for him use to produce code. It doesn't matter if the rest of the team
uses emacs and he uses vim, it doesn't matter if he wants to use OSX
and the rest of the team uses Centos. What matters is the code he
produces meets the standards the team has set, through previous
reviews, automated test suites and the like.

We want him to explore freely. We want him to be the most productive
that he can be using the tools that he is comfortable with. We don't
want him to worry about what impact that exploration will have or if
someone is going to be looking over his shoulder trying to validate
the quality of code that literally wont matter until and if it makes
it into canonical.

How Code Gets Into Canonical
----------------------------

We don't impose tools, or process on developers in their own
repo. They can code using any process they would like, using any
editor, compiler, platform etc. It doesn't matter in the least as long
as the *output* meets the teams standards. Code that exists in the
developers repo is kind of like Potential Energy. Potential Energy is
impotent, has no interaction with the world around it until it becomes
Kinetic Energy. Once that Potential Energy becomes kinetic it has the
potential to change the world. Code in a developer's repository is
very similar. It has no impact on the world (ie the
project/team/organization) until the Developer(s) feel that it is
ready, until the developer explicitly converts it to Kinetic
Energy. Lets talk a bit about the process used to convert that
Potential Energy to Kinetic Energy.

There are always two parties to the process. The Developer or
Developers producing the code and the engineer that's going to
review/validate that code. Lets get started with the producer side.

#### The Developers Responsibility

The Developer puts the completed code onto a dedicated branch in his
repository and refactors it to meet the standards of the project. He
should ensure that the following invariants hold.

* The commits are small, self contained and well named. If you have
  not already take a quick look at my previous posting on
  [Git Commit Hygiene](http://blog.ericbmerritt.com/2011/09/21/commit-hygiene-and-git.html).
* The code follows the coding conventions of the project.
* That the code is good, in the eyes of the code reviewer. Functions
  are not too long, modules are focused, etc. Basically, that the code
  follows normal practice for producing good code.
* The code compiles on the target platform.
* All the tests in the system pass. (This should be a project
  invariant).

It could be that you have a convention that code ready for review goes
onto a branch specifically named, perhaps something like 'reviewable'
or 'rv'. It could also be that the Developer lets his team mates know
what branch the code is on when he makes the announcement. In either
case, once code is ready he makes an announcement letting the team
know that fact. Usually this is in the form of patches sent to a
mailing list, or a github pull request. The mechanism doesn't actually
matter so much as the fact that an intentional announcement with a
short description is made to the team working on the project.

Once the Developer announces it, a team mate should be selected to do
the review. There are a ton of ways to select a reviewer to handle
shepherding the code into canonical. In my experience the best way to
do it is just let the code reviewer self select. As long is its not
always the same person stepping up things should be fine. If you have
put together a good mature team this approach is, by far, the
best. Other ways to do it, is just to randomly assign a team mate, or
you could also have the Developer producing the code to pick the team
mate that is going to review. In the end how the Reviewer is selected
matters a lot less then the fact that one is selected.

The final responsibility of the Developer is to make sure make sure
the code gets reviewed. The longer code sits out without review the
more likely it is that bit rot will occur, that merging into the
Canonical branch will become painful etc. So the Developer needs to do
what is necessary to make the review happen in a reasonable time
frame.

#### The Reviewer Responsibility

The Reviewer's job is to review the change to validate that it meets
the project standard. This comprises a few things.

* Look at the change itself to make sure it does what its purported to
  do and meets the standard of the organization.
* Make sure that the Developer's changes rebase or merge cleanly with
  the Canonical branch that is being targeted.
* Make sure that the code compiles cleanly and all related tests pass.

If any of these steps fail the Reviewer pushes back on the original
Developer to make changes and finish the work. This may take several
iterations. *That is completely fine*. This is normal development, we
are engineers and we want to the code to be as well engineered and
maintainable as possible. The above steps are the *minimal* steps that
need to be done ensure this standard. Sub-par code should never make
it into Canonical simply because the developer is annoyed with the
process or the or timelines are tight. You invariably pay more in the
long run by giving in to these pressures then you gain in the short
run.

Once the change is reviewed and accepted the reviewer signs off on the
code. Yes, we expect the Reviewer to put his name on it and take
responsibility for the fact that that code is in Canonical. If the
code breaks the build it should be embarrassing to the Reviewer.

#### Once The Change is in Canonical

Once the code is in Canonical and out of the Developers repository,
history revision, commit amends and the like should all stop. At that
point your team has people depending on that code and changes to
history become painful. If there is a bug or a fix that needs to be
made it should go into a new commit and go through the process
previously outlined.

Wrapping Up
-----------

This process is actually very smooth, but it does assumes a few things:

* You have decided to adopt the process as a team.
* You have a reasonably competent engineers.
* You have at least some tests for your system (if you do not run,
  don't walk away from that system).
* You have some standards for the codebase that should be applied.

If these things are true then you are golden. It may take a little bit
of time to get a feel for the process and work around the little
hiccups that will inevitably occur. However, the process should be
flowing smoothly after a few weeks.

Things to Watch Out For
-----------------------

This is not intended to be a rigid process. The only place that real
process actually comes into it is in the transition from the
Developer's repository to the Canonical repository. This is true by
design. Its whole purpose is to encourage the wild and wooly exchange
and growth of ideas, creativity and productivity in the project
wherever that is possible while providing a bit of rigidity and
discipline only when it is required. Getting that balance right in
such a way that you get both the most creativity and productivity
possible and the most maintainable, well founded code while at the
same time keep the team happy is the goal. This is what this process
seeks to encourage.

One of the ways I have seen the process become founder and become
rigid is in respect to a team that does not know git well. This
process takes quite a bit of comfort with Git to work. Your developers
need to be comfortable with local and remote repositories, rebasing,
merging, pushing and pulling from peers, signing off on commits
etc. If you have new or incompetent developers you may be tempted to
wrap all of these steps in scripts that, apparently, take the need for
knowledge away from your developers. However, these scripts don't
actually remove the need for knowledge of git, all they really do is
delay that need slightly. What they actually do is encourage the team
to avoid learning git, while at the same time spending an inordinate
amount of time fixing problems when the scripts inevitably break. At
the same time they lock down and rigidify a process that should, by
its nature, not be rigid.

Go ahead, bite the bullet and give the team the time and resources
they need to learn git well. It will take a few weeks and you will
take a productivity hit. However, in the long run you will make that
back many times over.

The other big thing to watch out for is the urge to bypass the
process. Sometimes when you are in a tight situation it can be
tempting to want to push code directly to Canonical, bypassing the
review. For example, you might have a bug in production lets say your
system is down and its costing the company a million dollars a
minute. Pressure is high and you may have a huge urge to tell the devs
(or yourself if you are the dev) that the process would only slow you
down and the code needs to get out *right now*. This is almost
invariably a bad decision. This process should only take a few minutes
assuming the code is good. The likelihood that the code is bad in that
situation is high and its much cheaper timewise to catch those
problems in the review cycle then to deploy the code and realize in
production that you fixed one bug but introduced another.
