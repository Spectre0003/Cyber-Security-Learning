# Purpose: These notes document my Cybersecurity learning Journey. They are intended as materials for beginners and are updated further as I gain more knowledge.

# What is Public Key Cryptography?

Public Key Cryptography (also called Asymmetric Cryptography) is a system where every user has two keys instead of one:
    Public Key   : Can be given to anyone, even published on the internet
    Private Key  : Never leaves the owner
The two keys are mathematically linked, but knowing the public key does not let anyone work out the private key in any practical amount of time.
It was introduced publicly by Whitfield Diffie and Martin Hellman in 1976, followed by RSA (Rivest, Shamir, Adleman) in 1977. The basic idea of asymmetric encryption, along with the toy RSA and Diffie-Hellman examples, is in CryptographyBasics.md. This note goes deeper.

# Why was it needed?

With symmetric encryption, every pair of people needs their own shared secret key. For n people that is n(n-1)/2 keys:
    10 people   -> 45 keys
    1000 people -> 499,500 keys
And each of those keys must be exchanged secretly before any communication can happen.
With public key cryptography, each person needs just one key pair (n pairs in total), and public keys can be shared openly. Two strangers on the internet can communicate securely without ever having met.

# Trapdoor One-Way Functions

Public key cryptography is built on mathematical problems that are easy to compute in one direction but extremely hard to reverse, unless you know a secret (the 'trapdoor').
    Integer Factorisation : Multiplying two large primes is easy; factoring the product back is hard (RSA)
    Discrete Logarithm    : Computing g^x mod p is easy; finding x from the result is hard (Diffie-Hellman, DSA, ElGamal)
    Elliptic Curve Discrete Logarithm : The same problem on elliptic curves, even harder per bit (ECC)
The private key is the trapdoor. Without it, reversing the operation would take longer than the age of the universe with today's computers.

# The Two Ways a Key Pair is Used

Encryption (Confidentiality):
    Encrypt with the RECEIVER's PUBLIC key  ->  only the receiver's PRIVATE key can decrypt
Anyone can send you a secret message, but only you can read it.

Digital Signatures (Authentication, Integrity, Non-repudiation):
    Sign with the SENDER's PRIVATE key  ->  anyone verifies with the sender's PUBLIC key
Only you can produce the signature, but anyone can check it.

An easy way to remember it:
    Public key  = a padlock you hand out to everyone. Anyone can lock a box, only you can open it.
    Private key = your personal seal. Only you can stamp it, anyone can recognise it.

# RSA in Detail

Key Generation:
    1. Pick two large random primes p and q (each around 1024+ bits for a 2048-bit key)
    2. n = p x q (the modulus, part of both keys)
    3. phi(n) = (p-1)(q-1)
    4. Pick public exponent e. In practice e = 65537 is almost always used, as it is prime and makes encryption fast
    5. Compute d = e^-1 mod phi(n), the private exponent
    Public Key  = (n, e)
    Private Key = (n, d), with p and q also kept secret
Encryption and Decryption:
    c = m^e mod n
    m = c^d mod n
Signing and Verifying:
    s = H(m)^d mod n          (sign the hash with the private key)
    H(m) == s^e mod n ?       (verify with the public key)
It works because of Euler's theorem: (m^e)^d = m^(ed) = m mod n, since ed = 1 mod phi(n).

Padding:
'Textbook' RSA, as written above, is insecure in practice:
    1. It is deterministic, the same message always gives the same ciphertext, so guesses can be checked
    2. It is malleable, an attacker can modify a ciphertext in predictable ways
    3. Small messages with small e can be recovered directly
To fix this, random padding is added before encryption:
    OAEP          : Used for RSA encryption today
    PSS           : Used for RSA signatures today
    PKCS#1 v1.5   : Older padding, vulnerable to Bleichenbacher's padding oracle attack when used for encryption
RSA is also slow and can only encrypt data smaller than its key size, so in practice it only encrypts a symmetric key or signs a hash, never large files directly.

# Diffie-Hellman in Detail

Diffie-Hellman (DH) is a key exchange, not an encryption algorithm. Two parties each combine their own private value with the other's public value and arrive at the same shared secret, which is then used as a symmetric key.
An eavesdropper sees p, g, A and B, but recovering a or b from them is the discrete logarithm problem.

The Man-in-the-Middle Problem:
Plain DH has no authentication. An attacker sitting in the middle can perform one DH exchange with Alice and a separate one with Bob, then decrypt, read and re-encrypt everything passing between them. Neither side notices.
This is why DH is always combined with digital signatures and certificates, so each side can prove the public value really came from the other.

Ephemeral DH and Forward Secrecy:
In DHE / ECDHE (the E stands for Ephemeral), fresh DH key pairs are generated for every session and thrown away afterwards.
This gives Perfect Forward Secrecy (PFS): even if a server's long-term private key is stolen later, previously recorded sessions cannot be decrypted, as their session keys no longer exist anywhere.

# Elliptic Curve Cryptography (ECC)

ECC performs public key operations using points on an elliptic curve (y^2 = x^3 + ax + b over a finite field) instead of plain large numbers.
    Private Key : A random number k
    Public Key  : The point Q = k x G, where G is a fixed starting point on the curve
Computing Q from k is fast; finding k from Q is the elliptic curve discrete logarithm problem.
Its biggest advantage is much smaller keys for the same security, which means faster operations and less bandwidth, ideal for mobile devices and TLS.

Common ECC algorithms:
    ECDH      : Elliptic curve Diffie-Hellman key exchange
    ECDSA     : Elliptic curve digital signatures (used by Bitcoin and many TLS certificates)
    X25519    : Modern ECDH on Curve25519, used by TLS 1.3, SSH and Signal
    Ed25519   : Modern signature algorithm on Curve25519, the recommended default for SSH keys

Note on ECDSA: Every signature requires a random number (nonce). If the same nonce is ever reused for two signatures, the private key can be calculated from them. This is exactly how the Sony PlayStation 3 signing key was extracted in 2010. Ed25519 avoids this by deriving the nonce deterministically.

# Key Size Comparison

Approximate equivalent security levels (based on NIST guidance):
    Security (bits) | Symmetric | RSA / DH   | ECC
    80              | 2TDEA     | 1024       | 160    (no longer acceptable)
    112             | 3DES      | 2048       | 224
    128             | AES-128   | 3072       | 256
    192             | AES-192   | 7680       | 384
    256             | AES-256   | 15360      | 512+
So a 256-bit ECC key is roughly as strong as a 3072-bit RSA key. RSA-2048 is the current minimum in use.

# Key File Formats

    PEM      : Base64 text between '-----BEGIN ...-----' and '-----END ...-----' lines. The most common format
    DER      : The same data in raw binary form
    PKCS#1   : RSA-specific format ('BEGIN RSA PRIVATE KEY')
    PKCS#8   : Generic private key format for any algorithm ('BEGIN PRIVATE KEY' or 'BEGIN ENCRYPTED PRIVATE KEY')
    OpenSSH  : SSH's own format ('BEGIN OPENSSH PRIVATE KEY'), public keys look like 'ssh-ed25519 AAAA... comment'
Finding a file containing 'BEGIN PRIVATE KEY' on a compromised system, in a public GitHub repo or in a backup is a critical finding.

# Public Key Infrastructure (PKI) in Detail

Public keys solve key distribution, but create a new problem: how do you trust that a public key really belongs to who it claims? PKI answers this with a chain of trust.
    Root CA          : Self-signed, pre-installed in your OS/browser trust store, kept offline
    Intermediate CA  : Signed by the Root CA, issues certificates day to day
    Leaf Certificate : The website's certificate, signed by the Intermediate CA
Your browser verifies each signature up the chain until it reaches a root it already trusts.
A certificate contains the subject (domain name), the public key, the issuer, a validity period, a serial number and the CA's signature.
If a certificate's private key is compromised, it can be revoked using:
    CRL   : Certificate Revocation List, a list of revoked serial numbers published by the CA
    OCSP  : Online Certificate Status Protocol, checks the status of a single certificate in real time
Viewing a certificate's details:
    openssl x509 -in cert.pem -text -noout

Web of Trust:
PGP/GPG uses an alternative model with no central CA. Users sign each other's public keys to vouch for them, and trust spreads through those signatures.

# Where Public Key Cryptography is Used

TLS 1.3 (HTTPS), simplified:
    1. Client Hello  : Client sends supported ciphers and its ECDHE public value
    2. Server Hello  : Server replies with its ECDHE public value
    3. Both compute the same shared secret and derive symmetric session keys
    4. Server sends its certificate and a signature over the handshake (proves it owns the private key)
    5. Client verifies the certificate chain and the signature
    6. All further data is encrypted with AES-GCM or ChaCha20 using the session keys
TLS 1.3 removed plain RSA key exchange entirely, so every connection has forward secrecy.

SSH Key Authentication:
    ssh-keygen -t ed25519 -C "my laptop"
This creates ~/.ssh/id_ed25519 (private) and ~/.ssh/id_ed25519.pub (public).
    ssh-copy-id user@server
This adds your public key to ~/.ssh/authorized_keys on the server. When you log in, the server sends a challenge which your client signs with the private key, so the private key is never sent over the network.
The file ~/.ssh/known_hosts stores the server's public key so you get a warning if it ever changes, which could indicate a MITM attack.

Others:
    1. PGP/GPG email and file encryption
    2. Code signing (Windows executables, Linux packages, mobile apps)
    3. Cryptocurrency wallets (the wallet address is derived from the public key)
    4. Signal and WhatsApp end-to-end encryption

# Hands-on with OpenSSL and GPG

(Key pair generation with openssl is shown in CryptographyBasics.md.)
Viewing the components of an RSA private key (modulus, exponents, primes):
    openssl pkey -in private.pem -text -noout
Encrypting with the public key and decrypting with the private key (OAEP padding):
    openssl pkeyutl -encrypt -pubin -inkey public.pem -pkeyopt rsa_padding_mode:oaep -in msg.txt -out msg.enc
    openssl pkeyutl -decrypt -inkey private.pem -pkeyopt rsa_padding_mode:oaep -in msg.enc
Signing a file and verifying the signature:
    openssl dgst -sha256 -sign private.pem -out msg.sig msg.txt
    openssl dgst -sha256 -verify public.pem -signature msg.sig msg.txt
This prints 'Verified OK'. Change even one character in msg.txt and it prints 'Verification failure'.
Generating an Ed25519 key:
    openssl genpkey -algorithm ED25519 -out ed25519.pem
Using GPG:
    gpg --full-generate-key
    gpg --encrypt --recipient friend@example.com file.txt
    gpg --decrypt file.txt.gpg
    gpg --sign file.txt
    gpg --verify file.txt.gpg

# Attacks and Weaknesses

    1. Private Key Theft       : By far the most common real-world failure. Keys left in repos, backups, or unprotected files
    2. Man-in-the-Middle       : Swapping public keys during exchange when there is no authentication
    3. Weak Randomness         : Bad random number generators produce predictable or shared primes, so keys can be factored
    4. Nonce Reuse             : Reusing the nonce in ECDSA leaks the private key
    5. Padding Oracle Attacks  : Exploiting PKCS#1 v1.5 error messages to decrypt ciphertexts (Bleichenbacher)
    6. Small Key Sizes         : RSA-512 and RSA-768 have been publicly factored
    7. Rogue Certificates      : A compromised or careless CA issuing certificates for domains it should not (e.g. DigiNotar, 2011)

# The Quantum Threat

A large enough quantum computer running Shor's algorithm could efficiently solve both factorisation and discrete logarithms, breaking RSA, DH and ECC completely. Symmetric ciphers and hashes are only weakened (Grover's algorithm), which is countered by using larger keys like AES-256.
Because attackers can record encrypted traffic today and decrypt it once quantum computers exist ('Harvest Now, Decrypt Later'), migration has already started. In 2024, NIST published the first Post-Quantum Cryptography standards:
    ML-KEM (FIPS 203)   : Key encapsulation, based on CRYSTALS-Kyber
    ML-DSA (FIPS 204)   : Digital signatures, based on CRYSTALS-Dilithium
    SLH-DSA (FIPS 205)  : Hash-based digital signatures, based on SPHINCS+
Hybrid key exchanges combining X25519 with ML-KEM are already used by major browsers and OpenSSH.

# Why does this matter in Cybersecurity?

    1. Configuring SSH key authentication and disabling password logins on servers
    2. Inspecting TLS certificates during investigations (expired, self-signed, or mismatched certificates are red flags)
    3. Hunting for exposed private keys during penetration tests and secret scanning
    4. Verifying signatures of downloaded tools and packages before running them
    5. Understanding MITM attacks and why certificate warnings must never be ignored
