---
layout: post
title:  "Blog Mechanics - Thinking Like a Software Project Manager"
date:   2016-01-30 11:30:00
categories: general
---

Since I am using [github][github]{:target="_blank"} to host and version my content, I might as well take advantage of other features of the platform to help organize and manage updates to my site. We use github for most of our software project management mechanisms at work, so implementing here should feel pretty natural. Initial thoughts on how each tool will be used:

#### [Issue Log (Work Item Tracking)][issue-log]{:target="_blank"}:
The issue log is used to track work items to be done. There is a set of custom tags that I will use to keep my issue log organized: "Post" issues contain topics for future blog posts and lists of ideas to cover in each (this post is currently being tracked as issue [#3][#3]{:target="_blank"}). "Bug" issues are true bugs that need to be fixed, or content that is still missing from the site. "Enhancements" are other areas of the site that could use polish but aren't mission critical. Using tags in the issue log allows me to filter on what types of issues are currently open when I prioritize work for the next milestone.

#### [Milestones (Timeboxing)][milestone]{:target="_blank"}:
To ensure work is being completed at regular intervals, I'm using the milestone system to timebox and assign a due date to a chunk of work. The milestone for this week tracks completion of issues [#3][#3]{:target="_blank"}, [#4][#4]{:target="_blank"}, [#5][#5]{:target="_blank"} and [#6][#6]{:target="_blank"}. I'll be doing bi-monthly milestones to start, to ensure that I'm pushing new content at least once every 2 weeks. If I hit a milestone, I'm going to post about it either on linkedIn, facebook or twitter. If I miss one, I'm going to run an extra 5k at the gym.

#### Branching (WIP):
Github Pages deploys "master" to production. I don't want to accidentally deploy WIP to my site before it is ready, so each work item will be implemented in a new branch off of master. This way if I'm in the middle of some work, but want to switch gears for a bit, I can commit changes to a branch without affecting the production-ready state of master. I'll use the naming convention "issue-type/work-item-name" for each branch (This branch is "post/blog-mechanics"). As I commit, I'll try to remember to include a commit message like "fixes #3" to take advantage of github's [issue auto-close][issue-auto-close]{:target="_blank"} feature.

#### [Pull Requests (Code Reviews)][pull-request]{:target="_blank"}:
Once a work item is complete, I'll open a pull request from its containing branch back in to master. This will allow me to review a work item before deploying to production. During this review, I will go over a [checklist][checklist]{:target="_blank"} that I've developed to ensure work item quality. To borrow from agile project management, this acts as the [definition of done][definition-of-done]{:target="_blank"} for the work item. The checklist can be added as a comment to each pull request, to ensure steps aren't forgotten. Once code review is complete, I'll merge the branch back in to master which will auto deploy to my site. Pretty nifty! The header of this section links to the pull request that merged this post in to master if you'd like to see the process in action.

This might wind up being too much overhead for the purposes of a personal website...but it has been a fun exercise to think about how a blog could conform to best practices for developing and managing software projects!

I'll try it out for a while and see what I think.


[github]:             https://pages.github.com/
[issue-log]:          https://github.com/bambielli/bambielli.github.io/issues
[milestone]:          https://github.com/bambielli/bambielli.github.io/milestones
[definition-of-done]: https://github.com/bambielli/bambielli.github.io/wiki/Post-Definition-of-Done
[#3]:                 https://github.com/bambielli/bambielli.github.io/issues/3
[#4]:                 https://github.com/bambielli/bambielli.github.io/issues/4
[#5]:                 https://github.com/bambielli/bambielli.github.io/issues/5
[#6]:                 https://github.com/bambielli/bambielli.github.io/issues/6
[checklist]:          https://www.goodreads.com/book/show/6667514-the-checklist-manifesto
[pull-request]:       https://github.com/bambielli/bambielli.github.io/pull/15
[issue-auto-close]:   https://help.github.com/articles/closing-issues-via-commit-messages/
