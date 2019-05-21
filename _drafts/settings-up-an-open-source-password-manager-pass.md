---
layout: post
title:  "Setting Up the Open Source Password Manager 'Pass'"
date:   2019-05-15 10:25:49 +1000
categories: jekyll update
---




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
         0 = key does not expire
      <n>  = key expires in n days
      <n>w = key expires in n weeks
      <n>m = key expires in n months
      <n>y = key expires in n years
Key is valid for? (0) 0
Key does not expire at all
Is this correct? (y/N) y

GnuPG needs to construct a user ID to identify your key.

Real name: Daniel Moore
Email address: danielmoore.info@gmail.com
Comment: RSA GPG key for Pass
You selected this USER-ID:
    "Daniel Moore (RSA GPG key for Pass) <danielmoore.info@gmail.com>"

Change (N)ame, (C)omment, (E)mail or (O)kay/(Q)uit? O

{% endhighlight %}