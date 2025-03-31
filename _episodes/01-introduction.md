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
similar to remote shell or telnet from the eighties: SSH allows you to connect your computer remotely to another computer to enter commands into a terminal. In the simplest case, you will just
type the name of the computer you want to connect to:

```bash
ssh username@servername.domain
```

However, some computers may require you to use a specific VPN if you are not within the geographical area set upon the computer. 

VPN Requirements 
---------------------

A VPN also known as a Virtual Private Network, is a secure connection that encrypts your internet traffic and masks your ip address to allow you to access the internet as if you were on a private network. This is used to essentially enhance the privacy and security of your network while you browse. 

To install a VPN you may have to apply for access or install the available vpn to your computer. 

You should be able to find information and instructions to install said VPN on the resources page of your University of company.
