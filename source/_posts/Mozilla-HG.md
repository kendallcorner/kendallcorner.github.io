---
title: Useful Mercurial Commands for Mozilla Central
date: 2019-12-29 10:59:44
tags:
---

Some helpful commands for using mercurial with Mozilla Central

`hg pull`

Get newest revisions. mozilla-central changes a lot, so this needs to be done at least daily and before any work is started

`hg update`



`hg wip`

Shows a nice graph of your revisions vs the revisions from central. Shows whether you need to rebase

`hg status`

Shows all files you have changed but not committed

`hg status --rev central`
https://stackoverflow.com/questions/8734817/mercurial-generate-file-name-only-diff

Shows all files you have committed (or not commited) that are different from central. I.e. it can show you all the files in your patch at once if you've made several revisions

`hg commit m "commit message"`

mercurial does not have staging. Straight to commit.

`hg rebase central`

Make sure all your stuff is committed first

`hg shelve`
https://www.markheath.net/post/git-stash-for-mercurial-users
If you don't want to commit the current changes

`hg histedit <rev>`
https://www.mercurial-scm.org/wiki/HisteditExtension
To squash and/or rename commits

Final commit message format:
`Bug 577872 - Create WebM versions of Ogg reftests. r=kinetik`

You may also want to change your default editor (unless you like using vi or vim). You can do that by adding an evironment variable:
`export HGEDITOR=<your favorite editor>`

`hg revert <filename>`
To remove changes to a given file

`hg diff -r central` 
To show changes since central (all commits)

`hg update -r <rev>`
To change to a different rev

`hg parent`
If you get lost in your revisions--to show which you are on.


Tests to run:
./mach lint -l eslint -o

./mach lint -l wpt -l wpt_manifest -o

./mach mochitest browser/components/preferences/in-content/tests/
Run the relevant folder here. All Mochitests will take a long time to run.