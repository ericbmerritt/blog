---
bg: 'apollo.png'
layout: post
title: Mocking is Evil
tags: [philosophy,development]
comments: true
---

[Mocking](http://stackoverflow.com/questions/2665812/what-is-mocking)
is evil. At the very notion of that, all the citizens of
[Nounlandia](http://steve-yegge.blogspot.com/2006/03/execution-in-kingdom-of-nouns.html),
are screaming for my blood, shouting 'Heresy! Heresy!'. I think I can
hear the sound of sharpening stones on pitchfork tines even as I write
this.  Mocking is fundamentally evil. It encourages you to write bad,
poorly factored code, not-very-functional code. It encourages you to
avoid standing up as much of your system as possible during
integration testing. Finally, all too often at the end of all that
mocking all you end up with are some really well tested Mocks nested
N! levels. In other words, mocking produces a bunch of useless code
that costs money and time to maintain.

Well, smart guy, I can already hear you saying, 'If that's true how do
you test functions? Huh? Tell me that!'. Well the answer to that is
the same as the reply to most other questions like this: By writing,
well factored, idiomatic code. In this case, you need to separate out
your side effect containing code from your side effect free code, then
create unit tests around your side effect free code.

Let's take an example to get your mind around the solution. Let's say
that we want to get the result of the command `git describe --tag
--always` and check to see if it is a git ref or something
else. Well, git refs are hex numbers so we can get away with making a
little function that tests that ref. Let's do this in
Erlang since I just got done writing something similar not half an
hour ago. Let's take the naive approach first.

    -module(foo).

    -export([get_git_ref/0]).

    -spec get_git_ref() -> ok | {error, no_ref}.
    get_git_ref() ->
      PossibleRef = re:replace(os:cmd("git describe --tags --always"), "\\s+",
                               "",
                               [global]),
      case re:run(PossibleRef, "^[0-9a-fA-F]+$")) of
        {match, _} ->
          ok;
        nomatch ->
          {error, noref}
      end.

Obviously, this isn't testable without having a git repo. So our unit
tests aren't possible unless we mock `os:cmd/1` right? No! No! No!. We
can make this testable bit of refactoring. Let's make a non-naive
version.

    -module(foo).

    -export([get_git_ref/0]).

    -spec get_git_ref() -> ok | {error, no_ref}.
    get_git_ref() ->
      is_ref(trim(os:cmd("git describe --tags --always"))).

    is_ref(Ref) ->
      case re:run(PossibleRef, "^[0-9a-fA-F]+$")) of
        {match, _} ->
          ok;
        nomatch ->
          {error, noref}
      end.

    trim(Input) ->
      re:replace(Input, "\\s+", "", [global]).

Notice here that we localize our side-effects to a small function
that calls other functions to do the majority of the work. Every
single one of those other functions is side effect free.  Our code goes from being a side effect dependent mess to well factored
testable code. Well factored, testable code is your goal when it comes to code that has side effects. Taking this approach will vastly reduce the need for Mocks.

Your next complaint will be 'Well this still isn't testable! We don't
export is_ref/1 and trim/1, so how can you test it?'. This issue of
private functions leads us to the next tenant of avoiding Mocks. Your
tests should live near the code that they test. If you have code that
tests functions in a module than those tests should be in that same
module. In Erlang, we would do it as follows, but the principle
applies in any language.

    -module(foo).

    -export([get_git_ref/0]).

    -spec get_git_ref() -> ok | {error, no_ref}.
    get_git_ref() ->
      is_ref(trim(os:cmd("git describe --tags --always"))).

    is_ref(Ref) ->
      case re:run(PossibleRef, "^[0-9a-fA-F]+$")) of
        {match, _} ->
          ok;
        nomatch ->
          {error, noref}
      end.

    trim(Input) ->
      re:replace(Input, "\\s+", "", [global]).

    -ifdef(DEV_ONLY).
    -include_lib("eunit/include/eunit.hrl").

    is_ref_test() ->
       ?assertMatch(ok, "af19665");
       ?assertMatch({error, "foobarbaz"}, "foobarbaz").

    -endif.

Why should your tests be near the code it tests? For the same reason
documentation should be near the things it documents. Tests are a form
of documentation, perhaps the primary form of documentation. That
implies that when the code changes the tests that test that code
change as well. Otherwise, they tend to get out of sync and cause a
lot of flipping back and forth between code and tests when
changing/reading/maintaining anything. You would think this would be a
no-brainer, but I have had people argue vociferously against this.

So now we have tests, but I can hear one last complaint echoing
through the halls of the great Temple of Imperativeness. 'You aren't
testing the functions that contain the side effects!!! You
fail!'. Nice try, but not quite.  While we don't try to unit-test the
functions that contain side effects. This doesn't mean that you don't
try to test them. You need to stand your system up during
testing. That way you can test the functionality your system and not
how well you have mocked it. Figuring that out is very hard. If you
plan that from the beginning of the project, it makes standing things
up much easier. In either case, you still have to do it. Otherwise,
your tests are useless.

If you don't see another post from me, you will know the forces of
Nounlandia has breached my fortress citadel and has taken me
prisoner. Even now they are probably torturing me by forcing me to
write EJBs Java and Groovy using Eclipse on Windows. Pity me. Pity me
greatly.
