---
title: "Kerberos-based login"
teaching: 0
exercises: 0
questions:
- "What is Kerberos?"
- "How does Kerberos work?"
- "Why do we use Kerberos?"
- "How do I get a krb5 ticket?"
- "How do I make my ssh config file use the krb5 ticket?"
objectives:
- "Obtain a krb5 ticket"
- "Make a ssh config file that uses the krb5 ticket"
- "Use the krb5 ticket to ssh into a remote machine"
keypoints:
- "Know how Keberos works"
- "Know how to get a krb5 ticket"
- "Know how to use the krb5 ticket to ssh into a remote machine"
- "Know how to make a ssh config file that uses the krb5 ticket"
---

 > ## What is Keberos?
 > Keberos is a network authentication protocol. It is designed to provide
 a secure identification verification for more insecure networks.
 > Keberos was developed by MIT in the 1980s as part of Project Athena.
 > It is based on symmetric key cryptography and uses a thurd party called
 the key distribution center (KDC) to authenticate users and services.
 > This is a commonly used in a single sign-on (SSO) systems, but we will
 go over macOS, Windows, and Linux systems.
 ---

---
## How does Kerberos work?
Keberos works by using a tickets system. It begins by having a user
login with kinit. The user starts by sending their username to the
Authentication Server (AS) in the KDC. The AS then sends back a
ticket-granting ticket (TGT) and a session key. The Ticket-Granting Ticket
secret key is usually derived from the user's password.

The user then uses the TGT to access the SSH protocol. The client
will use the TGT to request a service ticket from the TGS (or KDC).
The TGS will then return with a service ticket which will be encrypted
and will be valid for the desrired service.

To connect, the client will the present the service ticket and the
server will decrypt and verify with its own encrypted key. If the server
is able to verify the ticket, the user will be authenticated without
sending a second encrypted key.
---

> ## Why do we use Kerberos?
 > Keberos is used to provide a secure strong encryption. Because there
  are no passwords sent over the general networks, so it is much more secure
  then other authentication methods.

---
> ## How do I get a kbr5 ticket?
 To accquire a kbr5 ticket you will need to use the following process:
 1. Open a terminal
 We will start by using kinit to obtain the keberos ticket.
 2. Run this commnd:

 ```bash
 kinit <your_username@YOUR.REALM.COM>
 ```
 > 3. Once you run this command, it will then prompt you to enter your
 keberos password. Once you are successful in entering the password, a
 ticket granting ticket will be stored in your credential cache.
 > 4. We will now check the ticket. This will show your principal username,
 your ticket experiration time, and the ticket cache location.

 ```bash
 <klist>
 ```

 > Now that you have a kbr5 ticket, we will ensure SSH uses the keberos
 ticket by enabling keberos in your client SSH config file.

 ```bash
 ssh
  Host*
  GSSAPIAuthentication yes
   GSSAPIDelegateCredentials yes
 ```
 > This will ensure that SSH uses the keberos ticket to authenticate
 you to the remote machine.

 > 5. Now you can SSH into the remote machine using the kbr5 ticket like normal.

 ```bash
 ssh <your_username@server.domain.com>
 ```
---


## Note
If the kbr5 ticket has been correctly configured, it will not prompt
you for a password. If it does, please recheck the configuration.

---

> ## How to make the SSH config file use the kbr5 ticket

> For Linux:
> 1. To use the kbr5 tickrt, you will need to install the required packages.

```bash
sudo apt install krb5-user openssh-client
```
for RHEL/Fedora:

```bash
sudo dnf install krb5-workstation openssh-clients
```
> 2. Next, you will add:
```bash
  Host *
    GSSAPIAuthentication yes
    GSSAPIDelegateCredentials yes
```
> 3. you will then need to get the kbr5 ticket once more:

```bash
kinit <your_username@YOUR.REALM.COM>
klist  "# to verify"
```
> 4. You can now SSH into the remote server using the kbr5 ticket:

```bash
ssh <your_username@your.server.com>
```

> For Windows OpenSSH + MIT Keberos:
> 1. You will need to begin by installing MIT Kerberos for Windows.
> 2. You will then configure
```bash
kbr5.ini in c:\ProgramData\MIT\Kerberos\kbr5.ini
```
Then run

```bash
ssh <kinit your_username@YOUR.REALM.COM>
```
> 5. You will then need to enable GSSAPI in SSH by editing or creating
C:\Users\YourName\.ssh\config

```bash
ssh
Host *
    GSSAPIAuthentication yes
    GSSAPIDelegateCredentials yes
```
> 6. You can now SSH into the remote server using the kbr5 ticket:

```bash
ssh your_username@your.server.com
```

> For MacOS:
> 1. Make sure you have Beberos and SSH already installed. Then run:

```bash
kinit your_username@YOUR.REALM.COM
```
> 2. Now we will configure the SSH Client:

```bash
Edit ~/.ssh/config:
nano ~/.ssh/config
```

> 3. Now we can add the following lines:

```bash
Host *
    GSSAPIAuthentication yes
    GSSAPIDelegateCredentials yes
```
> 4. Our last step is to connect to the server as normal:

```bash
ssh your_username@your.server.com
```
---

## Technical and Security Considerations
 - Keberos is very time sensitive and time sychronization is mandatory.
  > All client and server systems must synch to the same time source.
  This can be done via ntpd or systemd-timesyncd
- These tickets have a lifetime. Default Ticket Granting Ticket (TGT) are usually 10-24 hours
 > To see the ticket's lifetime duration:

 ```bash
  klist -v
```

 > To request renewable tickets:

```bash
kinit -r 7d your_username@REALM
```
- The SSH server must have a keytab file with a host/FQDN@REALM principal. Without this, the SSH server will be unable to decrypt Kerberos Tickets and the authentication will fail. This is located at:

```bash
/etc/krb5.keytab
```
- It is also important to note that Kerberos is not a replacement for SSH Keys
 > Kerberos and SSH is great for enterprise enviorments or SSO setups. For remote access to personal servers, SSH key pairs may still be simpler to manage.

 - Kerberos Forwarding and Delegation:
  > Using GSSAPIDelegateCredentials yes forwards your Kerberos ticket to the remote server. This allows SSH hopping which is the process in which you SSH from one server to another without re-authenticating. It is important to note however that you should only enable delegation if you trust the server. If the server is not trustworthy, the ticket could be stolen.

- You can also Test Verbosely for debugging by using verbose mode to diagnose problems:

 > ```bash
 ssh -vvv user@host
 ```
 We are specifically looking for any GSSAPIAuthentication attempts, ticket errors, and any time or DNS issues.
 Since were talking about DNS issues, theres a few more important notes to include. Kerberos heavily relies on DNS for hostname resolution. If your server is accessed via a hostname that doesn't match the principal like host/fqdn@REALM, then the authentication may silently fail.

- The final important consideration to make is regarding Firewalls and Ports. SSH uses port 22, but Kerberos itself uses UDP/88 (KDC) and TCP/88 (fallback). So it is essential to ensure that these are open between client and KDC. This is not needed for SSH connection itself however.
