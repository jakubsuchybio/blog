---
id: 114
title: 'Lumia 950 won&#8217;t connect to PC'
date: '2017-05-15T06:00:59+02:00'
author: 'Jakub Suchy'
layout: post
guid: 'http://jakubsuchy.com/?p=114'
permalink: /2017/05/15/lumia-950-wont-connect-to-pc/
categories:
    - Personal
---

I don’t know how long has it been, that my phone (Lumia 950) wouldn’t connect to my PC. But one day I really needed to copy some photos from my phone (Lumia 950) to my PC. And that damn phone still just wouldn’t connect! So I started my investigation in Device Manager, where there was an Unknown Device of type “MTP Device”. After trying to update that driver I got this:

[![](http://jakubsuchy.com/wp-content/uploads/2017/05/mmc_2017-05-12_19-14-22-300x246.png)](http://jakubsuchy.com/wp-content/uploads/2017/05/mmc_2017-05-12_19-14-22.png)

“INF is invalid”

This also was a case for Lumia 650. The same problem.

Then I started googling and after a few minutes my problem was solved:

https://answers.microsoft.com/en-us/windows/forum/windows\_10-hardware/solved-mtp-not-working-on-windows-10-aniversary/391842ee-bacc-4e35-84e2-8089dfdbb0bf

This issue was bugging me for weeks or even monts and it took only few minutes to fix it. So if you have some issue that is bugging you a long time, don’t be lazy and give it a few minutes and you might solve it in a few minutes like I did.

Cheers!