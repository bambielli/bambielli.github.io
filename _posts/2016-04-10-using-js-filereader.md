---
layout: post
title:  "Using JS FileReader"
date:   2016-04-10 09:46:00
category: til
tags: [javascript]
---

TIL how to use the javascript FileReader to read data from a text file.

I participated in the qualification round of the [Google Code Jam][gcj]{:target="_blank"} over the weekend. Since I have been working with javascript a lot lately, I figured I'd write my solutions in JS. One thing I didn't consider, though, was that I had no idea how to read in text from a file using JS...

### Javascript's FileReader

Enter the javascript [`FileReader`][FileReader]{:target="_blank"}: this object exposes a set of methods that allows the client to interact with a file. Reading happens asynchronously, and triggers a FileReader.onload event when it has compelted. Any methods you want to interact with the contents of a file that has been read should therefore be included in the onload callback.

FileReader is a webAPI, and was not automatically accessible in my nodeJS runtime. I installed two NPM packages that implemented the APIs: [`filereader`][fr]{:target="_blank"} and [`file-api`][fapi]{:target="_blank"}. These allowed me to create both FileReader objects and File objects (which the reader expects as input to its readAsText method).

See below for the implementation I used to read from textFiles in my codeJam solutions.

{%highlight javascript%}

//exposing the FileReader webAPI
var FileReader = require('filereader');
var fapi = require('file-api');
var reader = new FileReader();
var File = fapi.File;

reader.onload = function(e) {
	var result = reader.result
	//do what you will with the result...
}

reader.readAsText(new File('someTextFile.txt'));

{%endhighlight%}

I create an instance of the FileReader object called `reader`, then attach a callback to the reader's `onLoad()` event which will fire asynchronously whenver it finishes reading input from a file.

I use the reader.readAsText method to read in the file, which attaches a string representation of the textfile to 'result' when it has completed reading. `readAsText` expects a `File` object, which was implemented by the `file-api` package I installed.

I had never used the FileReader before, so this was good practice. It seems like it isn't as robust as a server side fileReader like python's file library, which can parse through a file line by line while keeping it open. You also don't appear to be able to **write** to files using javascript's FileReader object (I guess you can tell by the name?). I wasn't able to figure out how to do this quickly, so I just wound up copying and pasting the output of my programs in to output.txt files for submission to the contest.

***NOTE:*** Upon further examination of the types of objects that both of the above NPM packages implement, it looks like I could have gotten away with just requiring `file-api`, since it also implements `FileReader`. It also looks like it might be possible to write to text files on the client according to this [SO][SO]{:target="_blank"} post

[gcj]: https://code.google.com/codejam
[FileReader]: https://developer.mozilla.org/en-US/docs/Web/API/FileReader
[fr]: https://www.npmjs.com/package/filereader
[fapi]: https://www.npmjs.com/package/file-api
[SO]: http://stackoverflow.com/questions/21012580/is-it-possible-to-write-data-to-file-using-only-javascript

