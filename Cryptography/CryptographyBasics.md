# Purpose: These notes document my Cybersecurity learning Journey. They are intended as materials for beginners and are updated further as I gain more knowledge.

# What is Cryptography?

Cryptography is the practice of securing information by transforming it into a form that only intended parties can read or verify. The word comes from the Greek 'kryptos' (hidden) and 'graphein' (to write).
Every time you open an HTTPS website, log in with a password, or send a message on WhatsApp, cryptography is working in the background.

# Basic Terminology

    Plaintext      : The original, readable data
    Ciphertext     : The scrambled, unreadable output
    Encryption     : Converting plaintext into ciphertext
    Decryption     : Converting ciphertext back into plaintext
    Cipher         : The algorithm used for encryption and decryption
    Key            : A secret value that controls how the cipher transforms the data
    Cryptanalysis  : The study of breaking ciphers without knowing the key
    Cryptology     : Cryptography + Cryptanalysis together

    Plaintext -> [Cipher + Key] -> Ciphertext -> [Cipher + Key] -> Plaintext

An important rule here is Kerckhoffs's Principle: a system should stay secure even if everything about it except the key is public. The security must come from the key, not from keeping the algorithm hidden.

# Goals of Cryptography

Cryptography directly supports the CIA Triad and adds a couple more goals:
    1. Confidentiality   : Only authorised parties can read the data (Encryption)
    2. Integrity         : Data has not been modified (Hashing, MACs)
    3. Authentication    : Verifying who sent the data (MACs, Digital Signatures)
    4. Non-repudiation   : The sender cannot deny having sent it (Digital Signatures)

# Classical Ciphers

These are historical ciphers. They are easily broken today but are great for understanding the core ideas.

Caesar Cipher:
Each letter is shifted by a fixed number of positions in the alphabet. With a shift of 3:
    HELLO -> KHOOR
There are only 25 possible keys, so it can be broken by simply trying them all (brute force).

Substitution Cipher:
Each letter is replaced by another letter according to a fixed mapping. There are 26! possible keys, but it is broken using frequency analysis, since letters like E, T and A appear most often in English and their substitutes appear just as often in the ciphertext.

Vigenere Cipher:
Uses a keyword where each letter of the keyword decides the shift for the corresponding letter of the plaintext. Since the same plaintext letter can encrypt to different ciphertext letters, it resists simple frequency analysis, but it can still be broken once the keyword length is found.

Transposition Cipher:
Instead of replacing letters, it rearranges their positions (e.g. Rail Fence, Columnar Transposition). The letters stay the same, only their order changes.

The two ideas behind these, substitution (confusion) and transposition (diffusion), are still the building blocks of modern ciphers.

# Symmetric Encryption

In symmetric encryption, the same key is used to both encrypt and decrypt.
    Plaintext -> [Key K] -> Ciphertext -> [Key K] -> Plaintext
It is very fast and is used for encrypting large amounts of data (disk encryption, file encryption, the bulk data in HTTPS).
The main problem is key distribution: both parties need the same secret key, and sharing it securely over an insecure network is difficult.

Common algorithms:
    DES       : Old 56-bit key standard, broken by brute force, no longer secure
    3DES      : DES applied three times, deprecated
    AES       : The current standard, block size of 128 bits, key sizes of 128, 192 or 256 bits
    ChaCha20  : Modern stream cipher, fast on devices without AES hardware support

Block vs Stream Ciphers:
A block cipher (like AES) encrypts fixed-size blocks of data at a time. A stream cipher (like ChaCha20) encrypts data one bit or byte at a time by combining it with a keystream.

Modes of Operation (for block ciphers):
    ECB  : Each block encrypted separately. Identical plaintext blocks give identical ciphertext blocks, which leaks patterns. Should not be used.
    CBC  : Each block is XORed with the previous ciphertext block before encryption. Needs a random IV (Initialisation Vector).
    GCM  : Counter-based mode that provides both encryption and integrity checking. Widely used in TLS.

# Asymmetric Encryption

Asymmetric (Public Key) encryption uses a pair of mathematically linked keys:
    Public Key   : Shared openly with everyone
    Private Key  : Kept secret by the owner
Data encrypted with the public key can only be decrypted with the matching private key.
    Plaintext -> [Receiver's Public Key] -> Ciphertext -> [Receiver's Private Key] -> Plaintext
This solves the key distribution problem since the public key can be shared freely. However, it is much slower than symmetric encryption.

Common algorithms:
    RSA            : Security based on the difficulty of factoring a large number into its two primes
    ECC            : Elliptic Curve Cryptography, gives the same security as RSA with much smaller keys
    Diffie-Hellman : Not used to encrypt data, but to let two parties agree on a shared secret key over an insecure channel

# RSA with Small Numbers

Real RSA uses primes hundreds of digits long, but the steps are the same:
    1. Choose two primes: p = 3, q = 11
    2. Compute n = p x q = 33
    3. Compute phi(n) = (p-1) x (q-1) = 20
    4. Choose e such that 1 < e < phi(n) and gcd(e, phi(n)) = 1. Take e = 3
    5. Find d such that (d x e) mod phi(n) = 1. Here d = 7, since 3 x 7 = 21 and 21 mod 20 = 1
    Public Key = (e, n) = (3, 33)
    Private Key = (d, n) = (7, 33)
Encrypting the message m = 4:
    c = m^e mod n = 4^3 mod 33 = 64 mod 33 = 31
Decrypting:
    m = c^d mod n = 31^7 mod 33 = 4
Anyone who can factor n back into p and q can compute d, which is why RSA needs very large primes.

# Diffie-Hellman with Small Numbers

    1. Both agree publicly on a prime p = 23 and a base g = 5
    2. Alice picks a secret a = 6 and sends A = g^a mod p = 5^6 mod 23 = 8
    3. Bob picks a secret b = 15 and sends B = g^b mod p = 5^15 mod 23 = 19
    4. Alice computes B^a mod p = 19^6 mod 23 = 2
    5. Bob computes A^b mod p = 8^15 mod 23 = 2
Both now share the secret 2, even though a and b were never sent over the network.

# Hybrid Encryption

In practice, symmetric and asymmetric encryption are used together. Asymmetric cryptography (or Diffie-Hellman) is used to securely agree on a symmetric session key, and then the fast symmetric cipher (like AES) encrypts the actual data.
This is exactly how TLS (HTTPS), SSH and most secure messaging apps work.

# Hashing

A hash function takes input of any size and produces a fixed-size output called a hash or digest. Hashing is one-way: you cannot get the original input back from the hash.
    echo -n "hello" | sha256sum
    2cf24dba5fb0a30e26e83b2ac5b9e29e1b161e5c1fa7425e73043362938b9824
Properties of a good hash function:
    1. Deterministic          : The same input always gives the same hash
    2. Fixed Output Size      : SHA-256 always gives 256 bits, whatever the input size
    3. Pre-image Resistance   : Given a hash, it is infeasible to find the input
    4. Collision Resistance   : It is infeasible to find two inputs with the same hash
    5. Avalanche Effect       : A tiny change in the input completely changes the hash

Common algorithms:
    MD5      : 128-bit, broken (collisions found), still seen in old systems
    SHA-1    : 160-bit, broken (practical collisions shown), deprecated
    SHA-256  : Part of SHA-2, widely used and secure
    SHA-3    : Newest standard, different internal design from SHA-2

Uses of hashing:
    1. Verifying file integrity (comparing a downloaded file's hash with the official one)
    2. Storing passwords (systems store the hash, not the password itself)
    3. Digital signatures and certificates
    4. Malware identification (threat intel shares file hashes as indicators)

# Password Hashing and Salting

If two users have the same password, they would have the same hash, and attackers can use precomputed tables of hashes (Rainbow Tables) to look up common passwords instantly.
A salt is a random value added to each password before hashing, so the same password gives different hashes for different users and rainbow tables become useless.
    hash = H(salt + password)
For passwords, slow algorithms designed for the purpose are used, like bcrypt, scrypt and Argon2, rather than fast hashes like SHA-256. Being slow makes brute forcing much harder.
On Linux, password hashes are stored in /etc/shadow.

# MAC and HMAC

A MAC (Message Authentication Code) is a hash combined with a secret key. It proves both integrity and authenticity, since only someone with the key could have produced it.
HMAC is the standard way of building a MAC from a hash function (e.g. HMAC-SHA256). It is used in APIs, JWT tokens and TLS.

# Digital Signatures

A digital signature proves who sent a message and that it was not altered. It works like asymmetric encryption in reverse:
    1. The sender hashes the message
    2. The sender signs the hash using their private key
    3. The receiver verifies the signature using the sender's public key
    4. The receiver hashes the message themselves and compares
Since only the sender has the private key, a valid signature provides authentication, integrity and non-repudiation.

# Certificates and PKI

How do you know a public key really belongs to google.com and not an attacker? This is solved by Digital Certificates.
A certificate (X.509 format) binds a public key to an identity and is digitally signed by a trusted Certificate Authority (CA). Your browser and OS come with a list of trusted root CAs.
The whole system of CAs, certificates and trust is called PKI (Public Key Infrastructure).
You can view a site's certificate with:
    openssl s_client -connect google.com:443

# Encoding vs Encryption vs Hashing

These are often confused:
    Encoding    : Changes data format for compatibility. No key, easily reversible. (Base64, URL encoding, Hex)
    Encryption  : Hides data. Needs a key, reversible with the key. (AES, RSA)
    Hashing     : Fingerprints data. No key, not reversible. (SHA-256)
Example of Base64:
    echo -n "hello" | base64
    aGVsbG8=
Base64 is NOT encryption. Anyone can decode it with 'base64 -d'. Attackers often use it to hide malicious commands from casual inspection, so analysts see it a lot in logs.

# Hands-on with OpenSSL

Hashing a file:
    sha256sum file.txt
    md5sum file.txt
Symmetric encryption and decryption of a file with AES:
    openssl enc -aes-256-cbc -pbkdf2 -salt -in file.txt -out file.enc
    openssl enc -d -aes-256-cbc -pbkdf2 -in file.enc -out file_decrypted.txt
Generating an RSA key pair:
    openssl genpkey -algorithm RSA -out private.pem -pkeyopt rsa_keygen_bits:2048
    openssl pkey -in private.pem -pubout -out public.pem

# Common Attacks on Cryptography

    1. Brute Force          : Trying every possible key or password
    2. Dictionary Attack    : Trying a list of common passwords against a hash
    3. Rainbow Table Attack : Looking up hashes in precomputed tables (stopped by salting)
    4. Frequency Analysis   : Breaking classical ciphers using letter frequencies
    5. Collision Attack     : Finding two inputs with the same hash (why MD5 and SHA-1 are broken)
    6. Man-in-the-Middle    : Intercepting a key exchange (stopped by certificates and signatures)
    7. Downgrade Attack     : Forcing a connection to use an older, weaker algorithm
Most real-world breaks come from weak keys, bad implementations or poor key storage rather than the math itself being broken.

# Why does this matter in Cybersecurity?

    1. Recognising weak algorithms (MD5, SHA-1, DES, ECB mode) during audits
    2. Cracking password hashes during penetration tests using tools like Hashcat and John the Ripper
    3. Verifying file integrity and identifying malware by hash during investigations
    4. Understanding TLS traffic seen in Wireshark and tcpdump
    5. Decoding Base64 and other encoded payloads found in logs
