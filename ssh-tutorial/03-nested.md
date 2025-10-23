# Nested SSH Connections (Optional)

```{admonition} Learning Objectives
- Jump to another host machine within a ssh connection
```

You can use ssh inside a ssh connection to "jump" from machine to machine.
This sometimes is necessary as not all computers can be connected to directly
from the internet. For example, the login node for the KEK computing center
cannot be accessed directly if your are not in the KEK network or using a VPN. The
KEK network is rather complex so a very simplified layout is shown below.

```{note}
It is possible for your home institute to get direct access to a KEK network so
you might not be affected by this restriction while at work.
```

So, unless you are using VPN or are at KEK, you most likely need to connect to the
gateway servers first, either `sshcc1.kek.jp` or `sshcc2.kek.jp`

```bash
ssh username@sshcc1.kek.jp
```

```{warning}
Your username on KEKCC is not necessarily the same as your DESY username.
```

and once this connection is established you can login to KEKCC from this gateway
server.

```bash
ssh username@login.cc.kek.jp
```

```{warning}
* This second step needs to be done in the terminal connected to the gateway
  server
* The initial password for `login.cc.kek.jp` is not the same as that of
  `sshcc1.kek.jp`.
```

Now, you should be connected to KEKCC but you needed to enter two commands. And
trying to copy files from KEKCC to your home machine becomes very complicated as
you would have copy them in multiple steps as well so, this is clearly not yet
the optimal solution.

```{admonition} Key Points
:class: tip
- You can jump between hosts by executing ssh within a ssh connection
```
