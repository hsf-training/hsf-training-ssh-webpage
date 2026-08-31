# Setup

# Overview

Before starting this tutorial, you’ll need to set up **SSH (Secure Shell)** on your computer or in GitHub Codespaces.
SSH lets you securely connect to another machine, execute commands remotely, and transfer files.

You will learn how to:

- generate your first SSH key pair
- test SSH connections (Codespaces or local machine)
- understand how SSH works
- optionally connect to lxplus or Fermilab LPC

This setup ensures you’re ready for the hands-on SSH lessons that follow.

---

# 1. Generate an SSH Key

SSH uses **asymmetric cryptography** — a *public key* you can share and a *private key* that must remain secret.

Create your key pair:

```bash
ssh-keygen -t ed25519 -C "your_email@example.com"
````

If `ed25519` is not available (for example, if you have an old OS or OpenSSH version):

```bash
ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
```

Your keys will be created in:

```
~/.ssh/id_ed25519      # private key (keep safe!)
~/.ssh/id_ed25519.pub  # public key (share with servers)
```

---

# 2. Test an SSH Connection

Now it's time to set up your actual SSH connection.

## Option 1: Direct Sign In

If you have access to a remote server (LXPLUS at CERN, KEKCC at KEK, etc.), you can sign in there for this training.
The general form for an SSH sign in is:

```bash
ssh username@servername
```

Example for CERN:

```bash
ssh username@lxplus.cern.ch
```

On first connection:

```
Are you sure you want to continue connecting (yes/no)? yes
```

If you are a CERN or Fermilab user, instructions on how to connect to those remote servers are given in the Addendum

---

## Option 2: Using SSH Inside GitHub Codespaces (Highly Recommended)

GitHub Codespaces provides a complete Linux environment in the cloud that already includes the **SSH client**.
We have set up a Codespace in which you can connect to a mock ssh server if you lack access to a remote server.

---

### Step 1 — Open Codespace

Click the button below to open a Codspace, then click ``Create codespace``.

[![Open in GitHub Codespaces](https://github.com/codespaces/badge.svg)](https://codespaces.new/hsf-training/hsf-training-ssh-webpage)


---

### Step 2 — Connect to SSH server

Once you are in the Codespace, you must connect to the mock server:

```
ssh hsf-user@hsf-training.org
```

The password for this connection is ``hsf-password``.

### Step 3 — Verify You’re Inside the SSH Session

Inside the session, run:

```bash
echo "USER: $(whoami)"
echo "HOST: $(hostname)"
echo "SSH:  $SSH_CONNECTION"
pwd
```

Expected:

```
USER: codespace
HOST: <codespaces-ID>
SSH: <connection info>
/config
```

To exit, run:

```bash
exit
```

Expected:

```Connection to hsf-training.org closed.```

---

## Option 3: Using SSH Locally (macOS, Linux, Windows)

If you’re not using Codespaces, you can still practice SSH using **localhost** (`ssh into your own machine`).

---

### macOS

Enable:

```
sudo systemsetup -setremotelogin on
```

Connect:

```
ssh $(whoami)@localhost
```

Disable (optional):

```
sudo systemsetup -setremotelogin off
```

---

### Linux

Install & start:

```bash
sudo apt install openssh-server
sudo systemctl enable ssh
sudo systemctl start ssh
```

Test:

```
ssh $(whoami)@localhost
```

---

### Windows (PowerShell)

1. Install *OpenSSH Client* and *OpenSSH Server* in
   **Settings → Apps → Optional Features**
2. Start service:

```powershell
Start-Service sshd
Set-Service -Name sshd -StartupType Automatic
```

SSH into yourself:

```powershell
ssh localhost
```

---

# 3. Common Issues and Fixes

| Problem                            | Solution                                             |
| ---------------------------------- | ---------------------------------------------------- |
| Permission denied (publickey)      | Check key permissions, upload your key to the server |
| Host key verification failed       | `ssh-keygen -R servername`                           |
| Connection timed out               | Ensure internet/VPN is working                       |
| Key not found                      | `chmod 600 ~/.ssh/id_ed25519`                        |

---

# Addendum: Connecting to CERN or Fermilab


These steps apply only if you belong to CMS or another collaboration.

### CERN (lxplus)

* Request account:
  [https://twiki.cern.ch/twiki/bin/view/CMSPublic/WorkBookGetAccount](https://twiki.cern.ch/twiki/bin/view/CMSPublic/WorkBookGetAccount)
* Connect:

  ```bash
  ssh username@lxplus.cern.ch
  ```

### Fermilab LPC (US-CMS)

* Request account:
  [http://www.uscms.org/uscms_at_work/computing/getstarted/getaccount_fermilab.shtml](http://www.uscms.org/uscms_at_work/computing/getstarted/getaccount_fermilab.shtml)
* Connect:

  ```bash
  ssh username@cmslpc-el8.fnal.gov
  ```

---

