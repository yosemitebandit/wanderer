An attempt to dupe the github contributions pane.
The first pass revealed no immediate changes when I pushed commits with spoofed dates.
It was thought that maybe github built the contributions pane from an internal log of when *pushes* occur, rather than when actual commits are made.
So that would be that..

But suddenly!  A newfound blip appeared on the contributions pane for the spoofed date.
I'm not sure which of the tested methods actually worked..
Let's be a bit more scientific.
I'd like to test these techniques again:

 * modifying the system time via `date`
 * amending a commit with a new date
 * manually setting `GIT_AUTHOR_DATE` and `GIT_COMMITTER_DATE` ([source](http://www.alexpeattie.com/blog/working-with-dates-in-git))
 * modifying the RTC via `hwclock`

As of January 30, 2013, my contributions pane has no commits on these days:

* November 6, 2012
* November 13, 2012
* November 20, 2012
* November 27, 2012

I'll do the following in a [new repo](https://github.com/yosemitebandit/nomad):

```
# modify the system time:
$ sudo date --set "6 Nov 2012 11:00:00"
$ git init
$ echo date > readme.md
$ git add readme.md
$ git commit -m 'edited after modifying system time'
$ git push

# amend the commit date
$ git commit --amend --date="Tue Nov 13 12:00 2012 +0800"
# this set the git author date to Nov 12, actually (also a day sans commits)
$ git pull
$ git push

# modify the env vars
$ export GIT_AUTHOR_DATE="Tue Nov 20 13:00 2012 +0800"
$ export GIT_COMMITTER_DATE="Tue Nov 20 13:00 2012 +0800"
$ echo env > readme.md
$ git add readme.md
$ git commit -m 'modifies the date via env vars'
$ git push

# ..saving hwclock changes for later
```

And on January 31st, only 15-or-so hours after running the above, there is a commit for November 6th in my contributions pane.
None of the other spoofed dates appear.
So far, this suggests that modifying the system time is the way to go..
