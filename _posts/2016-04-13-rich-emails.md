---
layout: post
title:  "HTML5 Email Templates..."
date:   2016-04-13 08:03:00
category: til
tags: [html5]
---

TIL how difficult it is to create emails with rich content...inline CSS and email client incompatibilities for days!

The emails we send in our app are often confused for spam. They are plaintext, and have a really ugly URL at the bottom that allows users to authenticate with our system via an email without needing to actually create an account with our system. During a recent survey launch, we got many questions in our support inbox from users who were questioning whether or not we were phishing or spamming them. This could affect survey response rates, if enough users don't believe that the email is legitimate!

We finally took the plunge to develop some rich content (HTML5) email templates. We had our new director of marketing draft up what the templates should look like, and then we began the translation in to our app. This is when we realized just how much of a PITA making rich email templates is.

###Inline CSS

Apparently you are not allowed to have CSS separate from your HTML tags in emails. It all needs to be inlined in each tag. Not sure who designed this spec, but it is gnarly from a maintainability standpoint. We thought about just getting the email to look nice with CSS separated, inlining each style rule to each tag manually, and then storing this in our repository, but that is an absolute nightmare from a maintainability standpoint.

We wound up finding an NPM package that does CSS inlining for you, and created a grunt task that runs this compilation whenever a new dyno is spun up on heroku. That way, the inlined template is created programmatically, and we can store the more maintainable "CSS separated" template in our repository.

###Email Client Inconsistencies

Each email client introduces its own set of constraints on what actually gets rendered from your CSS. Some clients don't support padding, some don't support margins, it needs to be responsive on mobile clients...basically your template will NOT look consistent if you make it look good in chrome on desktop and do not check other potential email clients. Since we are generally dealing with enterprise, we need to be certain that our email templates will look good in all potential versions of Outlook.

We found a tool that sends your email to a bunch of different clients, and takes screenshots of what it looks like in each, then lines them all up side by side so you can easily visualize it. Without this tool, we would have had to send emails every time we made a change to our template to each email client separately, and we would have had to purchase a license for Outlook for each version we wanted to test.

It took a lot longer than we originally anticipated to get these email templates working properly, but now that they are we are hoping it legitimizes us in the eyes of our clients even more.



