---
title:  "Project Managing a Blog?"
date:   2016-01-30 19:30:00
category: post
tags: [project-management, github]
---

Since I am using [github][github]{:target="_blank"} to host and version my content, I might as well take advantage of other features of the platform to help organize and manage updates to my site.

Some initial thoughts on how I plan to use each tool:

## [Issue Log (Work Item Tracking)][issue-log]{:target="_blank"}:
Work items to be completed will be tracked in the issue log. Each item will be tagged with one of the following: "Post" issues will contain topics for future blog posts and lists of ideas to cover in each (this post is currently being tracked as issue [#3][#3]{:target="_blank"}). "Bug" issues will track a bug that needs to be fixed, or content that is still missing from the site. "Enhancements" will be used to track non-critical issues or suggestions. Using tags in the issue log will allow me to filter on what types of issues are currently open when I prioritize what to tackle next.

## [Milestones (Timeboxing)][milestone]{:target="_blank"}:
To ensure work is being completed at regular intervals, I plan on using the milestone system to timebox and assign due dates to a chunk of work. The milestone for this week tracks completion of issues [#3][#3]{:target="_blank"}, [#4][#4]{:target="_blank"}, [#5][#5]{:target="_blank"} and [#6][#6]{:target="_blank"}. I'll be doing bi-monthly milestones to start, so expect to see new content at least every two weeks! If I hit a milestone, I'll post about it on social media. If I miss one, I'm going to run an extra 5k at the gym :)

## Branching (WIP):
Github Pages deploys "master" to production. I don't want to accidentally deploy WIP to my site before it is ready, so each work item will be implemented in a new branch off of master. This way if I'm in the middle of some work, but want to switch gears for a bit, I can commit changes to a branch without affecting the production-ready state of master. I'll use the naming convention "issue-type/work-item-name" for each branch (This branch is "post/blog-mechanics"). As I commit, I'll try to remember to include a commit message like "fixes #3" to take advantage of github's [issue auto-close][issue-auto-close]{:target="_blank"} feature.

## [Pull Requests (Code Reviews)][pull-request]{:target="_blank"}:
Once a work item is complete, I'll open a pull request from its branch back in to master. This will allow me to review changes proposed by the branch before deploying to production. During this review, I will go over a [checklist][checklist]{:target="_blank"} that I've developed to ensure work item quality. To borrow from agile project management, this acts as the [definition of done][definition-of-done]{:target="_blank"} for the item under review. The checklist will be added to the top of each pull request to ensure steps aren't forgotten. Once code review is complete, I'll merge the branch back in to master which will auto deploy to my site. Pretty nifty! The header of this section links to the pull request that merged this post in to master if you'd like to see the process in action.

This might wind up being too much overhead for the purposes of a personal website...but it has been a good exercise to think about how a personal website could conform to best practices for developing and managing software projects! This post went through the full process described above before entering production, and I think it worked out well.

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
