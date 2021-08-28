---
title: Encryption 
author: vwkd
index: 2
tags:
  - other
---
# Encryption 101



## Terminology

- safety: state of not being in danger
- security: state of being protected against danger
- privacy: state of not being observed
- security is necessary but not sufficient for privacy, i.e. `privacy => security` but not `privacy <=> security`
- confidentiality: state of being secret
- authenticity: state of being genuine
- integrity: state of being complete and unmodified
- encryption: conversion of a message into unintelligible form, plaintext to ciphertext
- decryption: conversion of an encrypted message into intelligible form, ciphertext to plaintext
- key: piece of information needed to de- / encrypt a message, beware: encryption key and decryption key are not necessarily the same
- cipher: mathematical algorithm that de- / encrypts a message, uses key
- symmetric encryption: same key is used for both encryption and decryption
- asymmetric encryption: different keys are used for encryption and decryption



## Encryption

- provides confidentiality of data
- used for security of data
- still needs to ensure the authenticity and integrity of data

### Symmetric Encryption

- same key is used for both encryption and decryption
- used to encrypt data at rest
- cipher is fast, computationally cheap, e.g. AES
- for data in transit sender and receiver need to somehow exchange the key first

### Asymmetric Encryption

- different keys are used for encryption and decryption
- used to encrypt data in transit
- made up of key pair, each key is the decryption key for messages encrypted with the other
- public key: one key is made public, e.g. on keyserver, everyone can encrypt a message
- private key: other key is kept private, only owner can decrypt a message encrypted with the public key
- cipher is slow, computationally expensive, e.g. DH, RSA
- for two-way communication, both parties have their private-public key pair, encrypt a message with the public key of the opposite party
- often used for a key exchange protocol to negotiate a symmetric key, then proceeds with faster symmetric encryption
- public keys are used for encryption and signature verification, private keys are used for decryption and signature, see Authentication

### Forward Secrecy

- property of an key exchange protocol
- use separate session keys for each communication session
- ensures that past communication sessions can not be decrypted if current session key is compromised
- perfect forward secrecy, if negotiation of session keys does not allow to reconstruct them, e.g. DH, but not RSA since session keys are directly transmitted
- perfect forward secrecy ensures that past communication sessions can not be decrypted even if private key is compromised
- beware: if private key is lost, future communication is not protected, adversary can imposter compromised party and continue communication with the other party ❗️



## Authentication

- provides authenticity and integrity of data
- transmits additional authentication for each message

### Message Authentication Code (MAC)

- cryptographic hash of the message and a key, e.g. the encryption key, but better a separate one
- used together with symmetric encryption
- if message is altered the MAC computed by receiver from the message doesn't match the sent MAC
- an adversary that altered a message can't switch out the sent MAC as well because he can't compute a correct MAC for the altered message without knowing the input key
- beware: doesn't protect against blocking or replay attacks, needs to add some sequence number to message, e.g. timestamp, ACK, etc.
- can not use as proof of sender identity, since verification MAC requires knowledge of input key, everybody that knows input key could have computed MAC

### Digital Signature

- cryptographic hash of the message, encrypted with the _private_ key
- used together with asymmetric encryption
- recipient can decrypt with senders public key, then compare the hash against the hash computed from the message he decrypted with his private key
- proves that sender is owner of public key, since only the _public_ key can decrypt messages encrypted with the _private_ key
- beware: doesn't protect against blocking or replay attacks, needs to add some sequence number to message, e.g. timestamp, ACK, etc.
- can also use as proof of sender identity, since verification of signature doesn't require knowledge of signature key, only sender knows signature key
- can also use as signature, an open message encrypted with senders private key, everyone can verify that public key decrypts it, proves that sender is owner of public key, e.g. Blockchain



## Identification

- provides authenticity of the communicating parties
- asymmetric encryption allows to communicate with unkown party, unlike with symmetric encryption where needed to first exchange a key
- asymmetric encryption guarantees confidentiality, authenticity and integrity of transmitted data
- but no guarantees of authenticity of receiver, the most perfect encryption wouldn't help
- receiver can be imposter, may not be who they claim to be, man-in-the-middle attack
- needs to verify identity of other party, needs to verify owner of a public key, or shorter hash of public key ("fingerprint")

### Personal Verification

- can verify public key yourself, over another medium, "out-of-band", e.g. in person, video chat, audio call, etc.
- better since no trusted third party, but difficult, doesn't scale, use for most important communication

### Third Party Verification

- can trust a third party to verify public key
- can be organisation, e.g. certificate authority (CA)
- can be already trusted people, e.g. web of trust
- "signs" public key and identity of owner, e.g. e-mail, domain name, etc., see [Digital Signature](#)
- issues certificate of signature, proof of identity



## Possible threads

- metadata, e.g. identities, time, quantity, location, etc.
- any participating party, e.g. steal private key, steal decrypted message, etc.
- third party that verfies public key, any CA in trust chain
- brute force attack on key
- random number generator, key is not totally random, ciphertext becomes predictable
- cipher, computation becomes cheap, e.g. prime factorization for RSA
- cryptographic hash function, hash becomes reversible



## Resources

- Wikipedia