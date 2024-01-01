Encryption involves ==converting human-readable plaintext into incomprehensible text, which is known as ciphertext and decrypting it back to plaintext again==.

*There are two classes of encryption, but until 1976 **[Symmetric Key Encryption](Symmetric%20Key%20Encryption)** was the only show in town. It involves a shared key used to encrypt and decrypt messages.

*The main problem with symmetric key encryption is **sharing the shared key securely**. Since a third party with the key could also decrypt the message if they got hold of the shared key.*

![Pasted image 20230605224230](../../_Attachments/Pasted%20image%2020230605224230.png)
Commonly used symmetric encryption algorithms include:
- AES
- 3-DES
- SNOW

***[Asymmetric Encryption](Asymmetric%20Encryption)** uses a mathematically related pair of keys for encryption and decryption: a public key and a private key.*

![Pasted image 20230605224333](../../_Attachments/Pasted%20image%2020230605224333.png)

*You can encrypt data with either key, but **only its pair can decrypt it**. The main benefit of this type of encryption is that **you can keep your private key entirely private and publicly share the public key, which people can use to encrypt data for you**.*

Commonly used asymmetric encryption algorithms include:
- RSA
- Elliptic curve cryptography

References:
1. https://architecturenotes.co/types-of-encryption/