---
id: 129
title: 'Profi Účtenka Printer hell'
date: '2023-05-07T12:23:32+02:00'
author: 'Jakub Suchy'
layout: post
guid: 'https://jakubsuchy.com/?p=129'
permalink: /2023/05/07/profi-uctenka-printer-hell/
categories:
    - Personal
tags:
    - 'C#'
    - CheckNetIsolation.exe
    - dnSpy
    - Printer
    - 'Profi Uctenka'
    - TCP/IP
    - 'Windows Service'
    - Wireshark
---

I bought a new notebook for my wife for her business, tried to setup the already working Profi Uctenkovka with the printer from her old notebook, but it wouldn’t work.

TL;DR Just add Profi Uctenkovka to the CheckNetIsolation.exe tool via “CheckNetIsolation.exe LoopbackExempt -a -n=”55FF9867.26423387A37DD\_rp4kqb6eg96dj””

Profi Uctenka is an app that tracks receipts for entreprenuers. It is quite popular in Czech Republic. Easy to work with. Until you want to connect a printer via USB that doesn’t talk with the app via USB. 🤣

The architecture is like this:  
– Profi Uctenkovka – UWP C# app that acts as a TCP/IP Client  
– USBPrintServerService – C# Windows Service app that acts as TCP/IP Server  
– UsbPrinter.dll – C# Library to handle printing via USB  
I don’t know why Profi Uctenkovka did not just use UsbPrinter.dll directly, maybe because UWP doesn’t have access to the USBs, dunno.

After setting it up:  
– Install drivers for the printer  
– Install Profi Uctenkovka  
– Download+Unzip the USBPrinteServer  
– Run UsbPrintServer and set the port \[9101\] for it  
– Run Profi Uctenkovka and add printer with ip \[127.0.0.1\] and port \[9101\]  
The “Test Printer” button in the Profi Uctenkovka wouldn’t work.

Debugging:  
– There was a weird behavior where the standard scenario – Start PC, Services starts, user logs into Windows and then starts the app and presses the Test Printer button – wouldn’t work. However I found out, that when I restarted the USBPrintServerService, it would actually print! So the actual printer and the printing would work, but the connection via TCP was the problem. Later I found out, that Profi Uctenkovka tries to retry the Test Print indefinitely, that is why after restarting Windows service it went through somehow 😂  
– I knew that it would work somehow, because the previous notebook worked with this printer. So I started to dig deeper.  
– I poke into the apps if they are C# apps by trying to disassemble them with dnSpy.  
– To my surprise and luck, the USBPrintServerService and UsbPrinter.dll were indeed C# apps. However the Profi Uctenkovka is Xamarin.Forms app that is compiled to UWP, Android and Apple apps. The UWP part is not disassembleable.  
– I tried to attach debugger to the USBPrintServerService and look what was happening. I was TcpListener started, but the connection wouldn’t go through.  
– At first I reproduced the C# code of the USBPrintServerService so that I could debug it more easily. But that didn’t change anything.  
– So I tried to mockup new C# Console app that would connect to that service. And it worked straight away.  
– Here I tried to look at the communication via WireShark on the loopback interface with \[tcp.port == 9101\] filter.   
 – The ConsoleApp+MyNewService were sending standard TCP/IP SYN and SYN+ACK packets, everything normal.  
 – However the ProfiUctenkovka+MyNewService would only send SYN from the client and it would not arrive to the MyNewService. In the wireshark there was a “\[TCP Retransmission\] \[TCP Port numbers reused\] 59996 → 9101 \[SYN\] Seq=0 Win=65535 Len=0 MSS=65495 WS=256 SACK\_PERM”  
– At some point I thought it would be the firewall. But it wasn’t. Tried to disable it and it still wouldn’t work.  
– After some searching I found these 2 articles:  
https://stackoverflow.com/questions/62661036/connecting-to-tcp-server-running-in-uwp-app  
https://github.com/microsoft/WindowsAppSDK/issues/113#issuecomment-665918424  
In both sources there was basically the same conclusion:  
 – Check UWP Capabilities to have “privateNetworkClientServer”  
 – To add “uap4:LoopbackAccessRules”  
 – To add the Profi Uctenkovka uwp app id in the “CheckNetIsolation.exe” tool  
– I looked into the Profi Uctenkovka AppxManifest if there were the capabilities as listed and they were not.  
– Here I created new UWP the same way as the ConsoleApp for checking the connection from client side. And after deploying it to my PC it worked immediatelly only with the capability “internetClient” that Profi Uctenkovka already had. =&gt; So it had to be something else.  
– I then checked the list of apps registered in that “CheckNetIsolation.exe” tool and this new UWP app was immediately there!!!  
– So then I tried to find the ID of the Profi Uctenkovka app so that I could add it manually there, found it somehow through “Get-AppxPackage” command  
– I Added the Profi Uctenkovka ID into the “CheckNetIsolation.exe” via “CheckNetIsolation.exe LoopbackExempt -a -n=”55FF9867.26423387A37DD\_rp4kqb6eg96dj”” =&gt; And VOILA it started to work.  
– I knew, that UWP apps have sandbox-like isolation from the rest of the computer, but I didn’t know, that they have additional loopback isolation firewall. Funny thing is, that the second scenario with restarting the Windows Service works. Maybe there is a bug in that firewall or something.  
  
On a paper retrospectively it looks easy, but more than 20hours took me to fix this problem. It took quite a toll on my brain. Hopefully in the future I won’t have this problem with it again after writing this article. And hopefully this article will help other people that have the same problem.  
  
Kudos to Ondra and Homie from my workplace for moral and technical support to get through this hell. Thank you! 🧡