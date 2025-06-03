
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

Secure Shell (SSH) is a protocol to access other computers. It was invented in
1995 to make the old methods more secure but, on first glance, it still behaves
similar to remote shell or telnet from the eighties: SSH allows you to connect your computer remotely to another computer to enter commands into a terminal. In the simplest terms, you will just
type the name of the computer you want to connect to:

```bash
ssh username@servername.domain
```

--------------------



To begin this process, you will need to make sure that you have a ssh endpoint. This means that you need an account on the server you are connecting to which will then give you a server name. 

To do this, you will need to get in contact with the person within your department/company that is responsible for adding new users to the desired server. 


> ## Note
> Doing this process in person with this designated person is the best way to ensure a smooth ssh process later on. 
> It is also reccomended you check any VPN requirements that your institution may have at the same time. (Directions below.)






Some computers may require you to use a specific VPN if you are not within the geographical area set upon the computer.

VPN Requirements
---------------------

A VPN also known as a Virtual Private Network, is a secure connection that encrypts your internet traffic and masks your ip address to allow you to access the internet as if you were on a private network. This is used to essentially enhance the privacy and security of your network while you browse.

To install a VPN you may have to apply for access or install the available vpn to your computer.

You should be able to find information and instructions to install said VPN on the resources page of your University of company.

Some computers may need two-factor authentication to be used to access the server. 

Two Factor Authentication
----------------------
This is a security method that requires two seperate forms of identification to access an accountant or system. In this case, we will be using it to access a server. 
The two parts of this authentication process include, a password/pin, and a hardware token or authentication app via a smartphone or another device. 

To perform the authentication, you will begin by using your login like normal. Then you will be prompted to provide a second version of a password. This will come in either the form of a code or set of numbers that appear on the authenticator app, or a physical button to be pressed via another form (email,text message, phone call, etc.)
Once the authentication process has been completed, your login will be validated and you should be given access. 

>## Note for Fermilab
> Fermilab uses Kerberos. See Kerberos section if you are connecting to a Fermilab server.

