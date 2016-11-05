---
title:  "First Open Source Contribution: Hacktoberfest"
date:   2016-11-03 21:36:00
category: post
tags: [hacktoberfest, django, python, open-source]
---

This year I participated in the [Hacktoberfest][hack]{:target="_blank"} challenge presented by Digital Ocean. I had not done much open source contributing before, and Hacktoberfest presented a nice time-boxed opportunity to give it a shot.

### Finding a project

The Hacktoberfest landing page provided a bunch of resources for how to get started in the open source community, as well as a few example repositories looking for new contributors. I found the best way to find a project was by searching github for issues in public repos tagged with the "Hacktoberfest" tag. These were generally smaller / earlier stage projects that had more reasonable issues to tackle than mature projects. I further limited my search to Javascript and Python repositories.

I eventually came upon the [django-konfera][konfera]{:target="_blank"} app: a team out of Bratislava was designing their own event organization application to manage their Django / Python meetup groups. I worked with Django at Syndio, and figured I could lend a hand while refreshing myself on some Django concepts.

I wound up fixing an issue to [add validation logic][validation]{:target="_blank"} when saving a ticket with a discount code and adding some updates to their readme to [improve developer setup documentation][setup]{:target="_blank"} based on my experience setting up the app as a new contributor.

### What worked well

Honestly I thought the Konfera team did a great job setting up contributors for success.

They had a Gitter channel set up for the project that was always active whenever I checked in (although most conversation was in Slovak). I had a DM conversation (in English) with one of the project's primary maintainers, Richard Kellner, where I asked a few questions about the bug I had signed up to squash before getting started.

The team had set up a CONTRIBUTING.md with detailed instructions about how to get started, and had set up Travis jobs both for running tests and for linting code that would run on each branch. They even had the github Travis feedback plugin enabled, so developers could see when the build broke and get a link directly to the builds. If you're preparing your project for open source contributions, I think setting up automated linting that runs on build is a great way to ensure new developers adhere to your organization's style rules. Automated testing, along with good test coverage, is also a great way to make new developers feel comfortable to work in the code base without the fear that they're breaking a feature somewhere else.

Django-Konfera also had detailed UML diagrams included in their repository, which documented their object models and how they interacted. I found this to be the most helpful aspect of the developer setup: by looking at this diagram I could instantly see where I needed to make my changes.

After proposing my changes, I received feedback from one of the other project maintainers, Zoltan. My PR was merged less than 24 hours after I opened it. I think this is another thing the Konfera team did well: having active project maintainers is a great way to reduce code review cycle time and get changes merged!

### What Did I learn

Since I wasn't given write access to the Konfera repository directly, I needed to fork the app and branch from there in order to propose changes. This was a new process for me, as I have usually been given write access to repositories to which I have contributed in the past.

I also learned how to fetch and merge changes from the upstream repository into my fork. From a branch:

{% highlight shell %}
  git remote add upstream remote-url #To add the upstream remote
  git fetch upstream #gets the changes from upstream
  git rebase upstream/master
{% endhighlight %}

You could also just merge upstream/master's changes in to your fork's master, then rebase on to your master branch instead of upstream.

### Would I do it again?

Heck yeah! I had a great experience working with the Django-Konfera team. Even with a 9 hour time difference, the feedback came very quickly and my PR was merged within 24 hours of opening. Richard and Zoltan both said they were very appreciative of my contributions.

That's what's so cool about the open source community: some rando from of the internet can stumble upon a repository, propose changes, and make a project incrementally better from thousands of miles away. It's like digital good Samaritan-ism / volunteerism, and it feels great.

I'll definitely try to work open source in to my goals for the rest of the year and in to 2017. Digital Ocean is sending me a Hacktoberfest 2016 t-shirt in the mail in a few weeks for participating. I'll try to remember to post a picture back here when I get it.

[hack]: https://hacktoberfest.digitalocean.com/
[konfera]: https://github.com/pyconsk/django-konfera
[validation]: https://github.com/pyconsk/django-konfera/pull/154
[setup]: https://github.com/pyconsk/django-konfera/pull/153
