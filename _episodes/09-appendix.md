---
title: "Appendix"
Teachhing: 0
Contents: 
- Asymmetric Encryption
- Making keys available on other machines
- Programs that open ports on the target machine

---

One cornerstone of technology that has been added to the internet is asymmetric cryptography. This is also referred to as public-key cryptography. We all use it constantly without noticing when we are browing the internet, but it's also important for the efficient usage of SSH. 

---
Key Features: 

The principal of asymmetric cryptography is to have two distinct different keys for encryption. One key is designed to encrypt the message and the other is to decrypt the message. In the following example, we will be keeping one of these keys private and publish the other (in practice we do the same.)

> ## Warning: Private means private!!!
> * While the public key can be shared freely, it is essential that the private key remains private. 
> * If someone gains access to your private key they can impersonate you in the digital world and get access to your resources.
{:.caution}

--- 

Example: 

Let's say we have two people, Alice and Bob, where Alice owns the private key and Bob has the public key. In this case, Bob can send confidential information only intended for Alice by encrypting it with the public key. Only Alice has the private key to decrypt it and nobody else would be able to see the content of the message. Then, with this key, pair, they can preform two things after checking confidentiality. 

* Bob can send confidential information only intended for Alice by encrypting it with the public key. 


