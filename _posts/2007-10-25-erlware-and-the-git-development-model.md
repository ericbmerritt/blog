---
layout: post
title: Erlware and the Git Development Model
date: 2007-10-25
comments: false
---

{{ page.title }}
================

### Introduction

Recently the core commiters for Erlware, mostly Martin and I, have
decided to migrate Erlware's source control system from the previous
Google hosted subversion repository to self-hosted
git. [Linus' Tech Talk on Git](http://www.youtube.com/watch?v=4XpnKHJAok8)
convinced us that this was a good idea. This isn't just a source
control change, but a methodology change. It changes the way code gets
from it's source (the Erlware community) into Erlware itself. The main
things we really want to get out of this migration is

1. Greater visibility into the code base for participants
2. Increased code quality
3. Faster, cleaner incremental development

It's yet to be seen whether we will achieve our third goal, but we
have already achieved the first two, which is encouraging.

We have had a few false starts. That is just to be expected. When we
first started, we took the easy default and just replaced the central
subversion repository with a git repository. We then treated that git
repository as if it were just another subversion repo. That makes for
a nasty commit history and it doesn't leverage git's new development
model very well at all. So I started searching for information about
how to do a large project in git in a truly distributed fashion. Let
me tell you, there just isn't that much information out there about
organising a project around git. Its just not there! Some people are
working on that, I hear. In any case, I finally pulled aside a friend
(hey [Scott](http://srparish.net/)!), who was familiar with git, and
asked them how this whole distributed development thing is supposed to
work. Much to my amazement he actually knew! He could even explain it
to a dim bulb like me!

What follows describes how we are implementing git in Erlware. It is
based on what Scott told me, lurking on the git mailing list and
widely varied sources out there on the interweb.

### The Development Model

The model we are using is actually dirt-simple. Each person has one
or more personal git repositories. This is where they do their hacking
and keep track of their commit history. There is one canonical
repository. However, this canonical repository never has commits
pushed to it directly. To get some change into the canonical
repository (and into the project) you have to send a patch
representing your change to the
[erlware-dev@googlegroups.com](http://groups.google.com/group/erlware-dev)
mailing list. This means that every single change is pushed out and
viewed by everyone on the mailing list. If we foster the community
well, people will give good feedback and code reviews. Hopefully, we
can build a community that is as engaged as the community that
surrounds git itself.

In any case, once the patch arrives on the list, gets commented upon,
changed as required, etc, it will be applied to the canonical
repository and become part of Erlware. Think about this for a second,
every change to Erlware goes through the Erlware dev list as a
patch. Every member of the community has a chance to comment, critique
and discuss it. The direction of the project is obvious to anyone who
has access to the mailing list, which is anyone that wants access. It
makes it extraordinarily easy for anyone to contribute to the project
without ever having to code. They can just subscribe to the list and
provide their knowledge and insight on the code passing through to the
implementers of the patch. Of course, the commiters have the final say
into what actually makes it into the canonical repository. However,
the community has a huge amount of leeway in making sure that the code
is correct and of the highest quality.

You may think that having to submit a patch would surely slow down
development on the project. However, you would be wrong. Each
developer has his own personal repository with which he can do
anything he wants. We encourage developers, and require commiters, to
make their repository publicly available. So that developers and
anyone working with that developer can have easy access to each
other's code. Their development velocity can be anything they are
comfortable with. When they finally have something that they think is
ready for commit to the canonical repository, they can create patches
and submit them. Of course, they will need to spend some time actually
refactoring their changes into a nice set of small interrelated
patches. This, though, is time well spent and will give them one final
chance to refactor their code.

Overall, this should be a huge boost to our productivity as a project
and our transparency as project leaders.

### The Nuts and Bolts

Actually getting this entire thing set up isn't a trivial project. You
need to have some public place to put your git repo,
http://git.erlware.org/git or git://git.erlware.org/git in our
case). You need to set up that repo with, at the very least, an http
server fronting it. You should probably also set up the git-daemon. It
makes cloning a git repository very, very fast. Much faster then
cloning a repository over http. git-daemon doesn't really have any
idea of permissions or users though. So you should set this up for
read access only. You do that by making sure that the user git-daemon
is running as doesn't have authority to write to any of the files in
the repo.

### Setting Up a Git Repo

These instructions work whether you are setting up your own repository
or the canonical repository.

1) Create a directory for your repos. You can just use mkdir for this,
 though if you are going to have multiple submitters you probably want
 to set the sticky group bit on the directory.


    $> mkdir repo_dir
    $> chown me:group_that_every_commiter_is_in repo_dir
    $> chmod g+s repo_dir
    $> cd repo_dir
    $> mkdir project_git_dir
    $> cd project_git_dir
    $> git --bare init

This is going to act as a public server, so we want to enable a bit of
index generation. To do that we simply make the post\_update hook
executable. Git will do the right thing with that.

$> chmod a+x hooks/post_update

If you look in post update you will see the command
'git-update-server-info'. This command allows git to update the
indices that it needs when cloning over a 'dumb' protocol like http.

Point your http server at the repo and you are done! If you want to do
some fancy stuff like send an email on every commit then you need to
get then copy the post\_receive\_email script from the contrib/hooks
directory of the git source tar ball to hooks/post\_receive in your
git repo. Then make that file executable. You need to fiddle with the
config a bit, but the instructions for how to do that are in the file
itself so there isn't any need to repeat it here.

    $> cp /contrib/hooks/post_receive_email ./hooks/post-receive
    $> chmod a+x ./hooks/post-receive

### Working as a Member of The Project

Working with a git project means that there are three commands that
you are going to be using quite a lot. These are
[git-format-patch](http://www.kernel.org/pub/software/scm/git/docs/git-format-patch.html),[ git-send-email](http://www.kernel.org/pub/software/scm/git/docs/git-send-email.html),
and
[git-am](http://www.kernel.org/pub/software/scm/git/docs/git-am.html). Getting
good with these commands will let you interact well with Erlware and
any other git based project.

[git-format-patch](http://www.kernel.org/pub/software/scm/git/docs/git-format-patch.html)
takes some command line options to detail which commits you want to
turn into patches and then writes the appropriate patches out a set of
files, one patch per commit. Its dirt simple, though you will need to
learn how to structure your commits in a reasonable way. This isn't
hard, but it is a bit involved so I will save that process for another
blog. In any case, you will learn how to do it pretty quick once you
start supplying patches.

[git-send-email](http://www.kernel.org/pub/software/scm/git/docs/git-send-email.html)
lets you take the patches created by git-format-patch and send them to
a specified email address. The process is very well documented in the
man page linked above so there really isn't any need to go into much
detail. One note though, if you are a gmail user and don't already
have sendmail or procmail setup to use gmail, I suggest you use
[msmtp](http://msmtp.sourceforge.net/). There are detail instructions
on how to do it on the
[GitTips](http://www.blogger.com/%5Bhttp://git.or.cz/gitwiki/GitTips#head-a015948617d9becbdc9836776f96ad244ba87cb8)
page of the GitWiki. If you are as blunt as I am, following the
instructions there will save you a huge amount of time.

Finally,
[git-am](http://www.kernel.org/pub/software/scm/git/docs/git-am.html)
allows you to take patches from the mailing list and apply them to
your local git repository. It understands both mbox format and raw
patches generated by git-format-patch. This makes it pretty damn
useful. You can set up a pull from your mail server to a local mbox
and then just run git-am on it. You can also just pull the patches
manually and run git-am on those. The whole process is really well
thought out and very, very simple.

The only real problem that I have had so far is that git-send-email
doesn't let you prepend any information to the subject line of the
sent email. They all end up with [PATCH] . This would be fine for most
projects but we run several projects under Erlware and it would be
really nice if we could do something so that the subject looked
something like [PROJECT][PATCH] . Anything that would let us indicate
the project would be great. When and if I get some time I intend to
remedy this and send a patch back to the git community.

### Conclusion

That's it. No doubt I have missed a huge number of things. Hopefully,
the commenters will be nitpicky and point them out so I can fix the
problems. I am really excited about this new model and expect it to do
some really awesome things for our project. If it doesn't do anything
but spread knowledge about the codebase to all our commiters I will be
overjoyed.
