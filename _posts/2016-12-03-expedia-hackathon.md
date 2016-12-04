---
title:  "Hackathon"
date:   2016-12-03 14:50:00
category: post
tags: [hackathon, react, dependencies]
---

It's been about a month since I last posted, but I've been busy in the meantime! I've switched from the Frontend to the Backend team at work, and participated in the Q4 Expedia internal hackathon where we had 48 hours to come up with an MVP for an idea aligned with the theme "internal tools to make engineers lives easier."

### Pre-hackathon

We picked teams before the hackathon began, and started brainstorming an idea. While we could form teams and brainstorm before the start of the 'thon, we were not able to begin coding until the official kick off.

There were 6 of us on the team, each person with a unique set of skills. Our idea was to extend the Expedia engineering hub with a new function: the ability to search for sets of dependencies (like "React" and "Spring") and get a list of the Github repositories in the Expedia Engineering organization that used that set of dependencies.

We thought this functionality would be helpful for engineers looking to find others at Expedia who used certain dependencies in their applications, creating new points of communication between teams.

  - The tool should be able to search for both FrontEnd and BackEnd deps.

  - The tool should be able to search for sets of deps (repositories returned are an AND of the dependencies requested).

### Research and Resources

We thought it made sense to extend the Expedia Engineering Hub with our functionality, since it is already a place where engineers go to for information. This way our tool would be easy to find by others when it went to production. We also knew the developers who maintained the code base that backs the hub, and we knew that the hub was written using React+Redux which the FrontEnd developers on our team were already proficient with. This also meant there were a lot of UI components already built and tested that we could leverage as part of our app.

We also knew of an internal application that "sort of" accomplished what we were trying to build already... although it only allowed you to search for backend deps and it wasn't very user friendly (no autocomplete for searches). We thought we could try to leverage the data source for our search tool, which would mean we would only need to find a way to source frontend deps from the thousands of repos in our organization.

To source the frontend deps, we could build a scraper that made requests for the package.json files in Expedia repositories via the Github API. We would then store both the dependency list and the mapping between dependencies and projects in a database. Since package.json files are stored in (you guessed it) JSON format, they are exceedingly easy to parse through. Since we just cared about production dependencies (not test deps) we chose to ignore any `devDependencies` specified in package.json files.

We knew we were going to have to build some sort of API layer to act as intermediary between these different data sources and the front end. We chose Spring Boot since it was quick to configure and deploy to a test environment. During the hackathon, our backend team had mock API endpoints deployed to test within an hour of our team kick-off! Getting these set up ASAP was critical; the frontend could start integrating against mock data while the backend worked on connecting to the actual data sources.

### The 'Thon

Time flies when your hackathon-ing!

After the kick-off, we had a team meeting where we finalized the scope of the MVP, agreed upon an initial API contract between the frontend and backend, and divvied up tasks. We scheduled 30 minute check-ins every few hours to see how everyone was progressing with their points of focus: This helped surface areas where scope might need to be cut to meet the delivery deadline (this happened a few times during the project).

By day 2 of the hackathon we had full clarity on the scope of the MVP, and some team members began working on our slides for the presentation while others finished up the last chunks of the implementation work.

There were 250 teams participating in the hackathon across Expedia globally, 50 from the Chicago office alone. The top 10 from Chicago would proceed to the semi-finals, then the top 5 from there to the global finals where they would present their idea to senior technology leadership.

Since we wanted to be sure our presentation stood out from the crowd, we decided to start with a skit as an attention grabber: I was a "new engineer" with the company, who was confused on how to find others in the organization who were using the dependencies I was interested in. With the help of the tool we built, I could quickly determine who else in the company had expertise with my dependencies, and see some code examples for inspiration. It was a great way to leverage some of my acting chops from High School / College :)

### The Presentation

The skit was a hit! We blazed through the initial round, and easily passed through the semis. We were then tasked with creating a video of our presentation for the finals (2.5 minutes max) which would be shown to the Senior Technology Leadership for their feedback. We used a free program called [snagit][snagit]{:target="_blank"} which allowed us to both screen cap and record live video, then stitched our recorded segments together using iMovie. We had 1 hour to create and submit our video, and we finished with about 5 minutes to spare.

### The Results

While we didn't wind up placing in the finals, our idea garnered a lot of attention internally and we are planning to move it to production within the next month. And in the end, isn't that what matters? Code that is shipped is better than tech demos that will never see the light of day.

Here are some of my takeaways:

- I think it is easier to extend a currently existing product with new functionality than to start a new product from complete scratch for a hackathon. The primary engineers for the development hub were also thrilled to see that we incorporated our functionality in to their ecosystem.

- A smaller scope + a functioning demo is better than a moonshot with no demo to back it up, at least from a judging standpoint.

- An attention grabber for the presentation, something that helps your project stand out from the crowd, is critical.

- Snagit for video capture was exceedingly easy to use. No one on the team had any video making / editing experience, and Snagit allowed us to make a video from scratch within an hour.

- Hackathons are great teambuilding opportunities. It gives engineers a chance to take a break from their day jobs, work with some people they don't normally work with, and get exposure across the office (and in our case across the global Expedia Engineering Organization!).

I had an awesome time participating in our hackathon, and can't wait to get some time to polish the code we wrote (admittedly "hacky") to get our functionality in to production.

[snagit]: https://www.techsmith.com/snagit.html


