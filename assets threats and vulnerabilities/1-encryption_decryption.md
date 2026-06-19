# Decrypting Files with Linux — Caesar Cipher & OpenSSL

> **Document type:** Encryption and decryption practical lab  
> **Context:** Google Cybersecurity Certificate — Assets, Threats and Vulnerabilities module  
> **Skills demonstrated:** Linux command line, Caesar cipher decryption, OpenSSL symmetric decryption, hidden file discovery, `tr` and `cat` command usage

---

## The Scenario

Logged in as user `analyst` with `/home/analyst` as the working directory, three items are present:

```
Q1.encrypted    README.txt    caesar/
```

- `README.txt` contains instructions pointing to a hidden file inside the `caesar/` subdirectory.
- `Q1.encrypted` is an AES-256-CBC encrypted file that needs to be decrypted.
- `caesar/` contains a hidden file encrypted with the Caesar cipher that holds the decryption key.

---

## Task 1: Read the Contents of a File

List the contents of the home directory:

```bash
ls
```

Output:
```
Q1.encrypted  README.txt  caesar
```

Read the instruction file:

```bash
cat README.txt
```

The file confirms that the `caesar/` subdirectory contains a hidden file with further instructions.

---

## Task 2: Find and Decrypt the Hidden File

Navigate into the `caesar` subdirectory:

```bash
cd caesar
```

List all files, including hidden ones (files whose names start with `.`):

```bash
ls -a
```

Output:
```
.  ..  .leftShift3
```

Read the hidden file:

```bash
cat .leftShift3
```

The content appears scrambled — it is encrypted with a **Caesar cipher** with a left shift of 3. This means each letter in the ciphertext is 3 positions ahead in the alphabet compared to the original: `d` stands for `a`, `e` stands for `b`, and so on.

Decrypt it using the `tr` command:

```bash
cat .leftShift3 | tr "d-za-cD-ZA-C" "a-zA-Z"
```

### How this command works

The `tr` command translates characters from one set to another. Here:

| Parameter | Value | Meaning |
|---|---|---|
| Input set | `d-za-cD-ZA-C` | The alphabet starting from `d`, wrapping back through `a-c` (and uppercase equivalent) |
| Output set | `a-zA-Z` | The standard full alphabet in order |

This effectively shifts every letter 3 positions to the left, reversing the Caesar cipher.

The decrypted output reveals the command needed for the next task:

```bash
openssl aes-256-cbc -pbkdf2 -a -d -in Q1.encrypted -out Q1.recovered -k ettubrute
```

Return to the home directory:

```bash
cd ~
```

---

## Task 3: Decrypt the Encrypted File

Run the command revealed in the previous task:

```bash
openssl aes-256-cbc -pbkdf2 -a -d -in Q1.encrypted -out Q1.recovered -k ettubrute
```

### Command breakdown

| Option | Meaning |
|---|---|
| `openssl` | Command-line tool for cryptographic operations |
| `aes-256-cbc` | Symmetric encryption algorithm: AES with a 256-bit key in CBC mode |
| `-pbkdf2` | Uses Password-Based Key Derivation Function 2 to strengthen the key derived from the password |
| `-a` | Encodes/decodes the file using Base64 |
| `-d` | Decryption mode |
| `-in Q1.encrypted` | Input file to decrypt |
| `-out Q1.recovered` | Output file where the decrypted content is written |
| `-k ettubrute` | Password used to derive the decryption key (`ettubrute`) |

Verify the new file was created:

```bash
ls
```

Output:
```
Q1.encrypted  Q1.recovered  README.txt  caesar
```

Read the recovered message:

```bash
cat Q1.recovered
```

The hidden message is now fully readable.

---

## Key Concepts

### Caesar Cipher

A substitution cipher that shifts each letter of the alphabet by a fixed number of positions. It is one of the oldest known encryption techniques, historically attributed to Julius Caesar. Despite its simplicity, it illustrates the fundamental concept of symmetric encryption: the same key (the shift value) is used for both encryption and decryption.

With a shift of 3 to the left:

```
Ciphertext:  d e f g h i j k l m n o p q r s t u v w x y z a b c
Plaintext:   a b c d e f g h i j k l m n o p q r s t u v w x y z
```

### AES-256-CBC

**AES (Advanced Encryption Standard)** with a 256-bit key in **CBC (Cipher Block Chaining)** mode is a widely used symmetric encryption algorithm considered cryptographically secure for modern use. Unlike the Caesar cipher, it cannot be broken by simple character substitution — it requires the correct password and key derivation parameters.

### Symmetric vs. Asymmetric Encryption

| Type | Key used | Example |
|---|---|---|
| Symmetric | Same key to encrypt and decrypt | AES-256, Caesar cipher |
| Asymmetric | Public key to encrypt, private key to decrypt | RSA, ECC |

In this lab, both ciphers used are symmetric: the Caesar cipher requires knowing the shift, and OpenSSL AES requires the password `ettubrute`.

---

## Personal Reflection

This lab connects two very different generations of cryptography. The Caesar cipher is trivially broken by exhaustive search (there are only 25 possible shifts) or frequency analysis — yet it captures the essence of symmetric encryption. AES-256-CBC, by contrast, is computationally infeasible to brute-force with current technology.

The pipeline from `cat .leftShift3 | tr "d-za-cD-ZA-C" "a-zA-Z"` is also a good demonstration of the Unix philosophy: compose small tools that do one thing well. The `tr` command does not know about Caesar ciphers — it simply maps characters — yet chained with `cat`, it solves the problem cleanly.

In real incident response or forensic work, recognizing cipher patterns and knowing which tools to reach for is a core skill. This lab builds the intuition for both.

---

*Document created as part of a professional cybersecurity portfolio.*
