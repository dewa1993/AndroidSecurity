# ANDROID SECURITY 
### test md file 


In overall encryption works as following:
You have plain data, that could be some sensitive information
Then, basing on some algorithm, you will create a special key and will use it to create cipher data.

For example, a simple algorithm — change every symbol in word with something. And a key — something is equal to next symbol from alphabet:
''' Algorithm: change every symbol in word with something
Key: something is equal to next symbol from alphabet
Plain Data (Input): "Hello World"
Cipher Data (Output): "ifmmp xpsme" 
Symbol Mask:
h -> i
e -> f
l -> m
o -> p
w -> x
r -> s
d -> e '''

Algorithm Types


Symmetric — The encryption key and the decryption key are the same. Also it is generally categorized as being either Stream Cipher or Block cipher.
The most common Symmetric AES — the Advanced Encryption Standard (AES)

Asymmetric — a modern branch of cryptography. Also known as public-key cryptography in which the algorithms employ a pair of keys (a public key and a private key) and use a different component of the pair for different steps of the algorithm.

Stream cipher — a symmetric encryption algorithm that processes the data a bit or a byte at a time with a key resulting in a randomized cipher data or plain data.

Block cipher — deterministic algorithm operating on fixed-length groups of bits, called blocks. Block ciphers are important elementary components in the design of many cryptographic protocols, and are widely used to implement encryption of bulk data.

Modes & Paddings

Block cipher has different Modes and Paddings that increases it protection level.
Modes — a mode of operation describes how to repeatedly apply a cipher’s single-block operation to securely transform amounts of data larger than a block.
Padding — block cipher works on units of a fixed size (known as a block size), but messages come in a variety of lengths. So some modes (namely ECB and CBC) require that the final block be padded before encryption.
Most common modes are :
ECB — Electronic Codebook, the simplest of the encryption modes. The message is divided into blocks, and each block is encrypted separately.
CBC — Cipher Block Chaining, each cipher data block depends on all plain data blocks processed up to that point. To make each message unique, an initialization vector must be used in the first block.
But simply because algorithm is not symmetric does not mean it can not have modes and paddings. Thats, for instance, RSA algorithm can be used with ECB mode and PKCS1Padding.


Key Types
There are three key types: Secret key, Private key and Public key.
Secret key — a single secret key which is used in conventional symmetric encryption to encrypt and decrypt a message.
Private key — the secret component of a pair of cryptographic keys used for decryption in asymmetric cryptography.
Public key — The public component of a pair of cryptographic keys used for encryption in asymmetric cryptography.
Together Public and Private keys forms a public-private cryptographic Key Pair.

Java Cryptography Architecture
Android builds on the Java Cryptography Architecture (JCA), that provides API for digital signatures, certificates, encryption, keys generation and management.

KeyGenerator — provides the public API for generating symmetric cryptographic keys.
KeyPairGenerator — an engine class which is capable of generating a private key and its related public key utilizing the algorithm it was initialized with.
SecretKey — a secret (symmetric) key. The purpose of this interface is to group (and provide type safety for) all secret key interfaces (e.g., SecretKeySpec).
PrivateKey — a private (asymmetric) key. The purpose of this interface is to group (and provide type safety for) all private key interfaces(e.g., RSAPrivateKey).
PublicKey — a public key. This interface contains no methods or constants. It merely serves to group (and provide type safety for) all public key interfaces(e.g., RSAPublicKey).
KeyPair — this class is a simple holder for a key pair (a public key and a private key). It does not enforce any security, and, when initialized, should be treated like a PrivateKey.
SecureRandom — generates cryptographically secure pseudo-random numbers. It is widely used inside of KeyGenerator, KeyPairGenerator components and Keys implementations.
KeyStore — database with a well secured mechanism of data protection, that is used to save, get and remove keys. Requires entrance password and passwords for each of the keys. In other words it is protected file that you need to create, read and update (with provided API).
Certificate — certificate used to validate and save asymmetric keys.
Cipher — provides access to implementations of cryptographic ciphers for encryption, decryption, wrapping, unwrapping and signing.
Provider — defines a set of extensible implementations, independent API’s. Providers are the groups of different Algorithms or their customizations. There are 3d party providers, such as Bouncy Castle and Spongy Castle (android version of Bouncy Castle),


Android Key Store
And in 18 version of Android, AndroidKeyStore was introduced:
The Android Key Store system lets you store cryptographic keys in a container to make it more difficult to extract from the device.
Once keys are in the key store, they can be used for cryptographic operations with the key material remaining non-exportable.
Moreover, it offers facilities to restrict when and how keys can be used, such as requiring user authentication for key use or restricting keys to be used only in certain cryptographic modes.

AndroidKeyStore is a JCA Provider implementation, where:
No KeyStore passwords is required (really, at all)
Key material never enters the application process
Key material may be bound to the secure hardware (Trust Zone)
Asymmetric keys are available from 18 +
Symmetric keys are available from 23 +
With out it, before Android 18, you would need to create a file somewhere in your local or external device storage, where all of your keys would be kept (as you can understand, it is easy to extract that file). But with AndroidKeyStore you do not need to create anything, system will manage everything for you:

If device manufacture supports Trusted Execution Environment(TEE), your keys will be saved there (the most secure option);
If device manufacture doesn’t support TEE, keys will be stored in emulated software environment, provided by the system.

In both cases, your keys will be automatically removed from the system after deleting the application. Also keys material is never exposed