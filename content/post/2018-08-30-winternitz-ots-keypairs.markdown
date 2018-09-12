---
author: admin
categories:
- Blockchains
date: 2018-08-30T00:00:00Z
layouts: post
title: Winternitz OTS keypairs
---

WOTS is a way of generating a public/private keypair, and using it for signing messages. In other words, it's a signature scheme. Importantly, it only depends on having a good hash function, which makes it 'quantum resistant' because Shor's algorithm can make cracking elliptic curves easier on quantum computers (????). XMSS uses a tweaked version called WOTS+, which improves some cryptographical aspects which I don't quite understand. A lot of what I learnt came from this page at [Cryptography Services](https://cryptoservices.github.io/quantum/2015/12/04/one-time-signatures.html).

Suppose we have a message '1234'. Let's sign it with WOTS to prove that we created/sent this message.

1. Generate a Secret Key

    Since our message is 4 characters long, we need to generate 4 random collections of bits (let's call them words, because calling them bytes would imply they're 8 bits long, which doesn't have to be the case). So let's say these words should be 6 bits long.

    `secretkey = ['011001', '010110', '100001', '001000']`

2. Calculate the Public Key from the Secret Key

    There are many hash functions out there, like SHA-1, SHA-2, SHA-3, Whirlpool, and the one everybody knows from Bittorrent, MD5.

    The idea is you hash each word in the secretkey /n/ times, so let's say /n=8/.
    
    `publickey = [sha2(sha2(sha2(sha2...('011001')))), sha2(sha2(sha2(sha2...('010110')))) ... ]`

    It is now quite impossible to calculate the secret key from the public key. It's impossible for 1 iteration of SHA2, let alone 8.

3. Signing the message

    ```
    The signature of '1' is sha2('011001')
    The signature of '2' is sha2(sha2('010110'))
    The signature of '3' is sha2(sha2(sha2('100001')))
    The signature of '4' is sha2(sha2(sha2(sha2('001000'))))
    ```

    Once you release your public key, everyone can see that if you hash sha2('011001') 7 more times, and sha2(sha2('010110')) 6 more times etc., they will have calculated your public key, and thus you have proved that you were the one who actually signed the message '1234'.

    Obviously, don't use this method to sign '2345'! Or any other message. It's called a OTS because it's a One Time Signature. Generate a new secret key each time you want to sign something.

WOTS+

The secret key now includes a random XOR bitmask for each word.

```
secretkey = [('011001', '111111'), ('010110', '001001'), ('100001', '000000'), ('001000', '101010')]
```
    

When calculating the public key/signature, after calculating the sha2(word), you XOR the result with the word's secret bitmask. This is supposed to make signature sizes smaller/harder to crack (don't ask me).

