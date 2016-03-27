---
layout: post
title:  "50 Minute Crash Course in Web-Development"
date:   2016-03-10 22:16:00
category: general
tags: [teaching, web-development]
---

There were only 3 days of class this week at school (students got Thursday and Friday off for Easter) so instead of forging ahead with  new course material before the break, I decided to put together a lesson on Web Development. ***The goal was to have students create an HTML page, with a bit of CSS and JavaScript, in a 50 minute class period, with no assumptions of prior knowledge of any of these web languages.*** Read on to see the lesson plan, and some of the thoughts I had around planning for the day.

### Planning

  1. Students can either bring their own laptops in from home for class, or use the school provided Macbook Airs. Some students use PCs, others have Macbooks. To reduce the number of variables during the activity, I decided to have all students grab one of the school provided Macbooks for the class period.

  2. I wasn't sure what type of text editors were installed on the school provided Macbooks, so I decided I needed to teach the students about `vim` which I know comes standard with the mac bash terminal.

  3. In order to get all of the students in vim, though, this meant I needed to do a bit of a crash course in using the `terminal`. I built my lesson under the assumption that no students had actually ever used the terminal before, which turned out to be a correct assumption.

  4. Class is only 50 minutes long, so I was going to have to really fly through the material...I sent the lesson plan out to my co-teacher prior to class, so he could understand the goals of the lesson before we started. This way, if any students ran in to issues, he could help debug while I continued on with the material.

  5. I wanted to motivate building the HTML page with a short lesson on how the web works (high level). I thought this would be helpful for students to understand where the HTML pages live after they are created, and how they make it to the browser. I created some slides with pictures to help with this discussion. I also had students open up Chrome, go to google.com, and use the developer tools to inspect the served html. I deleted the google img tag from the page, then refreshed the page and it reappeared: this helped students understand that the html page they receive in their browser is just a copy of what is on the google servers.

Find notes / steps for my lesson below.

### The lesson

#### How Does the Web Work?

Brief discussion on servers and how URLs are mapped to server addresses (requesting the resource available there).

  - Inspect google.com, show what the HTML page looks like
  - Delete elements off of the page
  - Change text on the page
  - Refresh the page, show that our modifications disappear

We didn’t actually change the page on Google’s SERVERS, we just changed the copy of the page that the server shipped to us.

#### The Terminal

Brief intro on what the terminal is, how to access it, and what it is used for.

**type:** `cd ~`

  - *cd* stands for “change directory”. Changing between folders in your computer’s file system.
  - *~* is your “home” directory

**type:** `ls`

  - looks at the files in the current directory.

**type:** `mkdir your_name_web`

  - *mkdir* stands for “make directory”. It creates a new folder at the current level.

**type:** `ls`

**Check:** you can see the new directory when you typed ls.

***Practice:*** Change directories in to the directory you just made

#### Using Vim

**type:** `vim index.html`

  - `vim` is a very powerful text editor in your terminal.
  - The above command will put you in to a file called `index.html` in your current working directory (your_name_web).
  - `vim` doesn’t allow you to type like a normal text editor does… It has two modes: navigation mode, and insertion mode.

**type:** `i`

  - Switches in to insertion mode

***Check:*** You should now be able to type in the file.

#### Creating an HTML Page

Talk about the high level anatomy of an HTML page.

  - **create:** `<html></html>` outer tags
  - **create:** `<head></head>` tags
  - **create:** `<body></body>` tags

Talk about structure of html tags (openining and closing a tag). What gets nested inside of what. Spacing properly is important for readability

  - **create:** `<h1></h1>` in body - "hello world" inside of the tags
  - **create:** `<title></title>` tag in head - "My Static Page” inside of the tags
  - **create:** `<a></a>` tag in body - `href` attribute to school website

#### Back to Vim

Need to save the file. You can’t just hit cmd+s like normal

**type:** `esc` key

  - Exits insertion mode

**type:** `:wq` + `enter`

  - This tells `vim` to save and exit the file

#### Open the Page

**type:** `ls`

  - confirm that your new file is there

**type:** `open index.html`

  - opens the file in your default browser

***practice:*** Have them confirm that the link works, and use the inspector to inspect their web page.

#### Adding Some Style With CSS

Page is a bit boring…lets add a bit of style to the page using CSS

**setup:** Go back in to the index.html file with vim. Switch back in to insertion mode.

**create:** `<style></style>` tags inside of `<head>`

  - turn the h1 red, center it
  - make link have px of 50

{%highlight css%}
  h1 {
    color: red;
    text-align: center;
  }
  a {
    font-size: 50px;
  }
{%endhighlight%}

***check:*** Save the file, and refresh the page in the browser

***practice:*** Create a new h2 tag that is centered below the header of your page. Make it have a text value of 0 (put 0 inside of the tags).
Center it, make it yellow.

#### Adding some Javascript

Browsers read and understand javascript, just like Eclipse understands Java.

**setup:** Give your h2 element an id attribute of “counter"

**create:** `<script></script>` tags inside of `<head>`

  - inside here we will be writing javascript. Three different languages in one file... pretty cool!

**Type the following in to the script tags:**
{%highlight javascript%}
      var count = 0;

      var increment = function() {

          count++;

          document.getElementById('counter').innerHTML = count;

      }
{%endhighlight%}

Give your h2 an onclick attribute of `increment()`

***check:*** Save the file, refresh the page, and click on the h2! The h2 should increment each time you click on it by 1.

#### Additional Resources

For anyone who wants to continue learning about web development languages:

codecademy —> javascript or html/css tracks

### Wrap up

We were able to cover all of the material in the 50-minute class period, and every student reached the end goal. Feedback on the lesson was very positive, so I think we will be doing more Web Development type work after the AP exam is over in May.

If anyone has additional questions about this lesson, feel free to shoot me an email!


