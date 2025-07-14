
---
title: "Introduction"
teaching: 0
exercises: 0
questions:
- What is SSH?
- What are the needed requirements to begin?
objectives:
- Understanding how SSH can be used.
- Know the necessary requirements to begin SSH protocol.
keypoints:
- "SSH is a way to connect your computer remotely to another computer so you can enter commands into a terminal."
- "Different computers may have different requirements to enact the SSH protocol."
---

Secure Shell (SSH) is a protocol to access other computers. SSH allows you to connect your computer remotely to another computer to enter commands into a terminal. In the simplest terms, you will just
type the name of the computer you want to connect to:

```bash
ssh username@servername.domain
```

--------------------



To begin this process, you will need to make sure that you have a ssh endpoint. This means that you need an account on the server you are connecting to which will then give you a server name.

To do this, you will need to get in contact with the person within your department/company that is responsible for adding new users to the desired server.


> ## Note
> Doing this process with instructions from your computer system, is the best way to ensure a smooth ssh connection.
> These Instructions should tell you if there are any additional requirements for connecting, like a VPN, two-Factor Authentication, or Kerberos.







Below is a list of common additional requirements past your username and password. 

## VPN Requirements 

A Virtual Private Network (VPN) is a secure connection that encrypts your internet traffic and masks your ip address to allow you to access the internet as if you were on a private network. This is used to enhance the privacy and security of your network.

The instructions for the computer you are connecting to will say if you need a VPN and they will link to instructions for setting up the VPN appropriately. 


## Two Factor Authentication

This is a security method that requires two seperate forms of identification to access an account or system. In this case, we will be using it to access a remote computer. The two parts of this authentication process include a password/pin and a hardware token or authentication app via a smartphone or another device.

To perform the authentication, you will begin by using your login like normal. Then you will be prompted to provide a second level of authentication. This will come in either the form of a code or set of numbers that appear on the authenticator app, or a button to be pressed on another device (email, text message, phone call, etc.)

Once the authentication process has been completed, your login will be validated and you should be given access.

## Kerberos 

Some institutions, like Fermilab, use Kerberos. See the [Kerberos](06-Kerberos.md) section if you are connecting to a Fermilab server.
