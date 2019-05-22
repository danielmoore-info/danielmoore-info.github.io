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


Install the pass package from your distros package manager.

{% highlight bash %}
sudo pacman --sync pass
{% endhighlight %}

Now initialise pass by passing the in ID of the generated GPG key.
{% highlight bash %}
gpg --list-keys

pub   rsa4096 2019-05-21 [SC]
      8F8F7070826647DC61E303D77AF6B9D316BACF56
uid           [ultimate] Daniel Moore (RSA GPG key for Pass) <danielmoore.info@gmail.com>
sub   rsa4096 2019-05-21 [E]
{% endhighlight %}


initialize pass by passing in the ID of our GPG key

{% highlight bash %}
pass init 8F8F7070826647DC61E303D77AF6B9D316BACF56
{% endhighlight %}

{% highlight bash %}
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

{% endhighlight %}

https://git.zx2c4.com/password-store/tree/contrib/importers/lastpass2pass.rb


❯ gpg --export -a 8F8F7070826647DC61E303D77AF6B9D316BACF56 > DanielMoorePassDb.pub             
❯ gpg --export-secret-keys -a 8F8F7070826647DC61E303D77AF6B9D316BACF56 > DanielMoorePassDb.priv
