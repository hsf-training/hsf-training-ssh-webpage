---
title: "Key based authentication!"
teaching: 0
exercises: 0
questions:
- "How can I avoid typing very long passwords while still ensuring strong security?"
objectives:
- "Generate an SSH key and use it"
keypoints:
- "`ssh-keygen` can be used to create private/public key pairs for authentication"
- "`ssh-copy-id` can be used to copy the public key to the server to enable
  login via the private key"
- "which keys to use for which server can be configured in the configuration
  file"
- "Agent forwarding can be used to make your local private keys available on
  the server you connect."
- "`ssh-add` allows you to add, list or remove identities from the agent"
---

As you have seen there is a lot of entering your password, especially when
jumping between hosts. Time to take care of that. When we explained
asymmetric encryption, we mentioned that SSH can use it to authenticate you to a server. This is
usually safer and more convenient than using the password directly.


> ## Note
> Key based login doesn't work to all servers. The most notable exception for us
> is DESY as they have a different security system called kerberos which is
> incompatible with key-based login. However, for DESY, one can obtain a
> `kerberos token <https://confluence.desy.de/x/173UBw>`_ instead
> which will have almost the same effect.
{: .callout}


## Creating a key pair

First, we need to create a private/public key pair to be used for SSH, called an
identity. We want to do this on your **local machine** just by calling `ssh-keygen`. Without any options this will just
create a private/public key pair we can use but, you might at least give it a
comment string so that you can identify the key easier. For example, `-C` will provide a comment to your key.

```bash
ssh-keygen -C "Alice's Laptop"
```

After executing this command, it will prompt you for a file name. By default, it should offer
`.ssh/id_rsa` in your home directory. If you do not already have a key,
just choose this. This will then be your default ssh identity and, if you happen to have another one in the same directory, it will be used
by default. Otherwise, you can choose any filename you like and you can create as
many keys as you want, but then, you will have to manually tell ssh which one to use.
It is advisable to put them in the `.ssh` directory but in theory you can put them
anywhere.


> ## Warning!
> If you already have a key in your `.ssh` directory this might overwrite
> the existing key and cause problems on existing logins. Only overwrite the
> existing key if you are sure you don't need the old one.
{: .caution}

Next, it will ask you for a passphrase to protect the key. While technically you
can create keys without a passphrase, you should **never** do so as it would make
it easier for others to get access to your key. This passphrase is not
related to your account passwords and doesn't need to be changed, but you should
choose something safe, preferably a [sequence of random words](https://xkcd.com/936/).
Don't worry, you don't have to type it all the time.
After that it should just print some information on the key and, with that,
you have created your very own SSH identity.

> ## Question
> Where is the public key stored?
> > ## Hint
> > Check the output of `ssh-keygen`
> {: .solution}
> > ## Solution
> > In a file with the same name as the private key but `.pub` in the end.
> {: .solution}
{: .challenge}

> ## Question (optional)
> The default key type is to use "rsa". What types are possible for a key?
> > ## Hint
> > Try `man ssh-keygen` or, for more information, google "ssh key types".
> {: .solution}
> > ## Solution
> > There should be [RSA](https://en.wikipedia.org/wiki/RSA_(cryptosystem)),
> > [DSA](https://en.wikipedia.org/wiki/Digital_Signature_Algorithm),
> > [ECDSA](https://en.wikipedia.org/wiki/Elliptic_Curve_Digital_Signature_Algorithm>`)
> > and [Ed25519](https://en.wikipedia.org/wiki/Curve25519>).
> > However, DSA has been found unsafe and there are some concerns about ECDSA so,
> > the only real options are RSA and Ed25519. Ed25519 was added later and should
> > be more secure but it is not supported on very old versions of SSH.
> {: .solution}
{: .challenge}

## Using your new key

Now that you have a key, let's tell ssh to use it. If you chose the default name, ssh
will offer it to the remote server automatically. If you did not, or have other keys in the same directory,
you need to tell ssh which identity to use with the `-i` flag each time you run it. But, again, the best way
to do this is to add the key to the configuration file. You can even tell ssh to not try to
use the password at all but just the listed keys. For example:

{% include includeconfiglines filename='code/ssh_config.txt' start=24 stop=27 %}

But if the remote server doesn't know your identity, it will reject it. So we
need to give the public key to the remote server. This is very simple, all
private keys you want to be able to log into a server should have their public
keys in `.ssh/authorized_keys` on the server; just the contents of the
`.pub` files, one after another. And, there is a program available to create
this file. We just have to call it for each server we want to be able to login
with this key.

```bash
ssh-copy-id -i ~/.ssh/id_rsa <hostname1>
```

> ## Hint
> If you created the key in a different file you need to change the filename
> given with the `-i` parameters. You can also omit the `-i <identity>`
> option and ssh-copy-id will copy all public keys it can find.
{: .callout}

Once that is done, you should be able to login to the server with only the key
password. You should only be asked to enter the passphrase once or
maybe not at all if you already used the key recently, since, on most machines,
ssh will automatically remember the passphrase during the session.

> ## Hint
> If ssh does ask you for your passphrase every time you might need to check
> or configure your `ssh-agent`, the process that remembers the keys.
> Start the ssh-agent in the background:
> `bash
> eval "$(ssh-agent -s)"
> `
> Now, add your private key to the ssh-agent
> `bash
> ssh-add ~/path/to/key
> `
{: .callout}

You need to repeat these steps for all machines you work from, so your laptop
and your workstation, if you have both. Each machine you "own" should have its
own private/public key pair and these should be known to the servers you want to
log into.

## Making keys available on other machines

Finally, you usually don't want to have private keys present on systems you
don't really have control over for security reasons. But, even more important,
that would require us to keep track of too many key pairs. Usually, we avoid
having keys on servers like KEKCC. But sometimes, especially when using git, you
might need or want keys to be available on these machines as well.

The best thing to do here is "Agent forwarding": You tell ssh "please make the
keys I have on this machine available while I connect to another machine". This is
done very easily, either by adding `-A` to the ssh call or by adding
`ForwardAgent Yes` to the configuration file, either globally or on a per
host basis.

Now, after connecting, you should be able to use your keys as if you were working
on your local machine. You can also inspect the keys available: ssh keeps
identities in an "authentication agent" for easy use. They are usually added the
first time they are used and then kept during the session. You can inspect and
modify this list of keys with the command `ssh-add`.

> ## Note for OSX users
> On MacOSX you need to add the following lines to the configuration file to
> enable agent forwarding:
> ```
> UseKeychain yes
> AddKeysToAgent yes
> ```
> (see [this note](https://developer.apple.com/library/archive/technotes/tn2449/_index.html)
> for technical details)
{: .callout}
