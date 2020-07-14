---
title: Check events fired while interacting with a DOM element
date: 2020-07-14
template: post
draft: false
url: /monitor-events/
description: How to check events fired while interacting with a DOM element
keywords: [DOM, event, log]
tags: [
  # code,
  # no_code,
  note,
  # idea,
  # make_log,
  ]
---

There is an easy way of monitoring all events fired while interacting with a DOM element.

Open dev tools and select element.

![screen1](./screen1.png)

While the element is selected, switch to console and type: `monitorEvents($0)`

![screen2](./screen2.png)

Interact with the element: hover, click, etc.
The console should print all the events fired.

![screen3](./screen3.png)

