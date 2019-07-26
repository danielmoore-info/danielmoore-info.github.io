---
layout: post
title:  "WSL2 for Penetration Testing"
date:   2019-07-26 10:25:49 +1000
categories: jekyll update
---

When WSL1 was released, I was excited at the possibility of being able to run docker, and have all my penetration testing tools in a Windows machine. However, as you probably know, WSL1 was quite the let down for these use cases. Firstly WSL1 only translated some Linux system calls to their appropriate Windows sytem calls. Secondly, many important tools did not run on WSL1, for example, nmap. Thirdly, I/O performance was horrible, this was especially noticeable when download many files during updated or cloning large github repositories.

Microsoft has recently announced WSL2. 


Windows Subsystem for Linux (WSL) provides a compatability layer for running Linux binaries on Windows. This is done by translating system calls to native Windows system calls. However, 

