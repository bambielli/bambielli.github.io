---
layout: post
title:  "Blog Mechanics"
date:   2016-01-30 9:40:00
categories: general
---

I figured since I am using [github pages][github-pages] to host my site and version my content, I might as well take advantage of other github features to help organize and manage updates and treat this blog like any other professional software project.

Issue Log (Work Item Tracking):
The [issue log][issue-log] is used to track work items to be done. There is a set of custom tags that I will use to keep my issue log organized: "Post", "Bug", and "Enhancement". "Post" issues contain topics for future blog posts (this one is currently being tracked as issue [#3][#3]) and lists of ideas for each. "Bug" issues are true bugs that need to be fixed, while "Enhancements" are other areas of the site that could use polish but aren't mission critical.

Milestones (Timeboxing):
To ensure work is being completed at regular intervals, I'm using the [milestone][milestone] tracking system to timebox and put a due date on a chunk of work. The milestone for this week tracks completion of issues [#3][#3] and [#4][#4]. I'll do bi-monthly milestones to start so I'm pushing new content at least once every 2 weeks. If I hit a milestone, I'm going to post about it either on linkedIn, facebook or twitter. If I miss one, I'm going to run an extra 5k at the gym :)

Branching (WIP):
Github Pages deploys "master" to production. I don't want to accidentally deploy WIP to the site before it is ready, so each work item will be implemented in a new branch off of master. This way if I'm in the middle of some work, but want to switch gears for a bit, I can commit changes to a branch without affecting the production-ready state of master. I figure I'll use the naming convention "issue-type/work-item-name" for each branch (This branch is "post/blog-mechanics").

Pull Requests (Reviews):
Once a work item is completed in a branch, I'll open a [pull request][pull-request] from the branch back in to master where I'll do some final proofing and editing of the change using the pull request review system. Once "Code Review" is complete, I'll merge the branch back in to master which will auto deploy to my site. Pretty nifty!

This might wind up being too much overhead for the purposes of a personal website...but it has been a fun exercise to think about how a blog could conform to best practices for developing and managing software projects! I'll try it out for a while and see what I think.


[github-pages]:       https://pages.github.com/
[issue-log]:          https://github.com/bambielli/bambielli.github.io/issues
[milestone]:          https://github.com/bambielli/bambielli.github.io/milestones
[#3]:                 https://github.com/bambielli/bambielli.github.io/issues/3
[#4]:                 https://github.com/bambielli/bambielli.github.io/issues/4
[pull-request]:       https://github.com/bambielli/bambielli.github.io/pulls
