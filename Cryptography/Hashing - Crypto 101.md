### Key Terms

**<mark style="background: #FF5582A6;">Plaintext</mark>** - Data before encryption or hashing, often text but not always as it could be a photograph or other file instead.

**<mark style="background: #FF5582A6;">Encoding</mark>** - This is NOT a form of encryption, just a form of data representation like base64 or hexadecimal. Immediately reversible.

**<mark style="background: #FF5582A6;">Hash</mark>** - A hash is the output of a hash function. Hashing can also be used as a verb, "to hash", meaning to produce the hash value of some data.

**<mark style="background: #FF5582A6;">Brute force</mark>** - Attacking cryptography by trying every different password or every different key

**<mark style="background: #FF5582A6;">Cryptanalysis</mark>** - Attacking cryptography by finding a weakness in the underlying maths

### What is a hash function ?

Hash functions are <mark style="background: #BBFABBA6;">quite different from encryption</mark>. There is no key, and it’s meant to be impossible (or very very difficult) to go from the output back to the input.

A hash function takes some input data of any size, and creates a summary or "digest" of that data. The output is a fixed size. It’s hard to predict what the output will be for any input and vice versa. Good hashing algorithms will be (relatively) fast to compute, and slow to reverse (Go from output and determine input). Any small change in the input data (even a single bit) should cause a large change in the output.

The output of a hash function is normally raw bytes, which are then encoded. Common encodings for this are base 64 or hexadecimal. Decoding these won’t give you anything useful.

**What's a hash collision ?**

A hash collision is when <mark style="background: #BBFABBA6;">2 different inputs give the same output</mark>. Hash functions are designed to avoid this as best as they can, especially being able to engineer (create intentionally) a collision. Due to the <mark style="background: #FF5582A6;">pigeonhole effect, collisions are not avoidable</mark>. The pigeonhole effect is basically, there are a set number of different output values for the hash function, but you can give it any size input. As there are more inputs than outputs, some of the inputs must give the same output. If you have 128 pigeons and 96 pigeonholes, some of the pigeons are going to have to share.

<mark style="background: #FF5582A6;">MD5 and SHA1</mark> have been attacked, and made technically insecure due to engineering hash collisions. However, no attack has yet given a collision in both algorithms at the same time so if we use the MD5 hash AND the SHA1 hash to compare, we will see they’re different. The MD5 collision example is available from [https://www.mscs.dal.ca/~selinger/md5collision/](https://www.mscs.dal.ca/~selinger/md5collision/)[](https://www.mscs.dal.ca/~selinger/md5collision/) and details of the SHA1 Collision are available from [https://shattered.io/](https://shattered.io/). Due to these, we shouldn't trust either algorithm for hashing passwords or data.

### Uses for hashing

**What can we do with hashing ?**  

Hashing is used for 2 main purposes in Cyber Security. To verify <mark style="background: #FF5582A6;">integrity of data, or for verifying passwords</mark>.

**Hashing for password verification**

Most webapps need to verify a user's password at some point. Storing these passwords in plain text would be bad. We've probably seen news stories about companies that have had their database leaked. Knowing some people, they use the same password for everything including their banking, so leaking these would be really really bad.

Quite a few data breaches have leaked plaintext passwords. We’re probably familiar with “<mark style="background: #FF5582A6;">rockyou.txt</mark>” on Kali as a password wordlist. This came from a company that made widgets for MySpace. They stored their passwords in plaintext and the company had a data breach. The txt file contains over 14 million passwords (although some are *unlikely* to have been user passwords. Sort by length if you want to see what I mean).

Adobe had a notable data breach that was slightly different. The passwords were encrypted, rather than hashed and the encryption that was used was not secure. This meant that the plaintext could be relatively quickly retrieved. If we want to read more about this breach, this post from Sophos is excellent: [https://nakedsecurity.sophos.com/2013/11/04/anatomy-of-a-password-disaster-adobes-giant-sized-cryptographic-blunder/](https://nakedsecurity.sophos.com/2013/11/04/anatomy-of-a-password-disaster-adobes-giant-sized-cryptographic-blunder/)

Linkedin also had a data breach. Linkedin used SHA1 for password verification, which is quite quick to compute using GPUs. 

We can't encrypt the passwords, as the key has to be stored somewhere. If someone gets the key, they can just decrypt the passwords.

This is where hashing comes in. What if, instead of storing the password, we just stored the hash of the password? This means we never have to store the user's password, and if our database was leaked then an attacker would have to crack each password to find out what the password was.

There's just one problem with this. What if two users have the same password? As a hash function will always turn the same input into the same output, we will store the same password hash for each user. That means if someone cracks that hash, they get into more than one account. It also means that someone can create a "<mark style="background: #FF5582A6;">Rainbow table</mark>" to break the hashes.

A rainbow table <mark style="background: #FF5582A6;">is a lookup table of hashes to plaintexts</mark>, so we can quickly find out what password a user had just from the hash. A rainbow table trades time taken to crack a hash for hard disk space, but they do take time to create.  
Here's a quick example so we can try and understand what they're like.

|Hash|Password|
|---|---|
|02c75fb22c75b23dc963c7eb91a062cc|zxcvbnm|
|b0baee9d279d34fa1dfd71aadb908c3f|11111|
|c44a471bd78cc6c2fea32b9fe028d30a|asdfghjkl|
|d0199f51d2728db6011945145a1b607a|basketball|
|dcddb75469b4b4875094e14561e573d8|000000|
|e10adc3949ba59abbe56e057f20f883e|123456|
|e19d5cd5af0378da05f63f891c7467af|abcd1234|
|e99a18c428cb38d5f260853678922e03|abc123|
|fcea920f7412b5da7be0cf42b8c93759|1234567|

Websites like [CrackStation - Online Password Hash Cracking - MD5, SHA1, Linux, Rainbow Tables, etc.](https://crackstation.net/) internally use HUGE rainbow tables to provide fast password cracking for hashes without salts. Doing a lookup in a sorted list of hashes is really quite fast, much <mark style="background: #BBFABBA6;">much faster than trying to crack the hash</mark>.

**Protecting against rainbow tables**

To protect against rainbow tables, we add a <mark style="background: #BBFABBA6;">salt</mark> to the passwords. The salt is <mark style="background: #BBFABBA6;">randomly generated and stored in the database</mark>, <mark style="background: #FF5582A6;">unique to each user</mark>. In theory, we could use the same salt for all users but that means that duplicate passwords would still have the same hash, and a rainbow table could still be created specific passwords with that salt.

The salt is added to either <mark style="background: #BBFABBA6;">the start or the end of the password</mark> before it’s hashed, and this means that every user will have a different password hash even if they have the same password. Hash functions like <mark style="background: #FF5582A6;">bcrypt</mark> and <mark style="background: #FF5582A6;">sha512crypt</mark> handle this automatically. Salts don’t need to be kept private.

### Recognising password hashes

Automated hash recognition tools such as [https://pypi.org/project/hashID/](https://pypi.org/project/hashID/) exist, but they are unreliable for many formats. For hashes that have a prefix, the tools are reliable. Use a healthy combination of context and tools.  If we found the hash in a web application database, it's more likely to be <mark style="background: #FF5582A6;">md5 than NTLM</mark>. Automated hash recognition tools often get these hash types mixed up, which highlights the importance of learning yourself.

Unix style password hashes are very easy to recognise, as they have a prefix. The prefix tells us the hashing algorithm used to generate the hash. The standard format is:

```bash
$format$rounds$salt$hash
```

Windows passwords are hashed using <mark style="background: #FF5582A6;">NTLM, which is a variant of md4</mark>. They're visually identical to md4 and md5 hashes, so it's very important to use context to work out the hash type.

On Linux, password hashes are stored in <mark style="background: #FF5582A6;">/etc/shadow</mark>. This file is normally only readable by <mark style="background: #FF5582A6;">root</mark> They <mark style="background: #BBFABBA6;">used to be stored in /etc/passwd</mark>, and were readable by everyone.

On Windows, password hashes are stored in the <mark style="background: #FF5582A6;">SAM</mark>. Windows tries to prevent normal users from dumping them, but tools like <mark style="background: #BBFABBA6;">mimikatz</mark> exist for this. Importantly, the hashes found there are split into <mark style="background: #FF5582A6;">NT hashes and LM hashes</mark>.

Here's a quick table of the most Unix style password prefixes that we'll see.

|   |   |
|---|---|
|Prefix|Algorithm|
|$1$|md5crypt, used in Cisco stuff and older Linux/Unix systems|
|$2$, $2a$, $2b$, $2x$, $2y$|Bcrypt (Popular for web applications)|
|$6$|sha512crypt (Default for most Linux/Unix systems)|

A great place to find more hash formats and password prefixes is the <mark style="background: #BBFABBA6;">hashcat example page</mark>, available here: [https://hashcat.net/wiki/doku.php?id=example_hashes](https://hashcat.net/wiki/doku.php?id=example_hashes).  
For other hash types, we'll normally need to go by <mark style="background: #BBFABBA6;">length, encoding or some research into the application that generated them</mark>. Never underestimate the <mark style="background: #FF5582A6;">power of research</mark>.

### Password Cracking

We've already mentioned rainbow tables as a method to crack hashes that don't have a salt, but what if there's a salt involved ?

We can't "decrypt" password hashes. They're not encrypted. We have to crack the hashes by hashing a large number of different inputs (often rockyou, these are the possible passwords), potentially adding the salt if there is one and comparing it to the target hash. Once it matches, we know what the password was. Tools like <mark style="background: #BBFABBA6;">Hashcat and John the Ripper</mark> are normally used for this.

**Why crack on GPUs ?**

Graphics cards have thousands of cores. Although they can’t do the same sort of work that a CPU can, they are very good at some of the maths involved in hash functions. This means we can use a graphics card to crack most hash types much more quickly. <mark style="background: #BBFABBA6;">Some hashing algorithms, notably bcrypt, are designed so that hashing on a GPU is about the same speed as hashing on a CPU which helps them resist cracking</mark>.

**Cracking on VMs ?**

It’s worth mentioning that virtual machines normally don’t have access to the host's graphics card(s) (We can set this up, but it’s a lot of work). If we want to run hashcat, it’s best to run it on our host (Windows builds are available on the website, run it from powershell). We can get Hashcat working with OpenCL in a VM, but the speeds will likely be much worse than cracking on our host. <mark style="background: #FF5582A6;">John the ripper uses CPU by default</mark> and as such, works in a VM out of the box although we may get better speeds running it on the host OS as it will have more threads and no overhead from running in a VM.

<mark style="background: #FF5582A6;">NEVER (I repeat, NEVER!) use --force for hashcat. It can lead to false positives (wrong passwords being given to you) and false negatives (skips over the correct hash).</mark>

<mark style="background: #BBFABBA6;">UPDATE: As of Kali 2020.2, hashcat 6.0 will run on the CPU without --force. I still recommend cracking on our host OS if we have a GPU, as it will be much much faster.</mark>

### Hashing for integrity checking

**Integrity Checking**

<mark style="background: #BBFABBA6;">Hashing can be used to check that files haven't been changed.</mark> If we put the same data in, we always get the same data out. If even a single bit changes, the hash will change a lot. This means we can use it to check that files haven't been modified or to make sure that they have downloaded correctly. We can also use hashing to find duplicate files, if two pictures have the same hash then they are the same picture.

**HMACs**

<mark style="background: #BBFABBA6;">HMAC is a method of using a cryptographic hashing function to verify the authenticity and integrity of data.</mark> A HMAC can be used to ensure that the person who created the HMAC is who they say they are (authenticity), and that the message hasn’t been modified or corrupted (integrity). They use <mark style="background: #FF5582A6;">a secret key, and a hashing algorithm in order to produce a hash.</mark> 