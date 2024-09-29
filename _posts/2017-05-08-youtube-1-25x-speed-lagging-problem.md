---
title: 'Youtube 1.25x speed lagging problem'
author: 'Jakub Suchy'
date: '2017-05-08T06:00:58+02:00'
layout: post
permalink: /2017/05/08/youtube-1-25x-speed-lagging-problem/
categories: ["Personal"]
---

Funny thing happend to me. I was reinstalling my PC the other day and when I set it up again, I wanted to watch some youtube videos. But when I started to watch them in [1.25x speed or more](http://jakubsuchy.com/2017/05/02/my-way-of-learning/) they were seriously lagging and obviously I didn’t have ANY clue why. So I backtracked some of my steps of installing and setting up my PC. And guess what change fixed that youtube problem… Reverting my Playback Device Advanced Setting Format from 24bit, 192KHz to something lower fixed it. Possibly my default sound card couldn’t handle this much data and youtube was compensating for it by lagging. So the sound card could keep up.