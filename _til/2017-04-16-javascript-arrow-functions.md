---
title:  "Arrow Functions and `this`"
date:   2017-04-16 08:58:00
category: til
tags: [javascript]
---

TIL how to leverage ES6 arrow functions to avoid losing "this" when writing functions inside of functions.

Arrow functions are more than just a syntactically shorter way of defining a function in javascript.

### Arrow Functions, and `this`

**Arrow functions do not bind a new `this` context like functions defined with the `function` keyword do.** 

When defining functions using the `function` keyword, the binding of a new `this` can lead to the loss of the intended `this` when using callbacks or inner functions. For example:

{% highlight javascript %}

const myObj = {
  name: 'Brian',
  salutation: 'Dr.',
  greet: function () {
    //the following logs "Brian", since `this` still refers to the current object
    console.log('outside of constructGreet: ' + this.name) 
    function constructGreeting () {
      // a new `this` is bound here, overwriting the parent object `this`
      //the following logs "hello undefined undefined"
      console.log(`hello ${this.salutation} ${this.name}`)
    }
    constructGreeting()
  }
}

myObj.greet()

{% endhighlight %}

In the `constructGreeting` method above, the `function` keyword binds a new `this` context accessible in its function body that overwrites the enclosing object’s context. This overwrite causes the enclosing object’s scope to be lost inside of the constructGreeting function body. In fact, **the `this` bound by the inner function will actually be the global object!**

### A Workaround to Losing `this`

Before arrow functions, a common workaround for this problem was to assign the outer function's `this` to a variable (often called `that` or `self`). The variable `that` is available for access in the inner function, and allows the inner function to access properties of the outer scope. 

{% highlight javascript %}

const myObj = {
  name: 'Brian',
  salutation: 'Dr.',
  greet: function () {
    const that = this // assigning the outer 'this' to 'that'
    function constructGreet () {
      // logs "hello Dr. Brian"
      console.log(`hello ${that.salutation} ${that.name}`)
    }
    constructGreet()
  }
}

myObj.greet()

{% endhighlight %} 

The inner function references outer scope properties from the closed over `that` variable.

### A Cleaner Solution with Arrow Functions

While the above workaround gets the job done, there is a cleaner solution if we use arrow functions. Since arrow functions **do not bind a new `this` context,** arrow functions can still access any parent object contexts by referencing the `this` keyword in their body. 

{% highlight javascript %}
const myObj = {
  name: 'Brian',
  salutation: 'Dr.',
  greet: function () {
    const constructGreet = () => {
      console.log(`hello ${this.salutation} ${this.name}`)
    }
    constructGreet()
  }
}
myObj.greet()

{% endhighlight %} 

The above code behaves exactly as you would expect: `this` inside of the `constructGreet` method refers to the containing object instead of a newly bound `this` context. Not only is the above code more consisce, but it is more readable and predictable. 

### Nuances of Arrow Function’s Handling of `this`

The fact that arrow functions do not bind their own `this` context has several important implications:

1) Arrow functions **cannot be used as object constructors** - Since arrow functions do not bind a `this` context, they cannot be invoked with the `new` keyword to return a new object context. In fact, trying to invoke an arrow function with the new keyword will throw a runtime error: `Function is not a constructor`.

2) Arrow functions **cannot be used as object methods** if those methods need to reference the containing object context `this`. For this reason, it is best practice to still declare object methods using the function keyword. To see an example of this issue in action, try changing the `greet` method in my examples above to be an arrow function instead of a using the `function` keyword... the object properties `this.name` and `this.salutation` are lost!

3) Arrow functions **cannot be passed new contexts using `call` or `apply`, and cannot be bound to new contexts using `bind`.** If an arrow function is invoked with `call` or `apply`, it will ignore any new `this` context passed to it, but it will still honor the function parameters passed. See the following example:

{% highlight javascript %}
// this is either 'window' in the browser
// or the node module context on the server
this.name = 'Brian'
this.salutation = 'Dr.'

const greet = () => {
  console.log(`hello ${this.salutation} ${this.name}`)
}

const newContext = {
  name: 'Lauren',
  salutation: 'Ms.'
}

// calling the method in 4 different ways
// They all log 'hello Dr. Brian'
greet()
greet.bind(newContext)()
greet.call(newContext)
greet.apply(newContext)

{% endhighlight %}

If greet were defined using the `function` keyword, the `bind`, `call` and `apply` calls above would have all bound the `newContext` as its new calling context, and would have logged 'hello Ms. Lauren'.

However, since greet was defined with an arrow function and arrow functions do not bind their own `this`, all 4 calls above log 'hello Dr. Brian'.

Click [here][here]{:target="_blank"} for the MDN documentation for Arrow Functions. It was helpful in cementing my understanding.

[here]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/Arrow_functions
