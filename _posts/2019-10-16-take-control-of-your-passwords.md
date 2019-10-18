---
layout: post
title:  "Taking Control of Your Passwords"
date:   2019-10-15 10:25:49 +1000
categories: jekyll update
---

## Table of Contents

* TOC
{:toc}

## Introduction

At the start of this year, I decided that I didn't really like the fact that I didn't know how my password manager worked. I didn't know where my passwords were stored in their infrastrucutre, I didn't know who had access to my passwords, and I didn't know how they were being handled. Because of all these unknowns, I decided to look into setting up my own password mananger.

During my research about open source password managers I happened to stuble across an open source password manager called 'pass'. [pass](https://www.passwordstore.org/) is an open source password manager built upon the Unix philosophy. Pass uses Gnu Privacy Guard (GPG) which is an open source implementation of Open Pretty Good Privacy (OpenPGP).

The rest of this post will show you the initial setup and then list some tools I use to make pass more user friendly.

## Initial Setup

The initial setup for pass is very straight forward.

### Generate a pgp key.

{% highlight bash %}
gpg --full-generate-key
Please select what kind of key you want:
   (1) RSA and RSA (default)
   (2) DSA and Elgamal
   (3) DSA (sign only)
   (4) RSA (sign only)
Your selection? 
RSA keys may be between 1024 and 4096 bits long.
What keysize do you want? (2048) 4096
Requested keysize is 4096 bits
Please specify how long the key should be valid.
         n 0 = key does not expire
         n d = key expires in n days
         n w = key expires in n weeks
         n m = key expires in n months
         n y = key expires in n years
Key is valid for? (0) 0
Key does not expire at all
Is this correct? (y/N) y

GnuPG needs to construct a user ID to identify your key.

Real name: Daniel Moore
Email address: danielmoore.info@gmail.com
Comment: RSA GPG key for Pass
You selected this USER-ID:

Change (N)ame, (C)omment, (E)mail or (O)kay/(Q)uit? O

{% endhighlight %}

### Install Pass
Install the pass package from your distros package manager.

{% highlight bash %}
# Arch
sudo pacman --sync pass
# Ubuntu
sudo apt update
sudo apt install pass
{% endhighlight %}

Now initialise pass by passing the in ID of the generated GPG key.

{% highlight bash %}
gpg --list-keys

pub   rsa4096 2019-05-21 [SC]
      8F8F7070826647DC61E303D77AF6B9
sub   rsa4096 2019-05-21 [E]
{% endhighlight %}


initialize pass by passing in the ID of our GPG key

{% highlight bash %}
pass init 8F8F7070826647DC61E303D77AF6B9
{% endhighlight %}

### Setup Pass to Sync to GitHub

First, create a private GitHub repo where you will sync your encrypted passwords to.

```bash
pass git init
pass git remote add origin "https://github.com/danielmoore-info/passdb.git"

pass insert Personal/Social/facebook.com
mkdir: created directory '/home/daniel/.password-store/Personal'
mkdir: created directory '/home/daniel/.password-store/Personal/Social'
Enter password for Personal/Social/facebook.com: 
Retype password for Personal/Social/facebook.com: 
[master 872d2ed] Add given password for Personal/Social/facebook.com to store.
 1 file changed, 0 insertions(+), 0 deletions(-)
 create mode 100644 Personal/Social/facebook.com.gpg

pass git push -u origin master
```

That's all for the initial setup.

## Using Pass

1. Use pass via the command line, figure it out with:
```bash
man pass
```
2. Use pass on Linux using this dmenu script <https://git.zx2c4.com/password-store/tree/contrib/dmenu> Basically create a shortcut that will run this script so you can copy your passwords without having to type a command
3. On windows, I like this project <https://github.com/geluk/pass-winmenu>

You can find a heap of similar projects on the pass website:
<https://www.passwordstore.org/>

