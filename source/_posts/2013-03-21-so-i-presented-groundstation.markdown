---
layout: post
title: "so I presented groundstation"
date: 2013-03-21 00:21
comments: true
categories: groundstation
---

So this evening I presented [groundstation][1] at [Melbourne Hack and Tell][2]
(Which is badass and if you're in Melbourne you should show up to the next
one), but I wanted to wrap up some of the content.

First off, what is groundstation, specifically?

1. It's a framework for syncing string of immutable objects.
2. It dynamically discovers any peers on it's local network.
3. It uses git as it's underlying storage, but it does NOT use git's data structures.

Right now, it can do two main things.

The first is importing issues from github or jira. The included web interface,
airship, can let you and anyone else using groundstation interact with the
database, and your changes will be synced.

It currently can't push it's changes back up to those systems.

Secondly, it can sync a git repo, however it CANNOT sync your refs, so you're
on your own to communicate updates to branches amongst your peers.

My next steps are optimising the sync protocol for multiple peers, implementing
a routing layer for nodes that aren't on the same local network, and
implementing tip signing.

The primary purpose of this post is to reach out to anyone who's using or
considering using it. I'd love to hear from you, hear what you're doing and
issues you're having.

My intention is to build a distributed platform on which other apps can be
built, so please if you're doing something cool (or thinking about it!) let me
know.

[1]: https://github.com/richo/groundstation
[2]: http://www.meetup.com/Melbourne-Hack-and-Tell/
