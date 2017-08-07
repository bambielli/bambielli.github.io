---
title:  "process vs. thread"
date:   2017-06-25 12:20:00
category: til
tags: [systems, operating-systems]
---

TIL the difference between a process and a thread.

## What is a Process?

A `process` is an executing instance of a program on a computer. It is allocated its own resources on the computer, including its own virtual address space, process id, and unique configuration like environment variables.

A process encompasses both the program that is executing, and the state of all the threads of execution within the program.

Each process starts with a single thread of execution, often referred to as the "main thread".

## A Process as a Restaurant

Think of it like a dinner service at a restaurant: when employees arrive at the restaurant and the doors open, that is analogous to a program being loaded in to memory and coming to life via its main execution context in a process.

This restaurant's resources are isolated from the resources of all other restaurants in the area: each restaurant has its own food, its own kitchen, and its own dining room. Similarly, a process's resources on a computer are isolated from the resources allocated to other processes.

## Show me a Thread!

A `thread` represents a single path of execution within a process. Multiple threads can be created as siblings of the process's main thread (multithreading). Threads exist in the context of a process, and therefore share the same resources allocated to the process. This implies that threads can affect each other via the consumption / modification of these shared resources.

Threads execute independently of one another, in parallel. Each has its own execution context, where data structures necessary for execution of the thread are allocated. The parallelism afforded by independent thread execution can be used to speed up a process, by fully utilizing the resources allocated to the process by the operating system.

## Multithreading Challenges

The parallel execution of a program in a process via multithreading has its own challenges, though. Since threads share the same resource space as the parent process, *if threads cause side effects in this shared resource space* (via writes to memory that other processes can see) this can lead to *race conditions* or *deadlocks* between threads. Also, if one thread unexpectedly exits or throws an exception, this can crash the entire process. A developer needs to take these challenges of thread `synchronization` in to account when considering ways to split a process in to multiple threads.

## A Single vs. Multithreaded Restaurant

First, consider a restaurant that is owned and operated by a single employee: this employee acts as the server, the chef, the dishwasher, the cashier... everything necessary to "process" a customer and get them their order!

**This employee is analogous to the main thread of a process.** The "program" of serving a customer will execute serially in this case: this single employee must first take the customer order, next cook the correct food, then serve the customer and finally accept the payment.

This method of operating a restaurant **does not take full advantage of the resources available to it** (i.e. there is an enormous kitchen with space for 3 chefs, and there are 2 cash registers for accepting payment).

If our restaurant gets popular, and we get more than 1 customer at a time, a single employee (thread) will take so long to complete the full process that we will surely start to lose customers.

Let's try a multithreading approach to speed up the execution of this process!

The initial employee hires 5 chefs, 2 cashiers, 3 servers, and 2 dishwashers. Each employee type represents a thread of execution of part of the restaurant process.

Now we can start executing in parallel: when a customer enters the restaurant, they are greeted by a server and their order is taken, one of the available chefs takes the order ticket and starts preparing the food, the server brings the cooked food back to the customer, and the customer pays at the cashier when they're ready to leave.

Each employee has a unique **thread of execution** that they care about, and as the shared resource space of the process is filled with tasks pertinent to their thread, they can perform the work necessary to complete that task in parallel with their co-workers. This means that when a second customer comes in, the server can greet, seat, and take the order of this customer, without needing to worry about the food preparation "thread" of execution for the first customer's order.

