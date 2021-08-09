---
title: "Port forwarding"
teaching: 0
exercises: 0
questions:
- "How can I connect to Jupyter notebook running on the server?"
- "How can I connect to a website tunnelling to a server?"
objectives:
- "Forward ports"
keypoints:
- "each network connection consists of a host and a port number"
- "port forwarding allows to connect to other services via ssh"
- "`ssh -L localport:remotehost:remoteport server` will forward all connections
  to `localport` on the local machine to whatever is called   `remotehost` at
  port `remoteport` on `server`"
- "this allows to open jupyter notebooks on kekcc or other computing centers"
---

One of the last topics we need to discuss is the ability of SSH to do port
forwarding. For this we first need to quickly explain what a port is: In
computer networks you need two things to connect to a computer, it's address and
a port number. The address identifies which machine to talk to and the port
number is just a simple number between 1 and 65535 to identify the service or
program to talk to.

Which service uses which port number is not fully fixed so in theory we could
run anything on any given port number but that would make the internet very
complicated. So there is a set of "well known" port numbers which are used by
default. For example, ssh uses port 22 by default and when browsing the web the
https connections are on port 443 while the older, un-encrypted http is on port
80.

Now sometimes you might want to have a connection other than ssh to a machine
like the KEKCC login node that is not reachable. For example to reach a
web server like `http://bweb3.cc.kek.jp` that is only reachable inside the kek
network we need to connect to port 80 on the machine `bweb3.cc.kek.jp`

ssh can help us do that: we can instruct it to make something like a tunnel and
route all the network traffic through its encrypted channel. The most common
case for this is called local port forwarding: ssh creates opens port on the
local machine and forwards all requests to connect to this one to another
machine on the other end of the ssh connection. So getting back to the example:

```bash
ssh -L 8080:bweb3.cc.kek.jp:80 kekcc
```

This will open a connection to kekcc and the local port 8080. Any connection to
this port is then forwarded to `bweb3.cc.kek.jp` on port 80. So now to connect
to bweb3 we can open our local web browser and enter `http://localhost:8080`
and we should see it open the correct webpage.

Note that the 8080 in the local side can be chosen freely but only one program
can open a port at any given time and you can only use ports above 1024 unless
you have administrator privileges. 8080 is a "typical" port used for forwardings
and is usually free. But any number is fine.

> ## Question
> How would the command look like if I want to open https://software.belle2.org
> via a connection to `bastion.desy.de`? What do I need to type in my
> web browser?
> > ## Hint
> > Make sure to choose the correct port
> {: .solution}
> > ## Solution
> > We need to run `ssh -L 8080:software.belle2.org:443 desy` and then type
> > `https://localhost:8080` in the browser.
> {: .solution}
{: .challenge}


### Programs that open ports on the target machine

One special case is running programs on the other side that directly open a port
on the machine you're working on for you to connect. The most prominent example
in our field are `Jupyter notebooks <https://jupyter.org/>`_ which offer a very
nice python interface via web browser.


> ## Warning
> Opening a port is not user specific but opens the port visible to **all
> users on the network**. So whenever you open a port to listen to connections
> you should make sure it cannot be misused. Jupyter does this for you with
> passwords or token strings so you don't need to worry in this specific case.
{: .caution}

> ## Weblogins
> Some organizations like DESY offer direct weblogins (e.g. [for desy](https://confluence.desy.de/x/rJetC))
> for Jupyter notebooks so you might have an easier solution at your fingertips.
{: .callout}

Now you can tell jupyter notebooks which port to use but this time we run it on
the KEKCC computers and there might be other users there so it can be a bit
tricky to find an open port number. This means you should not just use "8080".
You should pick a random number between 1025 and 65535 for yourself.

Now we can connect to kekcc and forward this port number to the same number on
the connected host. In this example we'll keep 8080 but you should really pick
your own number.

```bash
ssh -L 8080:localhost:8080 kekcc
```

In this case `localhost` means "whatever is called the local machine on the
remote side of the connection". Once that is done we can try to start a jupyter
notebook on kekcc

```bash
source /cvmfs/belle.cern.ch/tools/b2setup release-05-00-01
jupyter notebook --port=8080 --no-browser
```

This should print a bit of information including a bit at the end that looks
like this:

```
Or copy and paste one of these URLs:
  http://localhost:8080/?token=...
```

Now you just need to make sure that the number is the same as you chose. If that
is not the case then the port you chose is already occupied and you need to stop
the notebook (press `Ctrl-C`), disconnect ssh and try with a different number.

If it is the same number you're all good and you can just copy-paste the full
link (including the characters after `token=`) into your web browser and you
should see a notebook interface open up.
