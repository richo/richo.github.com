---
layout: post
title: "the point of writing software isn't to maintain it forever"
date: 2013-04-24 12:02
comments: true
categories: software release maintenaince
---

Today I went to install [twat][1], an admittedly slightly buggy piece of
software I wrote when I was first learning Ruby. It more or less does what it
says on the tin, but when I went to install it, I discovered that [all the versions of the twitter gem it used have been yanked][2].
I get it. You shouldn't be writing new software using old, unmaintained versions of gems.

However, *releases* are just that. Released. Snapshots. Yanking them will break
things. Unless something is actually broken (critical bug, security issue) I
don't understand the rationale for doing this. The only explaination is that
you want people to update, which hinges on the predicate that people are still
working on all the software it uses.

*This isn't always the case*. The version of cron(8) that OSX ships with hasn't
been touched since 1994. (From it's [release notes][3], I can't find it's homepage:

Vixie Cron      V3.0 Patch 2 notes
Paul Vixie
12-Dec-1994

Can you imagine if someone broke a dependency of this arbitrarily? Do you think
Paul Vixie really wants to hack on this for the first time in 20 years to bump
some version and change some internal details?

TL;DR why on earth would you yank working software?


[1]: https://github.com/richo/twat
[2]: http://rubygems.org/gems/twitter/versions
[3]: ftp://ftp.isc.org/isc/cron/cron_4.1.shar
