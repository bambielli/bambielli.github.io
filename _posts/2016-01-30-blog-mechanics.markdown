---
layout: post
title:  "Blog Mechanics"
date:   2016-01-30 11:30:00
categories: general
---

I figured since I am using [github pages][github-pages] to host my site and version my content, I might as well take advantage of other github features to help organize and manage updates. We use github for most of our software project management mechanisms at work, so implementing here should feel like second nature.

#### [Issue Log (Work Item Tracking)][issue-log]:
The issue-log is used to track work items to be done. There is a set of custom tags that I will use to keep my issue log organized: "Post", "Bug", and "Enhancement". "Post" issues contain topics for future blog posts (this one is currently being tracked as issue [#3][#3]) and lists of ideas for each. "Bug" issues are true bugs that need to be fixed or content that is still missing, while "Enhancements" are other areas of the site that could use polish but aren't mission critical. Using tags in the issue log allows me to filter on what types of issues are currently open when prioritizing work for the next milestone.

#### [Milestones (Timeboxing)][milestone]:
To ensure work is being completed at regular intervals, I'm using the milestone system to timebox and put a due date on a chunk of work. The milestone for this week tracks completion of issues [#3][#3], [#4][#4], [#5][#5] and [#6][#6]. I'll do bi-monthly milestones to start so I'm pushing new content at least once every 2 weeks. If I hit a milestone, I'm going to post about it either on linkedIn, facebook or twitter. If I miss one, I'm going to run an extra 5k at the gym :)

#### Branching (WIP):
Github Pages deploys "master" to production. I don't want to accidentally deploy WIP to my site before it is ready, so each work item will be implemented in a new branch off of master. This way if I'm in the middle of some work, but want to switch gears for a bit, I can commit changes to a branch without affecting the production-ready state of master. I figure I'll use the naming convention "issue-type/work-item-name" for each branch (This branch is "post/blog-mechanics"). As I commit, I'll try to remember to include a commit message like "fixes #3" to take advantage of github's [auto-issue closing][auto-issue-closing] feature when commits that fix issues are merged in to master.

#### [Pull Requests (Code Reviews)][pull-request]:
Once a work item is completed in a branch, I'll open a pull request from the branch back in to master. This will give me opportunity to review a work item before deploying to production. During this review, I will go over a checklist I've developed to ensure work item quality; to borrow from agile project management, this acts as the [definition of done][definition-of-done] for my work item. A checklist can be copied in to each pull request for each type of work item. Once "Code Review" of a work item is complete, I'll merge the branch back in to master which will auto deploy to my site. Pretty nifty! The link in the header of this section links to the pull request that merged this post in to master.

This might wind up being too much overhead for the purposes of a personal website...but it has been a fun exercise to think about how a blog could conform to best practices for developing and managing software projects!

I'll try it out for a while and see what I think.


[github-pages]:       https://pages.github.com/
[issue-log]:          https://github.com/bambielli/bambielli.github.io/issues
[milestone]:          https://github.com/bambielli/bambielli.github.io/milestones
[definition-of-done]: https://github.com/bambielli/bambielli.github.io/wiki/Post-Definition-of-Done
[#3]:                 https://github.com/bambielli/bambielli.github.io/issues/3
[#4]:                 https://github.com/bambielli/bambielli.github.io/issues/4
[#5]:                 https://github.com/bambielli/bambielli.github.io/issues/5
[#6]:                 https://github.com/bambielli/bambielli.github.io/issues/6
[pull-request]:       https://github.com/bambielli/bambielli.github.io/pull/15
[auto-issue-closing]: https://help.github.com/articles/closing-issues-via-commit-messages/
