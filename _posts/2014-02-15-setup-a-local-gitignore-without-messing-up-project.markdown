---
layout: post
title: "Setup A Local Gitignore Without Messing Up Project"
date: 2014-02-15 07:51:16 +0530
comments: true
categories: 
---

A lot of us use Git for Version Control. If you have used git, you must be aware of .gitignore files. It contains the list of all file and directories you want git to ignore so that it would nevet get committed. But if you work in multi person team. You might not have liberty to change the projects gitignore. What to do then?

Well, the solution is pretty simple. Say you are using any IDE like Pycharm or Netbeans. They always create some files to create track of your projects. Something like `.ropeproject`

So in order to make git ignore it, Go to : `cd .git/info/`

You see there is a file called exclude. Fill it up with the files and directories you want it to ignore, just like the gist below:

{% gist 7597812 %}

And it is done. Now you never have to worry about your editor messing up your commits.

