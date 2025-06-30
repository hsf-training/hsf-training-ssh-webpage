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

> ## Excercise 
> To establish the identity of Alice, Bob gives Alice a message to encrypt with her private key. Bob can then decrypt it with his public key to be sure he talks to Alice because only she would be have been able to encrypt the message to begin with.
> This is essentially what you are using everywhere on the internet; Every https connection on the internet uses this principle to make sure that the server that is claiming to be let's say, "netflix.com" is actually Netflix's server. This is also what SSH uses. 
> Following this example, consider a string of numbers `1435706`. Now for encryption, we could just add a value to each of these digits and take the modulus by 10 get get another digit,
> $$f_{k}(x) = (x + k) \bmod 10$$
 Now if we choose keys such as $$A=3$$ and $$B=7$$, then we can encrypt the message by applying
 $$f_A(x)$$ on each digit to get the encrypted message, `4768039`. Now we can decrypt it again by applying  $$f_B(x)$$ to get the original message, 
 `1435706`. This also works by first applying 
 $$f_B(x)$$ and then $$f_A(x)$$.
 
 So, if we encrypt a message with one key, we need to use the other key to decrypt the message again and vice versa. 
 Despite the simplicity of this example, it is rather close to the [RSAalgorithm] (https://en.wikipedia.org/wiki/RSA_(cryptosystem)) still used today.



