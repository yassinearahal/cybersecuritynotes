### Key terms

**<mark style="background: #FF5582A6;">Ciphertext</mark>** - The result of encrypting a plaintext, encrypted data .

**<mark style="background: #FF5582A6;">Cipher</mark>** - A method of encrypting or decrypting data. Modern ciphers are cryptographic, but there are many non cryptographic ciphers like Caesar.

**<mark style="background: #FF5582A6;">Plaintext</mark>** - Data before encryption, often text but not always. Could be a photograph or other file.

**<mark style="background: #FF5582A6;">Encryption</mark>** - Transforming data into ciphertext, using a cipher.

**<mark style="background: #FF5582A6;">Encoding</mark>** - NOT a form of encryption, just a form of data representation like base64. Immediately reversible.

**<mark style="background: #FF5582A6;">Key</mark>** - Some information that is needed to correctly decrypt the ciphertext and obtain the plaintext.

**<mark style="background: #FF5582A6;">Passphrase</mark>** - Separate to the key, a passphrase is similar to a password and used to protect a key.

**<mark style="background: #FF5582A6;">Asymmetric encryption</mark>** - Uses different keys to encrypt and decrypt.

**<mark style="background: #FF5582A6;">Symmetric encryption</mark>** - Uses the same key to encrypt and decrypt .

**<mark style="background: #FF5582A6;">Brute force</mark>** - Attacking cryptography by trying every different password or every different key .

**<mark style="background: #FF5582A6;">Cryptanalysis</mark>** - Attacking cryptography by finding a weakness in the underlying maths .

**<mark style="background: #FF5582A6;">Alice and Bob</mark>** - Used to represent 2 people who generally want to communicate. They’re named Alice and Bob because this gives them the initials A and B. [https://en.wikipedia.org/wiki/Alice_and_Bob](https://en.wikipedia.org/wiki/Alice_and_Bob) for more information, as these extend through the alphabet to represent many different people involved in communication.

### The importance of encryption

<mark style="background: #BBFABBA6;">Cryptography is used to protect confidentiality, ensure integrity, ensure authenticity</mark>. We use cryptography every day most likely, When we connect to SSH, our client and the server establish an encrypted tunnel so that no one can snoop on our session.

We rarely have to interact directly with cryptography, but it silently protects almost everything we do digitally.

<mark style="background: #BBFABBA6;">Whenever sensitive user data needs to be stored, it should be encrypted</mark>. Standards like [PCI-DSS](https://www.pcisecuritystandards.org/documents/PCI_DSS_for_Large_Organizations_v1.pdf) state that the data should be encrypted both at rest (in storage) AND while being transmitted. If we’re handling payment card details, we need to comply with these <mark style="background: #BBFABBA6;">PCI regulations</mark>. Medical data has similar standards. With legislation like <mark style="background: #BBFABBA6;">GDPR</mark> and California’s data protection, data breaches are extremely costly and dangerous to ur as either a consumer or a business.

<mark style="background: #FF5582A6;">We shouldn't encrypt passwords unless we’re doing something like a password manager. Passwords should not be stored in plaintext, and we should use hashing to manage them safely</mark>.

### Crucial Cryptography Mathematices

There's a little bit of math(s) that comes up relatively often in cryptography. <mark style="background: #BBFABBA6;">The Modulo operator</mark> Pretty much every programming language implements this operator, or has it available through a library. When we need to work with large numbers, use a programming language. Python is good for this as integers are unlimited in size, and we can easily get an interpreter.

When learning division for the first time, ew were probably taught to use remainders in our answer. X % Y is the remainder when X is divided by Y.

### Examples

25 % 5 = 0 (5*5 = 25 so it divides exactly with no remainder)

23 % 6 = 5 (23 does not divide evenly by 6, there would be a remainder of 5)

<mark style="background: #FF5582A6;">An important thing to remember about modulo is that it’s not reversible</mark>. If we gave us an equation: x % 5 = 4, there are infinite values of x that will be valid.

### Types of Encryption

<mark style="background: #FF5582A6;">The two main categories of Encryption are symmetric and asymmetric.</mark>

<mark style="background: #BBFABBA6;">Symmetric encryption</mark> uses the same key to encrypt and decrypt the data. Examples of Symmetric encryption are DES (Broken) and AES. These algorithms tend to be faster than asymmetric cryptography, and use smaller keys (128 or 256 bit keys are common for AES, DES keys are 56 bits long).

**<mark style="background: #BBFABBA6;">Asymmetric encryption</mark>** uses a pair of keys, one to encrypt and the other in the pair to decrypt. Examples are RSA and Elliptic Curve Cryptography. Normally these keys are referred to as a public key and a private key. Data encrypted with the private key can be decrypted with the public key, and vice versa. Our private key needs to be kept private, hence the name. Asymmetric encryption tends to be slower and uses larger keys, for example RSA typically uses 2048 to 4096 bit keys.

<mark style="background: #FF5582A6;">RSA and Elliptic Curve cryptography are based around different mathematically difficult (intractable) problems, which give them their strength.</mark>

### RSA - Rivest Shamir Adleman

**The math(s) side**

<mark style="background: #FF5582A6;">RSA is based on the mathematically difficult problem of working out the factors of a large number. </mark>It’s very quick to multiply two prime numbers together, say 17 x 23 = 391, but it’s quite difficult to work out what two prime numbers multiply together to make 14351 (113x127 for reference).

**The attacking side**

There are some excellent tools for defeating RSA challenges in CTFs, we can use this [https://github.com/Ganapati/RsaCtfTool](https://github.com/Ganapati/RsaCtfTool) which it worked very well. We’ve also [https://github.com/ius/rsatool](https://github.com/ius/rsatool).

The key variables that we need to know about for RSA in CTFs are<mark style="background: #BBFABBA6;"> p, q, m, n, e, d, and c.</mark>

“p” and “q” are large prime numbers, “n” is the product of p and q.

The public key is n and e, the private key is n and d.

“m” is used to represent the message (in plaintext) and “c” represents the ciphertext (encrypted text).

**CTFs involving RSA**

Crypto CTF challenges often present us with a set of these values, and we need to break the encryption and decrypt a message to retrieve the flag.

There’s a lot more maths to RSA, and it gets quite complicated fairly quickly. If we want to learn the maths behind it, We can take a look at MuirlandOracle’s blog post here: [https://muirlandoracle.co.uk/2020/01/29/rsa-encryption/](https://muirlandoracle.co.uk/2020/01/29/rsa-encryption/).

###  Establishing Keys Using Asymmetric Cryptography

<mark style="background: #BBFABBA6;">A very common use of asymmetric cryptography is exchanging keys for symmetric encryption.</mark>

<mark style="background: #BBFABBA6;">Asymmetric encryption tends to be slower, so for things like HTTPS symmetric encryption is better.</mark>

But the question is, how do we agree a key with the server without transmitting the key for people snooping to see?

**Metaphor time**

Imagine we have a secret code, and instructions for how to use the secret code. If we want to send our friend the instructions without anyone else being able to read it, what we could do is ask our friend for a lock.

Only they have the key for this lock, and we’ll assume we have an indestructible box that we can lock with it.

If we send the instructions in a locked box to our friend, they can unlock it once it reaches them and read the instructions.

After that, we can communicate in the secret code without risk of people snooping.

<mark style="background: #BBFABBA6;">In this metaphor, the secret code represents a symmetric encryption key, the lock represents the server’s public key, and the key represents the server’s private key.</mark>

<mark style="background: #BBFABBA6;">You’ve only used asymmetric cryptography once, so it’s fast, and you can now communicate privately with symmetric encryption.</mark>

**The Real World**

In reality, we need a little more cryptography to verify the person we’re talking to is who they say they are, which is done using <mark style="background: #FF5582A6;">digital signatures and certificates.</mark> We can find a lot more detail on how HTTPS (one example where we need to exchange keys) really works from this excellent blog post. [https://robertheaton.com/2014/03/27/how-does-https-actually-work/](https://robertheaton.com/2014/03/27/how-does-https-actually-work/)

### Digital signatures and Certificates

**What's a Digital Signature ?**

<mark style="background: #FF5582A6;">Digital signatures are a way to prove the authenticity of files,</mark> to prove who created or modified them. Using asymmetric cryptography, we produce a signature with our private key and it can be verified using our public key. As only we should have access to our private key, this proves us signed the file. Digital signatures and physical signatures have the same value in the UK, legally.

<mark style="background: #FF5582A6;">The simplest form of digital signature would be encrypting the document with our private key,</mark> and then if someone wanted to verify this signature they would decrypt it with your public key and check if the files match.

**Certificates - Prove who you are !**

<mark style="background: #BBFABBA6;">Certificates are also a key use of public key cryptography,</mark> linked to digital signatures. A common place where they’re used is for HTTPS.

The web server has a certificate that says it is the real website.com. The certificates have a chain of trust, starting with a<mark style="background: #FF5582A6;"> root CA (certificate authority).</mark> Root CAs are automatically trusted by our device, OS, or browser from install. Certs below that are trusted because the Root CAs say they trust that organisation. Certificates below that are trusted because the organisation is trusted by the Root CA and so on. There are long chains of trust. Again, this blog post explains this much better. [https://robertheaton.com/2014/03/27/how-does-https-actually-work/](https://robertheaton.com/2014/03/27/how-does-https-actually-work/)

We can get our own HTTPS certificates for domains we own using Let’s Encrypt for free. If we run a website, it’s worth setting it up.

### SSH Authentication

**Encryption and SSH authentication**

By default, SSH is authenticated using usernames and passwords in the same way that we would log in to the physical machine.

At some point, we’re almost certain to hit a machine that has SSH configured with key authentication instead. This uses public and private keys to prove that the client is a valid and authorised user on the server. <mark style="background: #BBFABBA6;">By default, SSH keys are RSA keys.</mark> We can choose which algorithm to generate, and/or add a passphrase to encrypt the SSH key. <mark style="background: #FF5582A6;">`ssh-keygen`</mark> is the program used to generate pairs of keys most of the time.

**SSH Private Keys**

<mark style="background: #FF5582A6;">We should treat our private SSH keys like passwords. We shouldn’t share them, they’re called private keys for a reason. If someone has our private key, they can use it to log in to servers that will accept it unless the key is encrypted.</mark>

<mark style="background: #FF5582A6;">It’s very important to mention that the passphrase to decrypt the key isn’t used to identify us to the server at all, all it does is decrypt the SSH key. The passphrase is never transmitted, and never leaves your system.</mark>

Using tools like John the Ripper, we can attack an encrypted SSH key to attempt to find the passphrase, which highlights the importance of using a secure passphrase and keeping our private key private.

When generating an SSH key to log in to a remote machine, we should generate the keys on our machine and then copy the public key over as this means the private key never exists on the target machine. For temporary keys generated for access to CTF boxes, this doesn't matter as much.

**How do we use these keys ?**

<mark style="background: #BBFABBA6;">The ~/.ssh folder is the default place to store these keys for OpenSSH.</mark> The <mark style="background: #FF5582A6;">`authorized_keys`</mark> (note the US English spelling) file in this directory holds public keys that are allowed to access the server if key authentication is enabled. By default on many distros, key authentication is enabled as it is more secure than using a password to authenticate. Normally for the root user, only key authentication is enabled.

In order to use a private SSH key, the permissions must be set up correctly otherwise our SSH client will ignore the file with a warning. <mark style="background: #FF5582A6;">Only the owner should be able to read or write to the private key (600 or stricter).</mark> <mark style="background: #FF5582A6;">`ssh -i keyNameGoesHere user@host`</mark> is how we specify a key for the standard Linux OpenSSH client.

**Using SSH keys to get a better shell**

<mark style="background: #BBFABBA6;">SSH keys are an excellent way to “upgrade” a reverse shell,</mark> assuming the user has login enabled (www-data normally does not, but regular users and root will). Leaving an SSH key in authorized_keys on a box can be a useful backdoor, and we don't need to deal with any of the issues of unstabilised reverse shells like Control-C or lack of tab completion.

### Diffie Hellman Key Exchange

**What is Key Exchange ?**

<mark style="background: #FF5582A6;">Key exchange allows 2 people/parties to establish a set of common cryptographic keys without an observer being able to get these keys. Generally, to establish common symmetric keys.</mark>

**How does Diffie Hellman Key Exchange work ?**

Alice and Bob want to talk securely. They want to establish a common key, so they can use symmetric cryptography, but they don’t want to use key exchange with asymmetric cryptography. This is where DH Key Exchange comes in.

Alice and Bob both have secrets that they generate, let’s call these A and B. They also have some common material that’s public, let’s call this C.

We need to make some assumptions. Firstly, whenever we combine secrets/material it’s impossible or very very difficult to separate. Secondly, the order that they're combined in doesn’t matter.

Alice and Bob will combine their secrets with the common material, and form AC and BC. They will then send these to each other, and combine that with their secrets to form two identical keys, both ABC. Now they can use this key to communicate.

**Extra Resources**

An excellent video if we want a visual explanation is available here. [https://www.youtube.com/watch?v=NmM9HA2MQGI](https://www.youtube.com/watch?v=NmM9HA2MQGI)

<mark style="background: #FF5582A6;">DH Key Exchange is often used alongside RSA public key cryptography, to prove the identity of the person we’re talking to with digital signing. This prevents someone from attacking the connection with a man-in-the-middle attack by pretending to be Bob.</mark>

### PGP, GPG and AES

**What is PGP ?**

<mark style="background: #BBFABBA6;">PGP stands for Pretty Good Privacy.</mark> It’s a software that implements encryption for encrypting files, performing digital signing and more.

**What is GPG ?**

[GnuPG or GPG](https://gnupg.org/) is an Open Source implementation of PGP from the GNU project. We may need to use GPG to decrypt files in CTFs. With PGP/GPG, private keys can be protected with passphrases in a similar way to SSH private keys. If the key is passphrase protected, we can attempt to crack this passphrase using John The Ripper and <mark style="background: #FF5582A6;">gpg2john.</mark>

The man page for GPG can be found online [here](https://www.gnupg.org/gph/de/manual/r1023.html).

### What about AES?

<mark style="background: #FF5582A6;">AES, sometimes called Rijndael after its creators, stands for Advanced Encryption Standard. It was a replacement for DES which had short keys and other cryptographic flaws.</mark>

AES and DES both operate on blocks of data (a block is a fixed size series of bits).

we’d like to learn how it works, here’s an excellent video from Computerphile [https://www.youtube.com/watch?v=O4xNJsjtN6E](https://www.youtube.com/watch?v=O4xNJsjtN6E)

### Quantum Computers and Encryption

**Asymmetric and Quantum**

While it’s unlikely we’ll have sufficiently powerful quantum computers until around 2030, once these exist encryption that uses RSA or Elliptical Curve Cryptography will be very fast to break. This is because quantum computers can very efficiently solve the mathematical problems that these algorithms rely on for their strength.

**AES/DES and Quantum**

AES with 128 bit keys is also likely to be broken by quantum computers in the near future, but 256 bit AES can’t be broken as easily. Triple DES is also vulnerable to attacks from quantum computers.

**Current Recommendations**

The NSA recommends using RSA-3072 or better for asymmetric encryption and AES-256 or better for symmetric encryption. There are several competitions currently running for quantum safe cryptographic algorithms, and it’s likely that we will have a new encryption standard before quantum computers become a threat to RSA and AES.

**More about Quantum Computers and Cryptography**

If we’d like to learn more about this, NIST has resources that detail what the issues with current encryption is and the currently proposed solutions for these. [https://doi.org/10.6028/NIST.IR.8105](https://doi.org/10.6028/NIST.IR.8105)

We also recommend the book "Cryptography Apocalypse" By Roger A. Grimes .