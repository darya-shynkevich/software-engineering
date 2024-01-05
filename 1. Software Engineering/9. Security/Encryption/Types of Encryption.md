Encryption involves ==converting human-readable plaintext into incomprehensible text, which is known as ciphertext and decrypting it back to plaintext again==.

*There are two classes of encryption, but until 1976 **[Symmetric Key Encryption](Symmetric%20Key%20Encryption)** was the only show in town. It involves a shared key used to encrypt and decrypt messages.

*The main problem with symmetric key encryption is **sharing the shared key securely**. Since a third party with the key could also decrypt the message if they got hold of the shared key.*

![Pasted image 20230605224230](../../../_Attachments/Pasted%20image%2020230605224230.png)
Commonly used symmetric encryption algorithms include:
- AES
- 3-DES
- SNOW

***[Asymmetric Encryption](Asymmetric%20Encryption)** uses a mathematically related pair of keys for encryption and decryption: a public key and a private key.*

![Pasted image 20230605224333](../../../_Attachments/Pasted%20image%2020230605224333.png)

*You can encrypt data with either key, but **only its pair can decrypt it**. The main benefit of this type of encryption is that **you can keep your private key entirely private and publicly share the public key, which people can use to encrypt data for you**.*

Commonly used asymmetric encryption algorithms include:
- RSA
- Elliptic curve cryptography

References:
1. https://architecturenotes.co/types-of-encryption/
2. [The Hardest Thing About Data Encryption](https://developer.okta.com/blog/2019/07/25/the-hardest-thing-about-data-encryption)
3. [Symmetrical vs asymmetrical Encryption Pros and Cons by Example](https://www.youtube.com/watch?v=Z3FwixsBE94&list=PLQnljOFTspQXOkIpdwjsMlVqkIffdqZ2K&index=56) (video)
4. [What happens when your Web Server Private Key is Leaked?](https://www.youtube.com/watch?v=ckEIQdGJ1tc&list=PLQnljOFTspQUybacGRk1b_p13dgI-SmcZ&index=34) (video)
5. [Diffie-Hellman key exchange Algorithm](https://vishalrana9915.medium.com/diffie-hellman-key-exchange-algorithm-cc91adff9b0f)
6. [Public Key Infrastructure (PKI)](https://blog.devgenius.io/public-key-infrastructure-pki-d0d513ba8819)