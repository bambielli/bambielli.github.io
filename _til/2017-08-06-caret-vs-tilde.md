---
title:  "Defining Dependency Versions in package.json"
date:   2017-08-06 13:22:00
category: til
tags: [javascript, dependencies]
---

TIL how to properly define dependency versions in a `pacakge.json` file.

`package.json` is used to define and configure the frontend dependencies for an application. It is also used to store npm scripts and configuration that is useful during development.

When you install a package using `npm install` or `yarn add`, the dependency version is, by default, listed with a caret in front of the version number. Just accepting the caret version for packages had served me well in the past, until we ran in to some dependency version clashes due to auto-updating dependencies.

I figured it was about time to learn what that caret stands for, and what the various options are for defining dependency versions in package.json.

## caret vs tilde

When you install a package using `npm install`, by default the dependency version is written to your package.json file with a caret prepended to the version number.

For example:
```
{
    "dependencies": {
        "react": "^0.15.3"
    }
}
```

The caret indicates that the **minor version of a dependency can be auto updated** the next time npm install is run for the project.

In the example above, the caret in front of the react version number indicates that react can be automatically updated to version 0.16.0 when it is released. A major release to 1.0.0, though, would not cause an auto-update.

You can specify dependency versions with a tilde instead of a caret.

```
{
    "dependencies": {
        "react": "~0.15.3"
    }
}
```

A tilde indicates that the **patch version of a dependency can be auto updated.**

In the example above, the tilde in front of the react version number indicates that react can be automatically updated to version 0.15.4 when it is released. A minor release to 0.16.0 or a major release to 1.0.0 would not cause an auto-update.

You may also define your dependencies without a caret or tilde in front of the version number. This will keep your project locked on that dependency version number until you explicitly decide to upgrade.

Refer to [my post on semantic versioning][sem] for details about what major, minor, and patch version increments indicate to consumers.

## What method should you use to define your dependencies?

If I've learned one lesson during my years of developing software, it's to **always be skeptical of your dependencies.**

Dependencies, by definition, are pieces of your system that are out of your control. You can't always assume that your dependency authors are putting in the same level of rigor around package updates that your team would.

Minor and patch versions *shouldn’t* contain changes that would break your application, but you can't always make that assumption...

For this reason, if you're working for a large tech organization that is risk averse and supports products that have hundreds or thousands of daily users, it probably makes sense to **remove all carets and tildes from your package versions** and to stay explicitly locked on version that you are certain behave well.

Your team can decide during a sprint to do package updates. This will allow you to thoroughly review patch notes and confirm proper application behavior before new versions go live to prod.

[sem]: /til/2017-07-03-semantic-versioning/

