---
title:  "Quickly Resolving Merge Conflicts from the Command Line"
date:   2017-01-21 21:32:00
category: til
tags: [git]
---

TIL of a quick way to quickly resolve merge conflicts from the command line, in a situation where you know you want to fully accept or reject the merging branch's changes.

I was recently in a situation at work where I had branched off a feature branch for quite some time, then wanted to merge my changes back in to the base branch. Unfortunately, this resulted in a bunch of merge conflicts since the branch was still somewhat in active development while I was branched off. The number of files impacted was somewhere between 15 and 20. 

I knew that I wanted to keep their changes for certain files, and my changes for others, but I wasn't looking forward to going through and manually resolving conflicts file by file... 

Luckily I found this [stack overflow post][so]{:target="_blank"} which provided an excellent way to resolve conflicts from the command line in just this situation. For files where I wanted to keep all of my changes, I could execute…
```
git checkout --ours -- path/to/file
``` 
…and all of "our" changes (made in my branch) would be kept in the conflict resolution. 

The opposite command also exists: in the situation where I want to keep all of "their" changes (the changes made in the base branch in this situation) I could execute 
```
git checkout --theirs -- /path/to/file
``` 

To reiterate, these commands resolve ALL conflicts with either "our" changes or "their" changes, so they should not be used if you need to carefully resolve a merge conflict with a combination of both changes in a file. 

Finally, if I just wanted to accept "our" changes or "their" changes in all files that contained conflicts, I could just run:

```
git merge --strategy-option ours
git merge --strategy-option theirs
```

These commands saved me a bunch of time, and worked like a charm.

[so]: http://stackoverflow.com/questions/161813/how-to-resolve-merge-conflicts-in-git#answer-39771096
