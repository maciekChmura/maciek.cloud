---
title: How I stay productive in a big project
date: '2018-12-18T22:12:03.284Z'
template: 'post'
draft: false
url: '/how-I-stay-productive-in-a-big-project'
category: 'tips'
tags:
  - 'productivity'
description: 'How I stay productive in a big JavaScript project... and not get lost on the way.'
---

...and not get lost on the way.

## Problem

For almost a year I've been a part of a big and mature JavaScript project. No frameworks. Just Node, JS and MVC. Often times when I am fixing a bug I have to jump into multiple files and classes for investigation. My open files tab gets full really quickly. My main problem is jumping between different solutions for a particular fix.

I want to perform some change in code, test it and leave it for later to go find a different approach. I repeat these steps a few times. Then, when I have a fix that is the best fit, in my opinion, I can make a PR for code review or discuss it with my team.

So ideally I would like to quickly switch between possible fixes.
For this, I have two approaches.

## Save a diff file

```bash
git diff > fix1.diff
```

Git will generate a patch file with all the changes made to the repository. I can send this file to someone, open it in its own window to compare with the current state, etc.
Very easy for quick verification.

To apply this file:

```bash
git apply fix1.diff
```

This is the simplest workflow for progressively save your work between commits.
I have just a file with all of the changes.
This is nice and simple, but there are better solutions.

## Git Stash

Stashing is saving work for later.
There are many great tutorials and documentation on this topic.
[atlassian](https://pl.atlassian.com/git/tutorials/saving-changes/git-stash)
[git-scm](https://git-scm.com/docs/git-stash)

I found this 2 commands helpful in my case:

```bash
git stash push <message>
git stash apply
```

`git stash save` will save the changes and clean my working directory, soo to continue working I must apply them back. (`git stash pop` will also apply changes but they will be deleted from stash).

Now I have a saved point in working "timeline" that I can easily evaluate or revert back to.
This can also be done inside VScode(if you use it) with [Gitlens](https://marketplace.visualstudio.com/items?itemName=eamodio.gitlens) plugin(Webstorm also has this functionality)

## Micro-tip: personalized comments

I put a comment in this manner:

```js
// @mch <what I think is happening here>
```

_mch > my initials_

Within editor, I've set up a rule to highlight `@mch` string.
For VScode there is nice plugin: [TODO](https://marketplace.visualstudio.com/items?itemName=wayou.vscode-todo-highlight)

I've customized it with:

```js
"todohighlight.keywords": [
    {
      "text": "TODO",
      "color": "#000000",
      "backgroundColor": "gold",
      "borderRadius": "2px",
    },
    {
      "text": "@mch",
      "color": "#66ffdd",
      "backgroundColor": "#116644",
      "borderRadius": "2px",
    },
  ],
```

This is helpful to quickly lookup all the places that cought my eye.
Ctrl + Shift + F for them with `@mch` or use TODO plugin lookup.

These 3 tips help me with day to day work.
What are your hacks for productive work??
