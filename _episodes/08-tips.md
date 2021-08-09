---
title: "Additional tips & tricks"
teaching: 0
exercises: 0
questions:
objectives:
- "Use `rsync` to copy large/many files"
- "Use tmux to keep your sessions alive and for tiling your terminal."
keypoints:
- "There's a lot to be discovered!"
---

By now you hopefully know all the things necessary to comfortably work with ssh
and configure it to your liking. There are a few more things that can make
working with ssh even more comfortable so we just collect them here for people
interested in this.

## `rsync` for file transfers

Very often one might want to synchronize a folder from a server with a local
machine. You already downloaded most of it but now you created a few new plots
and running `scp -r` would copy everything again unless you really specify
just the new files.

There is a program to solve exactly this problem called `rsync`. By default it
works on folders and will only transmit what is necessary. The most common use
case is

```bash
rsync -vaz server:/folder/ localfolder
```

which will efficiently copy everything in `folder` on `server` and put it in
the directory `localfolder` (beware, it matters whether or not you put a slash
at the end of the target)/ The most common options are

| Option     | Explanation |
| ---------- | ----------- |
`-v`         |  verbose mode, print file names as they are copied
`-a`         |  archive mode, copy everything recursively and preserve file times and permissions
`-z`         | compress data while transmitting it
`-n`         | Only show which files would be copied instead of copying. Useful to check everything works as expected before starting a big copy
`--exclude`  | exclude the given files from copying, useful for logfiles you might not need
`--delete`   | delete everything in `localfolder` that is not in the source folder on the server. This is great to keep an exact copy but can be **dangerous** as files created locally might get lost

So to create an exact copy of a directory but excluding the logs subdirectory
we could use

```bash
rsync -vaz --exclude=logs --delete server:/folder/ localfolder
```

## Editing files over SSH

There are multiple ways to show files on a system connected to by ssh as if they were
local files. For example

* there is `sshfs` which lets you connect the files on a remote machine via the
  command line. Once it's installed you can just run
  `sshfs [user@]hostname:[directory] mountpoint`. There even is a windows version.
* on Linux when using Gnome in the file browser there is a "+ Other locations"
  in left pane at the bottom. This should bring up a "Connect to Server" field
  where you can enter any ssh host in the form `ssh://username@host/folder` and
  gnome will let you see the files on that host. See `here
  <https://help.gnome.org/users/gnome-help/stable/nautilus-connect.html.en>`_ for
  more information but this works similar in other desktop environments.
* In addition many editors or development environments have their own support to
  work on a remote machine via ssh. There is a
  `guide on confluence <https://confluence.desy.de/x/XGJ8Cg>`_
  explaining the setup for some of them.

## SSH multiplexing

ssh allows us to have multiple sessions over the same connection: You connect
once and all subsequent connections go over the same connection. This can speed
up connection times and also reduces password prompts if key based
authentication doesn't work. All we have to do is put the following in the
configuration file

```
ControlMaster auto
ControlPath ~/.ssh/%r@%h:%p.control
ControlPersist 30m
```

And when connecting ssh will automatically create a control path that can be
used by other connections and will keep the connection alive for 30m after we
closed the last ssh session.

This also allows to add port forwards to an existing connection: once you are
connected to a server you can run `ssh -fNL localport:remotehost:remoteport
server` locally in a different terminal to add a port forwarding.

If you really want to close the connection to a server you will have to run
`ssh -O exit server` and ssh will close the channel completely.

## sshuttle for advanced forwarding

There is an additional tool called [sshuttle](https://sshuttle.readthedocs.io/en/stable/).
You can only run it on machines
where you have administration privileges but then it allows to use ssh to
transparently connect your whole laptop to the network. This is then basically
identical to a VPN connection.

<!-- ## Location aware SSH config file

Sometimes you want to have your SSH config depend on where you are with your
laptop. For example, while at KEK you don't need to jump through the gateway.
This is indeed possible and explained in detail on `B2 Questions
<https://questions.belle2.org/question/1247/sshconfig-dependent-on-network/>`_. -->

## Using a terminal multiplexer (e.g. tmux, screen)

When you loose your ssh connection or your terminal window is closed, all
processes that had been running in that terminal are also killed. This can
be frustrating if it is a long-running process such compilation or a dataset
download.

To avoid this you can use a [terminal multiplexer](https://en.wikipedia.org/wiki/Terminal_multiplexer>)
program such as [GNU screen](https://www.gnu.org/software/screen/) or the newer and more
feature-rich [tmux](https://github.com/tmux/tmux/wiki).
Both are
pre-installed on KEKCC and NAF.

> ## Hint
> For computational jobs like processing a steering file, use a
> batch submission system instead (see :ref:`this warning <batch system recommendation warning>`).
{: .callout}

These programs create one or multiple new terminal sessions within your
terminal, and these sessions run independently of the original terminal, even
if it exits. You just have to re-connect and re-attach the old screen or tmux
session. A terminal multiplexer allows for example to

* Start a process (e.g. a download or compilation) on your work computer, log
  out (which detaches the session) go home and from your home desktop /
  notebook re-attach the session and check how your process is doing.

* Having multiple interactive shells in parallel on a remote host, without
  needing to open multiple terminals and connecting from each one separately.
  Think of it like having multiple remote "tabs". Tmux can also act as a
  terminal window manager and arrange them side-by-side. This can be useful e.g.
  for running a process in one pane and monitoring the processor load via
  [htop](<https://htop.dev/)

<figure>
<img src="{{site.baseurl}}/fig/tmux_on_kekcc.png"/>
<figcaption>
  Tmux running in the local terminal and on KEKCC with multiple windows and
  panes.
</figcaption>
</figure>

If you don't know either programs yet: learn how to use (the newer) tmux.
Check out the official [getting started guide](https://github.com/tmux/tmux/wiki/Getting-Started)
 from its [wiki](https://github.com/tmux/tmux/wiki)
 or one of the various googable online
guides such as [this one](https://linuxhandbook.com/tmux/). And I also
recommend keeping a [cheat sheet](https://tmuxcheatsheet.com) in your
bookmarks. The commands that you need for the most basic use-case are

* `tmux new-session`: Creates and attaches a new tmux session. This is also the default behaviour
    when just entering the `tmux`. However, the `new-session` subcommand
    allows for additional options such as naming sessions (see `tmux
    new-session --help`).
* `tmux kill-session` Kills the current tmux session. Use this when you finish your work and
    don't require your session anymore, it is a polite thing to do on shared
    resources like login nodes.
* `tmux detach` Detaches the current tmux session, so that you return your original
    terminal session, but the tmux session keeps running in the background.
    This happens automatically when you loose your connection or your terminal
    is closed.
* `tmux attach`. Short form: `tmux a`.
    Attaches a running but detached tmux session. When you log into a cluster
    like KEKCC or NAF to attach your previous tmux session, make sure are on
    **exactly the same host** as the one in which you started
    the initial tmux session.

All these commands take optional arguments to be able to handle multiple
sessions and there are many more useful tmux commands than those listed here,
for example if you want to have multiple windows (tmux "tabs") and panes in a
tmux. To see those, check out the documentation links above, where you will
also find keyboard shortcuts for most of them.

<!-- .. admonition:: Question
   :class: exercise stacked

   Why should I keep track of the exact host on which the terminal multiplexer
   is run and how do I do that?

.. admonition:: Hint
   :class: toggle xhint stacked

   Check out the output of the `hostname` command in a computing cluster like
   KEKCC. Why is it different from the hostname that you used to login (the
   `Hostname` line in your :ref:`ssh config <online_book/prerequisites/ssh:SSH
   Configuration File>`)? Could you have found out the host name without typing
   any commands? How can you change the specific host?

.. admonition:: Solution
   :class: toggle solution

   When you connect to a computing cluster like KEKCC via a login node, e.g.
   `login.cc.kek.jp`, you are connected to a random host (also called "node",
   i.e. an individual server) in that cluster for load-balancing purposes.
   You can check the full host name with the `hostname` command. But you
   can also see the first part of the hostname (the current node)
   in your shell prompt (the string at the beginning of the command line).

   If you disconnect and reconnect to the login node, you can be connected to a
   different node, but your terminal multiplexer will still be running on the
   old host, so you will have to connect to that specific host which it is
   running on. From within the computing cluster, you can usually just use the
   node name for the ssh connection. For example, if your tmux session is
   running on `ccw01.cc.kek.jp`, but you have been connected to `ccw02`,
   from there you can simply use

   .. code-block:: bash

       ssh ccw01

   to connect to the other node. Alternatively, you
   can directly connect to a specific host instead of the login node, but for
   that you might need to extend your :ref:`ssh config
   <online_book/prerequisites/ssh:SSH Configuration File>` to also use a
   gateway server for the specific nodes in the cluster, e.g. for the KEKCC:

   .. literalinclude:: ssh_config.txt
      :lines: 31-35
      :linenos:

   Then `ssh ccw01` will also work from outside KEKCC. -->
