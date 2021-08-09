---
title: "Basics"
teaching: 0
exercises: 0
questions:
objectives:
keypoints:
- "simple connection to a server is just done by ``ssh username@servername``"
- "``scp`` can be used to copy files from and to remote computers"
- "you can jump between hosts by executing ssh in a ssh connection"
- "ssh uses host keys to ensure the identity of the server you connect to"
---
Now back to SSH: When we connect to a server using SSH it will use the same
method to establish the identity of the server. So when you connect for the
first time SSH will ask you if you know this computer. So if you type

```bash
ssh <desyusername>@bastion.desy.de
```

(and replace ``<desyusername>`` by your DESY username) for the first time you
should see something like

~~~
The authenticity of host 'bastion.desy.de (131.169.5.82)' can't be established.
RSA key fingerprint is SHA256:WbkI/Ko+FdCbIAVn6ky2odyWxCvCL3+5XqWSZQ6PynE.
Are you sure you want to continue connecting (yes/no)?
~~~
{: .output}

This means that ssh doesn't have the public key of ``bastion.desy.de`` which is
called the "host key" so it cannot be sure you connect to the right server. It's
usually fine to just say yes, it will only ask you the first time and remember
this decision. So after you enter yes you will get a message like


~~~
Warning: Permanently added 'bastion.desy.de' (RSA) to the list of known hosts.
~~~
{: .output}

which is perfectly normal. In the next step the server will ask you for your
password and after you entered that you should see a command line prompt and are
now connected.

Once you are connected the next important step is to disconnect, just type
``exit`` and press return and your connection will be closed. If you're very
impatient you can also press ``Ctrl-D`` as a shortcut.

> ## Warning: Long runing jobs
> Don't run long-running and CPU or memory heavy jobs on login nodes like
> we just used.
> The login nodes are shared
> resources for all users and it's not very polite and mostly also not
> permitted to occupy them with calculations that could be done on dedicated
> machines.
> You probably have access to batch systems to submit resource intensive jobs!
{: .callout}

> ## Exercise
> Login to ``bastion.desy.de``, verify that the login succeeded with the
> ``hostname`` command and log out again.
{: .challenge}

> ## One final thing about host keys
> After you connected to a server, ssh
> remembers the host key and will verify it on each connection. So you might
> see something like this:
>
> ~~~
> @@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
> @    WARNING: REMOTE HOST IDENTIFICATION HAS CHANGED!     @
> @@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
> IT IS POSSIBLE THAT SOMEONE IS DOING SOMETHING NASTY!
> Someone could be eavesdropping on you right now (man-in-the-middle attack)!
> It is also possible that a host key has just been changed.
> The fingerprint for the RSA key sent by the remote host is
> SHA256:zyIMwlji8jqtD+UuSFuknQmevQPAUCiT39BfH/NrIbA.
> Please contact your system administrator.
> ~~~
> {: .output}
>
> This means the host presented a different key than it used to. This can
> sometimes happen if the server you want to connect to was reinstalled. So if
> **you know** that the server was reinstalled or upgraded you can tell ssh to
> forget the previous host key. For example to forget the host key for
> ``bastion.desy.de`` just use
> ```bash
> ssh-keygen -R bastion.desy.de
> ```
{: .callout}


## Copying Files

In addition to just connection to a remote shell we can also ssh to copy files
from one computer to another. Very similar to the ``cp`` command there is a
``scp`` command for "Secure Copy". To specify a file on a server just precede
the filename with the ssh connection string followed by a colon:

```bash
scp desyusername@bastion.desy.de:/etc/motd bastion-message
```

This will copy the file ``/etc/motd`` to your local computer into the current
directory. In the same way

```bash
scp bastion-message desyusername@bastion.desy.de:~/
```

will copy the file ``bastion-message`` from your local directory into your home
directory on ``bastion.desy.de``.


> ## Exercise
> Copy a file from your local computer to your home directory on
> ``bastion.desy.de``
> > ## Hint
> > You can use the ``touch`` command to create empty files
> {: .solution}
{: .challenge}

> ## Exercise
> Copy a file from your home directory on ``bastion.desy.de`` to you local
> directory
{: .challenge}

> ## Exercise
> How can we copy full directories with all files at once?
> > ## Hint
> > Try `man ssp`
> {: .solution}
> > ## Solution
> > Supply ``-r`` to scp: ``scp -r desy:~/plots .`` will try to copy the full
> > directory ``plots`` on the remote machine to your current directory
> >
> > However for more efficient copying of large amount of files you should consider
> > using ``rsync``
> {: .solution}
{: .challenge}

## Debugging


If you run into trouble in one of the following sections it can be very
instructive to switch on debugging output by using the ``-v`` option of ssh:

```bash
ssh -v username@servername
```

Once you have created a configuration file (next section) it can also sometimes
be helpful to disable it to rule out this source of error. This can be done
by using the `-F` option to specify a blank config file:

```bash
sh -F /dev/null username@servername
```
