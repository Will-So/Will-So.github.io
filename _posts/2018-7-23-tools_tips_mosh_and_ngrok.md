---
layout: post
title: Making working on Remote Boxes a Breeze 
draft: false
tags: [Unix, Cloud, Tool Tips]
---
Up until recently, I rarely used the beefy workstation that I built out of frustration with the performance of my laptops' performance on intensive tasks. This all changed last week and the catalyst was a tool called Mosh. [Mosh](https://mosh.org/) is an alternative to ssh that has all the features I always wished it had. With mosh, there are no more broken pipes and no more fighting with tenuous internet connections. Here's what my setup looks like now:

![](https://www.dropbox.com/s/iddk6cueomtfvc5/sl011.png?dl=1)

 If you would normally type `ssh do` to ssh into your remote computer, instead you just do `mosh do`. It's that simple! It uses `.ssh/config` that you already probably use for ssh. For me the main benefits of mosh are (1) decreased latency and (2) perpetual sessions. I can leave one of my laptops off for days and the moment I turn it back on, my remote tmux session is still there. Mosh allows me work on remote from one laptop during the day and then seamlessly pick up what I was working on in the evening from another laptop[^1]. 

The magic happens when I run a tool called [ngrok](https://ngrok.com/) from my mosh session. ngrok lets me expose and navigate to a port on a remote machine from anywhere without messing with firewalls or anything. If I'm using my workstation to develop a website, I'd like to be able to connect to that website locally. By the same token, if I'm wearing my Data Scientist hat, I'd like to be able to access [Jupyter Lab](http://jupyterlab.readthedocs.io/en/stable/). Ngrok makes both of these things possible. Here's what it looks like:

![](https://www.dropbox.com/s/fvpmy43xvq041lz/sl012.png?dl=1)

Here, I'm forwarding my Jupyter Lab server to `ngrok`, which I can then access from any of my laptops:

![](https://www.dropbox.com/s/7s1dq378grmvsnk/sl010.png?dl=1)

`ngrok` has a bunch of security features to make this quite secure as the attack service is now quite small[^2]. 

Thanks to `ngrok` and `mosh` I can now work on the terminal and `localhost` on remote as if everything were locally hosted. That's great but there's still one problem -- how can I work in an IDE like PyCharm on remote?[^3] The only seamless solution I could think of is keeping files syncronized in Dropbox. So far, this has been working fine for me. I use the IDE locally and then, within a minute of making the change, the change is propagated to remote. In the terminal I can then start the website/script/whatever. I'm still experimenting with this setup and it *feels* like this Dropbox syncronization is going to burn me one day. 

So with all this, working on a powerful remote workstation is very nearly as convenient as working on my 12 inch, 2 pound laptop. 

## Footnotes


[^1]: Before I had to ssh into my cloud vps session and then type in another ssh command from there to get into my remote workscreen. Once I was there, typing on the terminal would respond rather slowly as my typing always had to make a round trip before showing up on my screen.

[^2]: My VPS is completely locked down except for an ssh port being open to the world. For an intruder to get terminal access, they'd either have to find a 0 day in ssh or an exploit in both `ngrok` and whatever app I am serving using ngrok. My VPS also has tools built in to detect intrusions. Since I am using a [reverse tunnel](https://blog.devolutions.net/2017/3/what-is-reverse-ssh-port-forwarding) from my workstation to my VPS, it's not possible to log in directly to my workstation. In the case of Jupyter Lab, I have two layers of authentication set up since compromising that allows shell access.

[^3]: With pycharm in particular, there's a better solution if you connect directly to remote without using an intermediary machine. PyCharm has the functionality to use remote python interpreters. See [here](https://www.jetbrains.com/help/pycharm/configuring-remote-interpreters-via-ssh.html) for more details.
