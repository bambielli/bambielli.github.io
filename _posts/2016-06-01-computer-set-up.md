---
title:  "Configuring the Terminal on a New Macbook"
date:   2016-06-01 21:12:00
category: post
tags: [efficiency, osx, shell, sublime]
---

I bought a new Macbook Pro Retina today for personal use, and spent most of the day re-configuring the terminal and development environment to match my old machine. This post is both a reference for my future self (to save some time with future environment configuration) and a set of recommendations for anyone looking for some time saving tricks.

## [Changing `ls` Colors][ls]{:target="_blank"}

The default colors displayed in the output of the `ls` command in the terminal don't make any differentiation between folders or file types. The guide above highlights a few environment variables to set in your `~/.bash_profile` that allow for customization of the colors of `ls` output. This is exceedingly helpful when trying to navigate foreign folder structures, or trying to find that one executable in a sea of non executables.

The variables are as follows:

{% highlight shell %}

  export CLICOLOR=1 # tells the shell to apply coloring to command output (just needs to be set)
  export LSCOLORS=exfxcxdxbxegedabagacad # configuration of colors for particular file types
  # NOTE: see the article linked above for detail on LSCOLORS values

{% endhighlight %}

## [Sublime Text Symlink][subl]{:target="_blank"}

My text editor of preference is Sublime, so the article linked above is applicable for creating a symlink for its CLI and adding it to your path. This is convenient since it allows you to open a file (or folder) in your text editor of choice ***from the command line***, instead of having to open files manually from the editor itself. Once you create the symlink, make sure you add the location of the link to your path and put it in your `~/.bash_profile`.

{% highlight shell %}

  export PATH=$PATH:~/.bin # augments PATH with location of sublime symlink

{% endhighlight %}

## Additional Sublime Configuration

On the topic of Sublime, I recommend enabling the following options in `Preferences > Settings - User`:

{%highlight json%}

{
	"trim_trailing_white_space_on_save": true,
	"ensure_newline_at_eof_on_save": true,
	"scroll_past_end": true
}

{%endhighlight%}

`trim_trailing_white_space_on_save` will do just that, making your diffs in github easier to parse through.

`ensure_newline_at_eof_on_save` is also self explanatory, and keeps your files consistent with the expectation of many unix based text editing utilities that non-empty files have a newline `\n` at the end. Github also tracks this in files, so having it enabled ensures all of your files remain consistent and do not show up in diffs because newlines were removed.

`scroll_past_end` lets the editor scroll further down than the last line of the file. This is useful to bring text at the end of a file higher up in the editor, without needing to add a bunch of unnecessary blank lines.

## [Current Git Branch in Your PS1][gbps1]{:target="_blank"}

This one blew my mind when I enabled it, and also taught me how to configure the "Prompt String 1" (PS1) that displays by default in the terminal. Just follow the instructions in the post above, which involves adding a shell function to your `~/.bash_profile` that checks for the presence of a git repository and parses the text of the current branch. If a branch was found, the name of the branch will be added to your `PS1`, right before your cursor! Knowing what branch you're on is critical when working on a complex project with multiple branches, preventing inadvertent work in / commits to the wrong branch.

## [Tab Completion of Git Branches][tab]{:target="_blank"}

Another trick that saves me a lot of time is tab completion of git branches. Without this, you are forced to type out the full branch name to which you are switching, which slows you down. See the SO post linked above for details on enabling this. It involves downloading a script that performs the tab completion logic (credit to Shawn O. Pearce <spearce@spearce.org>) and sourcing it in your `~/.bash_profile`. In all honestly I haven't looked through the script in much detail... but works like a charm!

Let me know in the comments if you have any additional time saving tricks that you think I've missed. I'm always interested in improving my personal development process.

[ls]: http://osxdaily.com/2012/02/21/add-color-to-the-terminal-in-mac-os-x/
[subl]: http://stackoverflow.com/questions/16199581/opening-sublime-text-on-command-line-as-subl-on-mac-os#answer-16495202
[gbps1]: https://coderwall.com/p/fasnya/add-git-branch-name-to-bash-prompt
[tab]: http://apple.stackexchange.com/a/55886
