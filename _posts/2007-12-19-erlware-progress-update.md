---
bg: 'apollo.png'
layout: post
title: Erlware Progress Update
date: 2007-12-19
comments: false
tags: [erlang,erlware,progress]
---

The other maintainers and I have been coding feverishly to get the
Erlware system to version 1.0 by the end of January. This has taken up
an enormous amount of my time, leaving me very little to time to
blog. It has been so long now that I feel sorry. So I decided that I
needed to spend a bit of time talking how development in our new model
is going.

All I can say is wow! This Open Development Model has proven to be a
huge boon to the project. Not only has it encouraged user
participation by a large margin, but it has also encouraged quite a
bit more design process. Rather spontaneously the maintainers have
started to submit design specs when they are preparing to do major
changes. These design specs are usually one pagers that give a
high-level overview of the problem and describe the nature of the fix
the developer is implementing. It lets the community and the other
maintainers know and comment on whats upcoming. It also makes the
developer think more closely about the changes he is planning to make
to the system. All in all, it doesn't take a significant amount of
time and its helps increase the quality of our offering. Since each
design spec goes to the mailing list, it fits right in with the
low-level patch model. I like it.

Another significant development is that we have moved our website to
the Open Development Model. Initially, we used
[dokuwiki](http://wiki.splitbrain.org/wiki:dokuwiki) for our
site. Dokuwiki is a great product, but its a wiki with the attendant
problems that a wiki has. In our case, we had kept wiki access pretty
limited. Only the core maintainers had access to it. This made it hard
for the community to fix issues and expand docs. What we wanted was to
combine the easy editing of the wiki with our, somewhat more careful,
patch-based approach to changes. We spent some time thinking about the
problem and decided that it would be best if our site were updatable,
via git, just like the rest of our projects. However, we didn't want
to hand edit HTML and do all the manual work involved. We liked the
readable, usable wiki syntax. We needed some mix of wiki with static
source files. We went to google with low expectations. We were
pleasantly surprised by the number of good offerings in various
languages. Eventually we settled on
[webgen](http://webgen.rubyforge.org/). This is a nice little ruby
framework for static site generation. It supports various markups,
including our choice
[Markdown](http://daringfireball.net/projects/markdown/). What we
ended up with is an open site with very nice wiki-ish syntax that is
easy to extend and change via a model that our contributors are
familiar with.

Once a change is in our official git repo getting the changes onto the
site is completely automated. A cronjob on our server pulls the site
from the repository and runs the generator. Then runs rsync to push
the changes into the area that they are served from. This is a very
recent change so we don't know if it will accomplish our purpose of
more contributions to the site. One unexpected side benefit is that
our site is now really fast. All the work is happening at generation
time, so the server just needs to serve static HTML files.