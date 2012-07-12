Building an Application with ClojureScript and the Closure Library
==================================================================

*Note:* This was taken whole cloth from the
 [Google Closure Tutorial](http://code.google.com/closure/library/docs/tutorial.html)
 and translated to (hopefully) idiomatic ClojureScript. I take credit
 as the translator. However, I can take no authorship credit. You can
 find the complete source for this tutorial on
 [github](https://github.com/ericbmerritt/google-closure-tutorial).

This tutorial gives you hands-on experience using
[ClojureScript](https://github.com/clojure/clojurescript) and the
[Google Closure Library](http://code.google.com/closure/) by walking
you through the construction of a simple application. To do this
tutorial you should have some experience with JavaScript and
Clojure. You should have already gone through the ClojureScript the
setup process, as described on the
[ClojureScript Site](https://github.com/clojure/clojurescript/wiki/Quick-Start)
or as
[I described](http://ericbmerritt.posterous.com/setting-up-clojurescript)
in a previous post.  This tutorial explains the different parts of
these source files step by step.

### The Notes Application

This tutorial illustrates the process of building a simple application
for displaying notes. The example:

* creates a namespace for the application,

* uses the Closure Library's goog.dom.createDom() through the
ClojureScript
[dom-helpers](https://github.com/clojure/clojurescript/blob/master/samples/twitterbuzz/src/twitterbuzz/dom-helpers.cljs)
functions to create the Document Object Model (DOM) structure for the
list,

* and uses a Closure Library class through ClojureScript in the note
list to allow the user to open and close items in the list.

### Creating a Namespace

When you use JavaScript libraries from different sources, there's
always the chance that some JavaScript code will redefine a global
variable or function name that you, yourself, have defined in your
code, creating a nasty bug. Clojure doesn't have this problem and, by
extension, neither does ClojureScript, though it compiles to
javascript.

In clojure script we define namespaces in exacty the same way we do in
Clojure. For our notepad application we can define the
tutorial.notepad.note namespace as follows:

    (ns tutorial.notepad.note
      (:require [tutorial.dom-helpers :as dom]))

Once the tutorial.notepad.note namespace exists, the example creates
the initialization function in the Note namespace.

    (defn init
      [title content node-container]
      {:title title :content content :parent node-container})

The note init function is now in the tutorial.notepad.note namespace
created with the ns macro.

### Creating a DOM Structure with dom-helpers

First and formost, go get the
[dom-helpers.cljs](https://github.com/clojure/clojurescript/blob/master/samples/twitterbuzz/src/twitterbuzz/dom-helpers.cljs)
from the twitterbuzz sample app and stick it in your project. It has the twitterbuzz namespace and you probably wont want to keep that. I changed it to the tutorial.notepad.dom-helpers namespace and refer to it as such below.

To display a Note in the HTML document, the example gives the note
namespace the following method:

    (defn make-note-dom
      [self]

      (let [header-element  (dom/build
                         [:div {:style "background-color:#EEE"}
                          (:title self)])
        content-element (dom/build
                         [:div (:content self)])
        new-note (dom/build
                  [:div header-element content-element])]

       (dom/append (:parent self)
              new-note)
      ;; Return an updated self object with the above declarations
      (-&gt; self
          (assoc :header-element header-element)
          (assoc :content-element content-element))))

This function uses the dom-helpers function build. The following 'require'
statement includes the code for this function:

    (ns tutorial.notepad.note
      (:require [tutorial.dom-helpers :as dom]))

The require directive for the namespace macro in ClojureScript works
exactly the same way as the require in Clojure. There is nothing
further to worry about.

The function build in dom-helpers creates a new DOM element using a
syntax very similar to that used in the Hiccup library.  For example,
the following statement from dom-helpers creates a new div element.

    ;; Remember we imported dom-helpers as dom
    (dom/build
      [:div {:style "background-color:#EEE"} (:title self)])

The map in the vector that starts with the :div keyword specifies
attributes to add to the element, and the vector is terminated by the
child to add to the element (in this case a string). Both the map and
the child specifiers are optional.

The make-note-dom function just makes a single Note argument. To make
a list of notes, the example includes a make-notes function that takes
a vector of note data, as a vector of maps, and instantiates a Note
for each item, calling the make-note-dom function for each one.

   (defn make-notes
     [data node-container]
     (doseq [cont data]
       (let [self
             (init (:title cont) (:content cont) node-container)]
         (make-note-dom self))))

### Using a Closure Library Class

With just a few lines of code the example makes each note a Zippy.A
Zippy is an element that can be collapsed or expanded to hide or
reveal content.  First the example adds a new require element to the
tutorial.notepad namespace. The namespace should now look like.

    (ns tutorial.notepad.note
      (:require [tutorial.notepad.dom-helpers :as dom]
                [goog.ui.Zippy :as zippy]))

Then it adds a line to the make-note-dom function:

    (defn make-note-dom
      [self]

      (let [header-element  (dom/build
                             [:div {:style "background-color:#EEE"}
                              (:title self)])
            content-element (dom/build
                             [:div (:content self)])
            new-note (dom/build
                      [:div header-element content-element])

            ;; NEW LINE
            zippy (goog.ui.Zippy. header-element content-element)]

          (dom/append (:parent self)
                  new-note)

          ;; Return an updated self object with the above declarations
          (-&gt; self
              (assoc :header-element header-element)
              (assoc :content-element content-element)
              (assoc :zippy zippy))))


The constructor call (new goog.ui.Zippy. header-element
content-element) attaches a behavior to the note element that will
toggle the visibility of content-element when the user clicks on
header-element. For more information about the Zippy class, see the
[Zippy API documentation](http://closure-library.googlecode.com/svn/docs/class_goog_ui_Zippy.html)

### Using the Notepad in an HTML Document

Here is the complete ClojureScript code for this example application:

    (ns tutorial.notepad.note
      (:require [tutorial.dom-helpers :as dom]
                [goog.ui.Zippy :as zippy]))

    (defn init
      [title content node-container]

      {:title title :content content :parent node-container})

    (defn make-note-dom
      [self]

      (let [header-element  (dom/build
                             [:div {:style "background-color:#EEE"}
                              (:title self)])
            content-element (dom/build
                             [:div (:content self)])
            new-note (dom/build
                      [:div header-element content-element])
            zippy (goog.ui.Zippy. header-element content-element)]

          (dom/append (:parent self)
                  new-note)
          ;; Return an updated self object with the above declarations
          (-&gt; self
              (assoc :header-element header-element)
              (assoc :content-element content-element)
              (assoc :zippy zippy))))

    (defn make-notes
      [data node-container]
      (doseq [cont data]
        (let [self
              (init (:title cont) (:content cont) node-container)]
          (make-note-dom self))))


We can't embed ClojureScript in html like we can with javascript. This
is a very good thing, but I digress. This does mean that we need to do
things just a bit differently. In our case we are going to create
another file in the notepad namespace called core.cljs that will
define a main function for us. It basically takes the data required
and calls the various note functions.

    (ns tutorial.notepad
      (:require [tutorial.notepad.note :as note]
                [tutorial.dom-helpers :as dom]))

    (defn main
      []
      (let [note-data [{:title "Note 1" :content "Content of Note 1"}
                       {:title "Note 2" :content "Content of Note 2"}]
            note-container (dom/get-element :notes)]

        (note/make-notes note-data note-container)))

    ;; Take note of this, this is how we call main at startup!
    (main)

### Including Kicking off the Process with HTML

With Google Closure and ClojureScript how we call the generated
javascript depends on whether or not we compiled with the advanced
optimizations. If you compiled without advanced optimizations
[(see getting started)](http://ericbmerritt.posterous.com/setting-up-clojurescript)
then your html file should look like this:


If you compiled with advanced optimizations your html should look like this:

    &lt;!doctype html&gt;
    &lt;!--[if lt IE 7 ]&gt; &lt;html lang="en" class="no-js ie6"&gt; &lt;![endif]--&gt;
    &lt;!--[if IE 7 ]&gt;    &lt;html lang="en" class="no-js ie7"&gt; &lt;![endif]--&gt;
    &lt;!--[if IE 8 ]&gt;    &lt;html lang="en" class="no-js ie8"&gt; &lt;![endif]--&gt;
    &lt;!--[if IE 9 ]&gt;    &lt;html lang="en" class="no-js ie9"&gt; &lt;![endif]--&gt;
    &lt;!--[if (gt IE 9)|!(IE)]&gt;&lt;!--&gt; &lt;html lang="en" class="no-js"&gt; &lt;!--&lt;![endif]--&gt;
    &lt;head&gt;
        &lt;meta charset="UTF-8"&gt;
        &lt;meta http-equiv="X-UA-Compatible" content="IE=edge,chrome=1"&gt;

        &lt;title&gt;Google Closure Tutorial for Clojurescript&lt;/title&gt;

        &lt;meta name="description" content="Demo showing off the ClojureScript hotness"&gt;
        &lt;!--[if lt IE 9]&gt;
        &lt;script src="http://html5shiv.googlecode.com/svn/trunk/html5.js"&gt;&lt;/script&gt;
        &lt;![endif]--&gt;

    &lt;/head&gt;
    &lt;body&gt;
        &lt;header&gt;
            &lt;div id="notes"&gt;&lt;/div&gt;

            &lt;script type="text/javascript" src="js/goog/base.js"&gt;&lt;/script&gt;
            &lt;script type="text/javascript" src="js/deps/tutorial.js"&gt;&lt;/script&gt;
            &lt;script type="text/javascript"&gt;
                goog.require('tutorial.notepad');
            &lt;/script&gt;
    &lt;/body&gt;
    &lt;/html&gt;

Note two things, the first is that we no longer need to include the
goog/base.js script and the second is that we no longer need to have
the explicit require in our html file.

Also note that we are including the script after the notes div has
been defined. That's rather important.

### Putting It All Together

Go ahead and checkout the project from github.

    $ get clone https://github.com/ericbmerritt/google-closure-tutorial.git
    $ cd google-closure-tutorial

#### The Unoptimized Build

From the project root (where you should be right now). Run the the following:

    $ ./bin/compile.sh

This does the unoptimized build. You may inspect the compile.sh script
to see the actual command that is run. There is a very simple web
server included in the distro. From the root again, do the following:

   $ ./bin/webserver.sh

Now you can point your browser to
*http://127.0.0.1:8000/index-unoptized.html* to see the result.

#### The Optimized Build

Again from the project root, run the command:

    $ ./bin/compile-optimized.sh
    $ ./bin/webserver.sh

This will do the exact same thing as the unoptimized, just with
optimization turned on. Again, you may inspect compile-optimized.sh to
see the command details.

### Notes

I came a across a few realizations as I was working through this and
thought I would share them with you. You should compile your
ClojureScript with both advanced options on and off. Advanced options
being on is going to help you catch a lot of errors at compile time
that you would otherwise have to find at run time. However, its
absolutely impossible to debug. So during development you will mostly
be compiling without advanced options. Make sure, though, that you
compile with advanced options on a fairly frequent basis. You will be
happy you did in the long run. At the very least anyway you are going
to want to compile with advanced options before deploying.
