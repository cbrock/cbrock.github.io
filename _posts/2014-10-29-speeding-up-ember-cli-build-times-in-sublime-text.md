---
layout: post
title:  "Speeding up ember-cli build times in Sublime Text 2"
date:   2014-10-29 19:34:24 -0800
---
I'm pretty excited about the (relatively) new ember-cli project. [You can read all about exactly why it's exciting](http://www.ember-cli.com/#why), but one of the central components of the project is its Brocolli-powered build pipeline.

I started experimenting with the project at version 0.0.46 and immediately noticed incredibly slow build times. Exactly the opposite of what I was expecting, and it was so bad as to be pretty much unusable - imagine writing a test and waiting almost a minute for the project to rebuild and the test suite to be run:

    version: 0.0.46
    Livereload server on port 35729
    Serving on http://0.0.0.0:4200

    Build successful - 51668ms.

After upgrading to ember-cli 0.1.0 I immediately saw initial build times decrease by 6-7x, but with build times consistently around 6-8s, it still felt pretty unusable:

    version: 0.1.1
    Livereload server on port 35729
    Serving on http://0.0.0.0:4200

    Build successful - 6523ms.

It turns out that my editor, Sublime Text 2, was causing the slowdown via its indexing of the auto-generated `tmp` directory (which the build process makes much use of). The ember-cli docs mention Sublime Text 3 as potentially causing issues ([though they have since been updated](https://github.com/stefanpenner/ember-cli/pull/2385)), but it took me longer than it should have to realize Sublime Text 2 can exhibit similar behavior.

Sublime Text 2's user settings are available at `Preferences -> Settings - User`  (or simply hit <kbd>&#8984;</kbd> + <kbd>,</kbd> to open the file directly.)

Then, ignore ember-cli's `tmp` directory:

    "folder_exclude_patterns": ["tmp"]

Of course, modify the specific pattern to suit your needs. But aftwards, if using Sublime Text, you should experience build times as I'm sure the ember-cli authors intended them to be. In one particular project, I'm experiencing initial build times of around 4s and rebuilds of <1s.
