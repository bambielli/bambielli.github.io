---
title:  "AI and Rational Agents"
date:   2017-08-26 8:12:00
category: til
tags: [ai, artificial, intelligence, artificial-intelligence, machine-learning, computer-vision, knowledge-representation, robotics, natural-language-processing, automated-reasoning]
---

TIL about the 6 different branches of AI, and what a Rational Agent is.

I started my first class of the [Georgia Tech OMSCS][OMSCS]{:target="_blank"} program this week: Artificial Intelligence. While I took an AI class at Northwestern during my graduate studies, the GT class covers the full scope of my Northwestern class within the first 3 weeks.

The textbook for the class is [Artificial Intelligence: A Modern Approach][amzn]{:target="_blank"} by Norvig and Russell. Chapter 1 covered the history of Artificial Intelligence, as well as how the fields of mathematics, economics, and even linguistics have had their impact on the discipline.

## 6 Branches of AI

Chapter 1 also covered the 6 different branches of AI.

- **Natural language processing** - the ability for a computer to parse and produce language - *communication*
- **Knowledge Representation** - the ability for a computer to store and retain what it knows. It should also be able to fit new knowledge it acquires in to this storage - *memory*
- **Automated Reasoning** - The ability for a computer to use its acquired knowledge to answer questions and draw conclusions - *intelligence*
- **Machine Learning** - the ability for a computer to detect patterns in data, and adapt to new scenarios on its own - *learning / growing*
- **Computer Vision** - the ability for a computer to perceive objects in its surroundings - *perception*
- **Robotics** - The ability for a computer to manipulate objects, and move itself through space - *motor function*

A computer agent with mastery of all 6 branches could pass the [Total Turing Test][fullT]{:target="_blank"}, and would in theory be indistinguishable from a human agent.

## Rational Agents

Creating an AI that passes the Turing test is a noble goal, but is the ability to fool a human really that value-add?

Norvig and Russell suggest a more constructive approach: AI developers should strive to produce **rational agents** that, when presented with a problem space, pick the most optimal solution.

The difference here between **human performance** and **ideal performance** (often referred to as rationality) is something I never really considered. Economists would state that humans are inherently rational beings, at least in the context of an economic system, but humans don't always have enough computing power to make the most rational decision in all contexts. Humans also experience [decision fatigue][fatigue]{:target="_blank"} over time, something computers don't have issue with.

A comparison that the book authors made when justifying this rational agent approach, was to the quest for "Artificial Flight" in the early 20th century. True progress was made when the Wright brothers stopped trying to imitate birds, and instead focused on the underlying physics and aerodynamics of flight. Success was defined by a machine that could fly, not a "machine that [could] fly exactly like pigeons that [it could] fool other pigeons."

I'm looking forward to studying and implementing rational agents over the next semester!

[amzn]: https://www.amazon.com/Artificial-Intelligence-Modern-Approach-3rd/dp/0136042597/ref=sr_1_1?ie=UTF8&qid=1503112156&sr=8-1&keywords=peter+norvig
[omscs]: https://www.omscs.gatech.edu/
[fullT]: https://en.wikipedia.org/wiki/Turing_test#Total_Turing_test
[fatigue]: https://en.wikipedia.org/wiki/Decision_fatigue
