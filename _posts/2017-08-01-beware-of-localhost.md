---
title: 'Beware of localhost'
author: 'Jakub Suchy'
date: '2017-08-01T20:48:59+02:00'
layout: post
permalink: /2017/08/01/beware-of-localhost/
categories:
    - Personal
---

One day at work we encountered odd bug with our application. It was Windows Service that was client/server for TCP/IP connections.

The bug was that the server part didn’t seem to listen to any connection.

The oddness of this bug was, that this bug occurred only when service started automatically after restarting PC. When we restarted the service it started to work.

My first idea was that somehow DNS service was connected. (thinking back, it was totally stupid from me thinking that) So my first attempt was disabling DNS cache. Obviously that didn’t help.

Secondly I thought that maybe there were some dependent services for network communication which started after our service. So I set our service to start automatically but delayed. This fixed the underlining issue. But obviously I wasn’t satisfied without knowing what was the culprit.

Then I finally remembered that I can enumerate all listening ports. So I did that. First I enumerated when the service didn’t work and then when the service worked. To my big surprise on this port the IP addresses were different… So by taking this into consideration I deduced that network is not immediately connected after restart so our service bound to the wrong network 127.0.0.1 instead of our internet facing IP address.

Good thing that I investigated this further, because If I was satisfied with delayed autostart I wouldn’t find that our server bound on localhost IP. Why binding to specific IP? Why not to listen globally on all networks?

In the end, we changed binding to IP 0.0.0.0 which listens on all networks.

My take from this is to investigate issues into depth and not be satisfied with “just a quick-fix”

Cheers!