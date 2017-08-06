---
title:  "node: require.cache"
date:   2017-04-30 12:01:00
category: til
tags: [node, javascript]
---

TIL that node's `require` method maintains a cache of the modules that have been imported. I also learned that it is possible to invalidate this cache.

Per the [node documentation,][node]{:target="_blank"} modules are cached after the first time they are loaded (loaded is synonymous with 'required'). They are placed in the `require.cache`. This means that every future `require` for a previously loaded module throughout a program will load **the same object** that was loaded by the first require.

{% highlight javascript %}
// data.json

{
    "name": "Brian"
}

// moduleA.js
const data1 = require('data.json')

const data2 = require('data.json')

data1 === data2 // returns true

{% endhighlight %}

Since `data1 === data2` returns true, this indicates that data1 and data2 are not new copies of the same module, but are themselves **the same value** (same object reference). Try executing the example above in a node REPL to confirm!

This also implies that code defined in a required module is only executed one time (during the first require) since all the following requires to that module will just resolve the object from the cache. If there is some code you want to be executed on each re-require of the module, **you should export a function instead,** and manually invoke that function in each relevant location.

Another notable caveat: `require` **caches modules by file path**. This means, if you are working on a machine with a case-insensitive filesystem (like the default OSX) where the same file can be referenced with a combination of upper and lower case letters, **require will cache modules referenced with different cases as different modules.** See the below example:

{% highlight javascript %}
// data.json

{
    "name": "Brian"
}

// moduleA.js

// all 4 of the following data objects resolve to the same data.json file above
const data1 = require('./data.json')

const data2 = require('./Data.json')

const data3 = require('./dAtA.json')

const data4 = require('./DATA.json')

// however they are treated as different modules by require!
data1 === data2 // returns false

data2 === data3 // returns false

data3 === data4 // returns false

{% endhighlight %}

Any comparisons of the above data objects will return `false`, since `require` is treating each as a different module and is caching each at a different key in the `require.cache`.

*Side Note: TIL that the OSX filesystem defaults to resolving files in this case-insensitive manner: `data.json` and `DATA.json` resolve the same file!*

## Examining the `require.cache`

`require` caches modules that have already been loaded in an object found at `require.cache`. The keys of this object are the **file paths at which previously resolved modules were located**: this is the reason for the case-insensitivity caveat that I highlighted in the last section.

Before any modules are required, the `require.cache` is just an empty object. Here is a look at the `require.cache` after a single import of `data.json`:

{% highlight javascript %}

{
  '/Users/username/data.json': //file-path key
      Module {
          id: '/Users/username/data.json',
          exports: { name: 'Brian' },
          parent:
              Module {
                  id: '<repl>',
                  exports: {},
                  parent: undefined,
                  filename: null,
                  loaded: false,
                  children: [Object],
                  paths: [Object]
              },
          filename: '/Users/username/data.json',
          loaded: true,
          children: [],
          paths: [
              '/Users/username/node_modules',
              '/Users/node_modules',
              '/node_modules'
          ]
      }
}

{% endhighlight %}

You can see from the output that the top level key of the require.cache object is the path `/Users/username/data.json`, which is the absolute path in my filesystem of the data.json file. This further illustrates how case-insensitive filenames can look like different modules to require: they will all have different keys in the require.cache object!

You'll also notice that the value of the key is itself a node `Module` object, which contains references to the parent module (in this case, the node REPL), any child modules of the imported module, if the module is 'loaded', and the values that were imported from the file (under `module.exports`).

## A Real-Life Example

I started researching this topic after grading some of my students' homework from the prior week: the homework involved creating an express app with GET and POST routes for a "friends" entity.

Students were encouraged to include some seed data for the app in a JSON file. This data would be returned from initial requests to the GET route. The POST route would add new "friend" data to the app: in other words, the POSTed data was expected to "persist" and was expected to be returned from future GET requests.

I assumed students would just `require('./seedData.json')` and push items to the data array once it had been brought in to memory. One of the students implemented what I considered to be 'the long road', and used `fs.readFile` to read the JSON file after each GET request to the server.

Their POST route used `fs.writeFile` to write the JSON data back to the seed data file. I thought we might be able to replace the `readFile` in the get request with a re-require of the module, **but due to the way require caches modules after initial load, any changes written to the module using `fs.writeFile` in the POST route would not be reflected!**

I thought there had to be a better way to read "changing" data from a JSON file without using `fs` APIs.

## Invalidating the cache

It is possible to invalidate the `require.cache` for a particular module: just delete the key from the `require.cache`!

{% highlight javascript %}

delete require.cache['/Users/username/data.json']

{% endhighlight %}

If a module isn't in the cache, require will load the module again from disk. In the homework example, this would allow any changes written to the .json file to be reflected in future GET requests: we would just need to invalidate the cache after each write to the file, and require the module in each GET. This would be more performant than reading the file from disk each time, since re-requires of the same module will just resolve the module from the require cache.

One caveat to invalidating the cache: sibling modules in your node app can get **out of sync** with the most recent state of the module on disk! For example, if module A and module B both require data.json, but module A invalidates data.json from the cache and re-requires it, module B must also re-require data.json to stay in sync. This is somewhat risky, and puts more responsibility on the developer to remember this caveat.


[node]: https://nodejs.org/api/modules.html#modules_caching
