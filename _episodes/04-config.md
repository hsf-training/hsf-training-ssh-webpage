---
title: "The config file!"
teaching: 0
exercises: 0
questions:
- "How can I avoid typing long SSH commands?"
- "How can I automatically hop via an intermediate server?"
- "How can I set and remember advanced settings?"
objectives:
- "Write your first SSH config file!"
keypoints:
- SSH configuration file is in ``.ssh/config``
- It allows us to automate and configure ssh
- We can define short names to connect to servers
- We can automate jumping between hosts
---
Now you can already connect to a server but if your username or the server name
is long you will have to type all of this every time you connect. Luckily we can
automate this using the SSH configuration file ``.ssh/config`` in your home
directory. Usually this file doesn't exist but you can simply create an empty
file with that name.

The syntax of the configuration file is very simple. It's just the name of a
configuration option followed by it's value. For example, to send periodic
status updates which might help keep connections from disconnecting we can
simply write the following in the file:

{% include includeconfiglines filename='code/ssh_config.txt' start=10 stop=11 %}

But more importantly we can also define "hosts" to connect to and settings that
should only apply for these hosts

{% include includeconfiglines filename='code/ssh_config.txt' start=13 stop=15 %}

This now allows us to just execute ``ssh desy`` and the correct username and
full hostname are taken from the configuration file. This will also work with
``scp`` so now you can just use the shorter version

```bash
scp desy:/etc/mtod bastion-message
```

to copy a file from the desy login server. This now also allows us to automate
the login to KEKCC via the gateway server

{% include includeconfiglines filename='code/ssh_config.txt' start=17 stop=25 %}

The line containing ``ProxyJump`` tells ssh to not directly connect to the host
but first connect to the gateway host and then connect from there. We could make
this more complicated if needed by also adding a ProxyJump to the gateway server
configuration if we need to perform even more jumps. You should now be able to
login to KEKCC by just typing ``ssh kekcc`` and also copy files directly with
``scp``. But you will have to enter your password two times, once when
connecting to the gateway server and then when connecting to the KEKCC machine.

> ## In case of `ProxyJump` trouble
> The `ProxyJump` directive was introduced in OpenSSH 7.3. If you get an
> error message `Bad configuration option: proxyjump`, please check if
> you can update your SSH client.
>
> While we definitely recommend you to get an up-to-date system that can use
> the newer version, a quick workaround is to replace the `ProxyJump` line
> with the following (using `ProxyCommand`):
>
> ```
> ProxyCommand ssh hostname -W %h:%p
> ```
> Where `hostname` should be the server you jump through, so
> `sshcc1.kek.jp` in our case.
{: .callout}

> ## Exercise
> Add a working kekcc configuration to your config file and verify that
> you can login to kekcc by simply writing ``ssh kekcc``
> > ## Hint
> > Create the config file and take the above snippet. But make sure you replace
> > your own usernames for both servers.
> {: .solution}
{: .challenge}
