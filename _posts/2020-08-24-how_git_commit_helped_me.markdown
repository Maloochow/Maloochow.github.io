---
layout: post
title:      "How `git commit` helped me!"
date:       2020-08-24 21:07:41 +0000
permalink:  how_git_commit_helped_me
---


Git is deeeeeep. Although it's full potential can only be explored in a collaborative environment, a good understanding of it can definitely be helpful even with individual project.

In water-bland language, `git commit` is like `save` a doc file, with the addition of a name or `message`. 

This "small" addition is a game changer. It transfroms `git` from purely protecting your file to a project management tool and protect your entire project!

Imagine your project is to ship a lot of different vegetables from the U.S. to Country B. For the ease of yourself and others, you will want to categorize the vegetables into different containers (probably by their type). So when they arrive, you will know exactly what vegetables you delievered.

Similar in project planning, you need to think of what model, features, and views to create. What is the flow of the app? How to achieve the separation of concerns? All these questions require you to break down the project into smaller pieces during development. `git commit` is exactly a tool for that. By doing `git commit` with a message, it breaks down your project into smaller features. Use `git log` to track the history of your commits and see what features have you created. Do they fit into your plan? It is also a great tool to document your project. If you come back to it after a while and want to have a quick review of what it does, the history of commit can be a great help!

Now, vegetables from different sources do not come in clean containers. Different vegetablse are mixed together when they were transported to you. Just like humans are talented multi-tasking animals. Sometimes, you will work on several features together. How do you differentiate what changes belong to which feature? Where is the containers to put these different vegetables? Here is when `git add` becomes handy.

Use `git log` to see all the changes you have made (they will be organized by files) since last commit. They are like the mixed vegetables you have. And with `git add`, you can put some changes into the queue of commit. `git stash` and `git add -p` can be handy if you want to put only part of the changes in this commit! Walaaaaa, now you have a neatly grouped feature!

`git add` put vegetables into different containers, and `git commit` moved them onto the ship! When you are ready, let's hit `git push` to sail the ship! 🚢
