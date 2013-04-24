---
layout: post
title: "Resolving difficult merges"
date: 2013-04-24 20:09
comments: true
categories: git
---

On my way home tonight I went to ship a feature on groundstation that'd been
hanging around for a while. Lots of work had happened on both sides, and so
when I went to merge it, I was greeted by a LOT of merge conflicts.

First I tried just rebasing it on master, but when that fell over in the same
way I decided to take a new approach. What I did is rougly analogous to a
rebase, with the exception that I didn't have to lose any history (I'll end up
with the original commits merged) but I also won't create a ton of noise in my
logs.

I opened [tig][1] with the graph view of my branch and master `tig origin/master features/tip_signing`,
and then merged each commit by hand.

Having the diff that each commit had on the topic branch makes this easy, even
if the original commit was a PITA.

Once I finished merging, I was left with this monstrosity:

```
*   5194cc8 (HEAD, master) Merge commit '6e27da1' Richo Healey 8 seconds ago
|\
| * 6e27da1 (origin/features/tip_signing, features/tip_signing) Clean up the input form richo 2 weeks ago
* |   0fbf1f1 Merge commit 'c417108' Richo Healey 2 minutes ago
|\ \
| |/
| * c417108 Border around input bofixupx richo 2 weeks ago
* |   d90c150 Merge commit 'a21bca6' Richo Healey 3 minutes ago
|\ \
| |/
| * a21bca6 Clean up styling richo 2 weeks ago
| * 2b4753d Give airship the option of a signing key richo 2 weeks ago
| * 1f0e7f3 Use good/bad colours from bootstrap richo 2 weeks ago
* |   c1a0c50 Merge commit '1d4e710' Richo Healey 3 minutes ago
|\ \
| |/
| * 1d4e710 Send signature status to the client properly richo 2 weeks ago
| * 10db7fe Show signature status in the UI richo 2 weeks ago
* |   1d88e3a Merge commit 'f46ac6a' Richo Healey 3 minutes ago
|\ \
| |/
| * f46ac6a Squashed commit of the following: richo 2 weeks ago
* |   7e56994 Merge commit '2fa4d54' Richo Healey 4 minutes ago
|\ \
| |/
| * 2fa4d54 Script to sign grefs richo 2 weeks ago
| * 59d2004 Implement lookup of crypto adaptors richo 2 weeks ago
* |   d2dede5 Merge commit '70bcce1' Richo Healey 5 minutes ago
|\ \
| |/
| * 70bcce1 Send signatures with responses in airship richo 3 weeks ago
* |   5de2154 Merge commit 'ce2327c' Richo Healey 7 minutes ago
|\ \
| |/
| * ce2327c Load crypto adaptor on station richo 3 weeks ago
* |   aa627f0 Merge commit 'f76d976' Richo Healey 8 minutes ago
|\ \
| |/
| * f76d976 Nuke a ton of dead user code richo 3 weeks ago
| * 0e6e9fb Test deals with untrusted signatures richo 3 weeks ago
| * d4f2b35 Sanely marshall and store signatures richo 3 weeks ago
| * dee7b12 Tests for get signature richo 3 weeks ago
* |   2da7487 Merge commit 'ea46f19632003397a64650a4aa755416288dfcf9' Richo Healey 15 minutes ago
|\ \
| |/
| * ea46f19 Represent tips as (tip, signature) tuples richo 3 weeks ago
* | fb531e2 (origin/master, features/integration-testing/master) Test that we can send objects between nodes Richo
* | 11f6f12 (origin/features/integration-testing/master) Clean up after running tests Richo Healey 4 days ago
* | 2a5fb9a Simple test that stations can connect to one another Richo Healey 4 days ago
* | 73e0a7c Cleanup stream_client Richo Healey 4 days ago
* | ba7172a (features/socket_api) Test station init Richo Healey 2 weeks ago
* | 433ffc1 Bootstrap stations from environment richo 2 weeks ago
* |   2048da0 Merge pull request #23 from richo/bugs/ff-rendering Richo Healey 2 weeks ago
```

Once you've done this merge, you just need the name of the tree (Not the commit!) which you can get from `git cat-file -p HEAD`, which will emit something like:

```
tree 2efc6dee9eb621a1e95e88294228456d88d7790f
parent 0fbf1f189dd6d6e155d3e6ccd1750533e31c2d11
parent 6e27da1e890adfff452c3a4b6807d48e15614bc6
author Richo Healey <richo@psych0tik.net> 1366797843 +1000
committer Richo Healey <richo@psych0tik.net> 1366797843 +1000

Merge commit '6e27da1'
```

Now it's time to construct our "real" merge:

`echo "Merge branch 'features/tip_signing'" | git commit-tree -p origin/master -p features/tip_signing 2efc6dee9eb621a1e95e88294228456d88d7790f`

This will commit the final tree we arrived at, as a new commit that's a child
of both master and our topic branch (a merge), but without the messy history.
[1]: http://jonas.nitro.dk/tig/
