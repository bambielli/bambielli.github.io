---
title:  "No Private Class Members in Python"
date:   2017-10-22 15:41:00
category: til
tags: ["python", "private", "methods", "class", "member"]
---

TIL that Python does not have the ability to make class members (methods, data fields) private.

## Scenario

I've been building a small python class that calculates the [information gain][ig]{:target="_blank"} for a particular set of decisions, with respect to a random variable. This was inspired by a midterm for the Georgia Tech AI course I'm currently taking.

In this python class, I only wanted to expose one method as publically accessible through the API. There is a helper method that is a class member (it relies on class data) which I wanted to make private.

## It's all in the name?

I initially found a stack overflow post that noted making a method private in python is all in the name.

By prepending two underscores to the beginning of a class method name, the method becomes inaccessible on the class instance itself.

{% highlight python %}

class TestClass:
    def __init__(self, data):
        self.data = data

    def __privatemethod(self):
        print "is it private?"

    def publicmethod(self):
        print "definitely public"

tc = TestClass([1,2,3])
tc.publicmethod() # prints "definitely public"
tc.__privatemethod() # throws AttributeError: TestClass instance has no attribute '__privatemethod'

{% endhighlight %}

The thrown `AttributeError` makes it appear at first glance that the `__privatemethod` method is inaccessible off of the class instance.

**Unfortunately, though, that's not the case.**

## No Private Class Members in Python

I learned from reading the [python class docs][docs]{:target="_blank"} that **python doesn't actually support making class members private.**

Prepending double underscore to the beginning of a class name is a convention used to indicate the method should be regarded as private, and is not meant to be used outside of the class context.

The double underscore **mangles the class name** by additionally prepending `_ClassName` to the member name. This makes it *more difficult* to access a "private" member of the class, *but not impossible*.

{% highlight python %}

class TestClass:
   """
   Same as above
   ...
   """

tc = TestClass([1,2,3])
tc.publicmethod() # prints "definitely public"
tc._TestClass__privatemethod() # prints "is it private?"

{% endhighlight %}

Therefore, **it is actually impossible to make class members private in python.** The mangling convention makes it more difficult to access members that should be considered private, but they can still be accessed by persistent developers.
