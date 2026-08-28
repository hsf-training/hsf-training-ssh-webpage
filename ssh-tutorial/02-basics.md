# Basics

```{admonition} Learning Objectives
- Connect to a server via ssh
- Copy files from your local computer to the host server
- Copy files from your host server to your local computer
- Debug SSH connection problems
```

Now back to SSH: When we connect to a server using SSH, it will use the same
method of asymmetric encryption to establish the identity of the server. When you connect for the
first time, SSH will ask you if you know this computer. If you type for the first time

```bash
ssh <username>@<servername>
```

(again, replace `<username>` for your collaboration username and `<servername>` for your collaboration servername), you
should see something like

```
The authenticity of host 'bastion.desy.de (131.169.5.82)' can't be established.
RSA key fingerprint is SHA256:WbkI/Ko+FdCbIAVn6ky2odyWxCvCL3+5XqWSZQ6PynE.
Are you sure you want to continue connecting (yes/no)?
```

This means that ssh doesn't have the public key of the servername, bastion.desy.de (in this case), which is
called the "host key". Therefore, it warns that it cannot be sure you connected to the right server. It's
usually fine to just say yes; it will only ask you the first time and remember
this decision. After you enter yes, you will get a message like


```
Warning: Permanently added 'bastion.desy.de' (RSA) to the list of known hosts.
```

which is perfectly normal. In the next step, the server will ask you for your
password and after that, you should see a command line prompt and are
now connected.

Once you are connected, the next important step is to disconnect. To do so, just type
`exit` and press return (enter), and your connection will be closed. If you're very
impatient you can also press `Ctrl-D` as a shortcut.

```{warning} Long running jobs
Don't run long-running and CPU or memory heavy jobs on login nodes like
the one you just used.
Login nodes are shared
resources for all users and it's not very polite and, mostly, it is also not
permitted to occupy them with calculations that could be done on dedicated
machines.
You probably have access to batch systems to submit resource intensive jobs!
```

````{admonition} Exercise
:class: tip
Login to your desired server and verify that the login succeeded with the
`hostname` command and log out again.
````

```{note} One final thing about host keys
After you connected to a server, ssh
remembers the host key and will verify it on each connection. So you might
see something like this:

~~~
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
@    WARNING: REMOTE HOST IDENTIFICATION HAS CHANGED!     @
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
IT IS POSSIBLE THAT SOMEONE IS DOING SOMETHING NASTY!
Someone could be eavesdropping on you right now (man-in-the-middle attack)!
It is also possible that a host key has just been changed.
The fingerprint for the RSA key sent by the remote host is
SHA256:zyIMwlji8jqtD+UuSFuknQmevQPAUCiT39BfH/NrIbA.
Please contact your system administrator.
~~~

This means the host presented a different key than it used to. This can
sometimes happen if the server you want to connect to was reinstalled. If
**you know** that the server was reinstalled or upgraded, you can tell ssh to
forget the previous host key by running
```bash
ssh-keygen -R <servername>
```



## Copying Files

In addition to just connecting to a remote shell we can also ssh to copy files
from one computer to another. Very similar to the `cp` command, there is a
`scp` command, for a "Secure Copy". To specify a file on a server just precede
the filename with the ssh connection string followed by a colon. For example:

```bash
scp desyusername@bastion.desy.de:/etc/motd bastion-message
```

This will copy the file `/etc/motd` into the current directory on your local computer as `bastion-message`. In the same way,

```bash
scp bastion-message desyusername@bastion.desy.de:~/
```

will copy the file `bastion-message` from your local directory into your home
directory on the server `bastion.desy.de`.


````{admonition} Exercise
:class: tip
Copy a file from your local computer to your server's home directory
```{dropdown} Hint
You can use the `touch` command to create an empty file to copy.
```
````

````{admonition} Exercise
:class: tip
Copy a file from your server's home directory to you local
directory.
````

````{admonition} Exercise
:class: tip
Copy a full directory with all files at once either from your local computer to the server or from the server to your local computer.
```{dropdown} Hint
Try `man scp` or `scp --help` to get to know about scp flags that may be of use.
```
```{dropdown} Solution
Supply `-r` to scp to recursively copy the full directory.

However, for more efficient copying of large amount of files you should consider
using `rsync`
```
````

## Debugging


If you run into trouble in one of the following sections, it can be very
instructive to switch on debugging output to get to know possible issues, by using the `-v` flag of ssh:

```bash
ssh -v <username>@<servername>
```

Once you have created a configuration file (next section) it can also sometimes
be helpful to disable it to rule out this source of error. This can be done
by using the `-F` option to specify a blank config file:

```bash
ssh -F /dev/null <username>@<servername>
```

```{admonition} Key Points
:class: tip
- Simple connection to a server is just done by `ssh username@servername`
- `scp` can be used to copy files from and to remote computers
- SSH uses host keys to ensure the identity of the server you connect to
```
