---
layout: post
title:  "WSL2 for Penetration Testing"
date:   2019-07-26 10:25:49 +1000
categories: jekyll update
---

When WSL1 was released, I was excited at the possibility of being able to run docker, and have all my penetration testing tools in a Windows machine. However, as you probably know, WSL1 was quite the let down for these use cases. Firstly WSL1 only translated some Linux system calls to their appropriate Windows sytem calls. Secondly, many important tools did not run on WSL1, for example, nmap. Thirdly, I/O performance was horrible, this was especially noticeable when downloading many files during updated or cloning large github repositories.

Microsoft has recently announced [WSL2](https://devblogs.microsoft.com/commandline/announcing-wsl-2/) which will ship with a complete Linux Kernel. WSL2 will run the Linux Kernel in a 'lightweight VM', instead of trying to translate the system calls, the VM will deal with them.

I wanted to explore the possibility of using WSL2 during for penetration testing, instead of using virtual machines, as they have significant overhead, and WSL2 is supposed to have minimal impact on resource usage. The rest of this blog will look at getting early access to WSL2 and determining if it could be used for penetration testing.
 **Note: Opting into the program requires you to use a Microsoft online account, they will also change some of your privacy settings for "feedback"**

 ### Getting WSL2

 WSL2 is currently only available to users who have opted in to the Windows Insider Program. To join this program, users can go to Settings > Update & Security > Windows Insider Program and go through the process of joining the program.



Windows Subsystem for Linux (WSL) provides a compatability layer for running Linux binaries on Windows. This is done by translating system calls to native Windows system calls. However, 

