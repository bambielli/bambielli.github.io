---
title:  "Neo4j pt. 2: Data Modeling Class"
date:   2016-03-25 14:48:00
category: post
tags: [neo4j, graph-databases, data-modeling]
---

The Syndio development team got to take another *all expenses paid* (thanks to the awesome Neo team!) class on modeling your data in a graph database today! The class was pretty insightful, taught by long time member of the neo team [Max de Marzi][max]{:target="_blank"}. It put to rest a lot of potential fears that we had about making the transition from Relational DB to Graph. Here are some of the takeaways:

### Our Scale is (Comparatively) Small

A concern that is constantly brought up by users of graph databases is that of memory usage: in order for the graph queries to be as performant as they are, the entire graph needs to be loaded in to memory. Because of this, you don't want to get your model wrong. Any excess data, or unnecessary data stored will just be bloat and slow down your queries.

After speaking with Max about our scale (up to 100k nodes and generally under 5million edges) he told us we had nothing to worry about. The memory concerns most people have are for graphs of millions, even billions of nodes and edges, and this is where Neo can slow down... with version 3.0 max noted that they will be offering support for almost half a TRILLION edges to be loaded at once, and the queries will still remain relatively performant. Our scale should give us some leeway to make mistakes in our models without suffering from performance hits.

### Modeling Starts with A Question

Modeling data in a graph schema is very fluid: e.g. there is no hard coded set of model definitions and nodes / edges can have properties or labels that others of the same type do not. Since there isn't really any "best practice" for setting up a graph model, the starting point to figure out how best to model your data should **start with the question you are trying to ask.** After that, you can optimize your model to give you back answers for that particular question as fast as possible. This is a different modeling paradigm that I think will take some time to get used to...

### Where to Start?

I think a good place for us to start is by loading some of the "tag" data we are going to begin showing to customers in to a graph DB like Neo4j as a proof of concept. We are in the process of researching a few new metrics we would like to show to customers, so I think experimenting with Neo4j fits well in to the R&D theme.

[max]: http://maxdemarzi.com/

