---
layout: post
title: Off Line Development
date: 2007-05-18
comments: false
tags: [erlang,sinan,progress,build]
---

After numerous requests I have finally implemented off-line
development. You no longer have to be connected to build your
projects. In the past the dependency analysis task ran every build and
if it thought that there was a chance that the dependencies had
changed it connected to the repository to check the dependencies. This
no longer happens. There is now a `check_depends` task that checks to
make sure that dependencies have been run at some time in the past. It
then checks if the dependencies need to be updated. If the
dependencies do need to be updated it asks the user if he wants to
update the dependencies (by connecting to the server). If the answer
is affirmative then the update occurs if not then it continues with
the existing dependencies. The user may run dependencies at any time
by running the depends task directly. This approach gives users much,
much more control over when and how dependencies are resolved. It also
allows the user to control when and how the build system connects to
the repository. I hope I have done this in such a way as to not add
any additional burden to the user.
