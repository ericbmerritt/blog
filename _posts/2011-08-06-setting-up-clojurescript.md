---
layout: post
title: Setting Up ClojureScript
---

{{ page.title }}
================

Setting up Clojurescript is fully described over at the
[Clojurescript Quick Start](https://github.com/clojure/clojurescript/wiki/Quick-Start). What
I am doing here is just reorganizing and rewriting to make things a
bit more clear to someone that thinks like I do. While I was setting
up [clojurscript](https://github.com/clojure/clojurescript), I missed
a couple of things as I was going along. So I thought it might be useful
to others to go through the process for others. I work on linux, and
this method will work there. I suspect very strongly that it will work
on OSX as well.

Download Clojurescript
----------------------

First and foremost download Clojurescript as below.

    $ git clone https://github.com/clojure/clojurescript

Yes, you need to go ahead and do the git clone. Clojurescript is
moving forward rapidly and will be for the foreseeable future. You are
going to be updating the repo on a regular basis. So go ahead and
clone it. I suggest you put in whatever working directory you use for
projects. I tend to keep my projects in $HOME/workspace and that is
where clojurescript lives on my box. This works out rather well
because I expect to be
[contributing back](http://clojure.org/contributing). Hopefully, you
will too.

Bootstrapping
-------------

You can bootstrap everything by running the bootstrap script in the
clojurescript directory.

    $ ./script/bootstrap
    Fetching Clojure...
    Fetching Google Closure Library...
    Fetching Google Closure Compiler...
    Building goog.jar...
    Copying closure/compiler/compiler.jar to lib/...

it will pull down all the dependencies for clojurescript. Note that
clojurescript relies on clojure 1.3.0beta1 (at the time of this
writing). Its very probable that you are thinking about using
clojurescript as the front end to a project that uses clojure as a
back end. If that is the case its probably best to base your back end
project on clojure 1.3.0beta1 or what ever happens to be the current
version of clojure that clojure script uses.

Setup
-----

If everything went well, the next thing we need to do is to setup the
CLOJURESCRIPT_HOME env variable. This should point to where you
installed clojurescript. If you remember I put my clojurescript in
$HOME/workspace/clojurescript and thats where my CLOJURESCRIPT_HOME
points.

    $ export CLOJURESCRIPT_HOME=$HOME/

Of course, replace  with actual location of your
clojurescript repo.

Setting up the Paths
--------------------

Finally we want to set up the paths. You can either set your system
PATH env variable to include both $CLOJURESCRIPT_HOME/script and
$CLOJURESCRIPT_HOME/bin or you can symlink the cljsc and repl scripts
to a location already in your bin directory. So you might do

    $ export PATH=$PATH:$CLOJURESCRIPT_HOME/bin:$CLOJURESCRIPT_HOME/script

for putting those two in your path. Or you might do

    $ ln -s $CLOJURESCRIPT_HOME/bin/cljsc $HOME/bin/
    $ ln -s $CLOJURESCRIPT_HOME/script/repljs $HOME/bin/

As you might imagine I have $HOME/bin already in my path. Either one
works just fine, so its up to you. What you do not want to do is copy
those scripts into locations in your PATH. Remember those scripts are
going to change.

Try it Out
----------

So lets try it out. Run the repljs, you should see something that
looks like this.

    $ repljs
    #'user/jse
    "Type: " :cljs/quit " to quit"
    ClojureScript:cljs.user&gt;

If you do, then WOOT! you are golden. If you don't then you probably
forgot a step or did something wrong (or something fundamental has
changed since I wrote this). Go back and take a look and see what went
wrong.


Compiling Clojurscript
----------------------

Having a repl is pretty awesome, and you are going to use it; but what
you really want to do is integrate it into your project. At the moment
how you do that is going to vary a lot depending on your
project. However, in general here are two ways to do that. The first
is to use the repl, the second is to run cljsc. I tend to use the
cljsc script because I can stick it in a script.

I am building a project in Google App Engine using
[appengine-magic](https://github.com/gcv/appengine-magic). I would
love to actually have a compiler built into
[leiningen](https://github.com/technomancy/leiningen). However, there
are no lein plugins for clojurescript yet. So I have a little shell
script the runs in the root of the project. That shell script
basically does the following.

    $ cljsc ./src '{:optimizations :advanced :output-dir "war/js" :output-to "war/js/myprojectname.js"}'

That will do the whole program compilation over all your cljs
files. As soon as we get some lein goodness I will be migrating to
that. I like an integrated build system quite a lot.
