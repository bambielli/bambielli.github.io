---
title:  "Read By Refactoring - Part 2: Naming as a Process"
date:   2017-02-26 20:57:00
category: post
tags: [code-review, legacy-code]
---

This is part 2 in my Read By Refactoring series. In this post, I'll discuss what our instructor described as "Naming as a Process", which highlights a way to extract domain knowledge out of the code and in to human readable form through the names of variables and methods.

[Find Read by Refactoring Part 1 here.][rbr]{:target="_blank"}

## Naming as a Process

Most of read by refactoring is focused on naming: clearer names lead to more readable code. There is a suggested process for naming extracted chunks of code that aims to highlight any latent domain knowledge in a code block. The suggested steps for naming an extracted block of code are as follows:

1. **Nothing** - You begin with nothing. There are no useful method names in the code... just a series of calls to other methods from constructed objects, conditionals, and a bunch of poorly named variables... At this point the code is in its least readable state, but you do have a hunch that certain blocks could be grouped together in some way and described more semantically.

2. **Nonsense** - You follow your hunch and extract a code block without knowing much about the ultimate function of the block in the program... At this point it doesn't make sense to waste time trying to think of a specific or relevant name, since the name is likely to change as you continue to develop understanding. You name the newly extracted method `applesauce` and move on.

3. **Honest** - You begin to take a closer look at `applesauce` and realize that it is doing quite a few things: It seems to be operating on some object that contains user preferences, and makes a call to a payments service with user information. You update the method name to `GetUserPreferencsAndSendToPaymentService` since that is about as honest as you can be when figuring out what the heck is going on.

4. **Honest and Complete** - Upon further investigation, you realize the code is not just operating on the UserPreferences object, but also making a call to a payments service by constructing an intermediary Payments object, **and** it is writing to a database that contains invoices. You rename the function again to: `GetUserPreferencesAndConstructPaymentsObjectAndSendToPaymentServiceAndWriteToInvoiceDB` since this is the complete description of functionality in this code block.

5. **Domain Knowledge** - Whew that is one long method name! At this point, the complete description of the name is indicative that the code block is *doing too much*. A rule of thumb: methods should have a single responsibility, and the use of the word "And" in a complete method description is a red flag that the method should be broken up. A suggestion at this point is to break up the larger method along the "And" boundaries in its name, which will delagate one unique concern to each method. These method descriptions get you closer to latent domain knowledge stored within the original code: in this case, the sequence of steps to construct and write payments to the invoice DB!

## Conclusion

At this point, you haven't written any new code... You've just taken spaghetti code that previously existed, and have refactored it in a way that makes it more readable and makes the underlying domain knowledge more apparent to the reader.

Not only did it help you get a better understanding of a particular code block, but **it will also prevent the next poor soul who needs to make a change in this block from going through the same process of understanding that you just went through.**

You've effectively reduced the amount of time to comprehend the code for the next developer who passes through, which is unarguably valuable.

[rbr]: /posts/2016-08-21-read-by-refactoring/
