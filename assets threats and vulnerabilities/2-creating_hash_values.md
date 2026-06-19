# Creating Hash Values with Linux — File Integrity Verification

> **Document type:** Cryptographic hashing practical lab  
> **Context:** Google Cybersecurity Certificate — Assets, Threats and Vulnerabilities module  
> **Skills demonstrated:** Linux command line, SHA-256 hashing, file integrity verification, output redirection, byte-level file comparison with `cmp`

---

## The Scenario

Working as user `analyst` in `/home/analyst`, two files are present: `file1.txt` and `file2.txt`. Both display the same content when printed to the terminal — but the goal is to determine if they are actually identical by comparing their SHA-256 hash values.

---

## Task 1: Generate Hashes for Files

List the contents of the working directory:

```bash
ls
```

Output:
```
file1.txt  file2.txt
```

Display the contents of both files:

```bash
cat file1.txt
cat file2.txt
```

Output (both files):
```
X5O!P%@AP[4\PZX54(P^)7CC)7}$EICAR-STANDARD-ANTIVIRUS-TEST-FILE!$H+H*
```

The output looks identical — but visual inspection is not a reliable method for comparing files. A single invisible character (a trailing space, a different line ending, a null byte) would not be visible in the terminal output.

Generate the SHA-256 hash for each file:

```bash
sha256sum file1.txt
sha256sum file2.txt
```

Output:
```
131f95c51cc819465fa1797f6ccacf9d494aaaff46fa3eac73ae63ffbdfd8267  file1.txt
2558ba9a4cad1e69804ce03aa2a029526179a91a5e38cb723320e83af9ca017b  file2.txt
```

The two hash values are completely different, which proves that despite identical visual output, the files are not the same.

---

## Task 2: Compare Hashes

Write each hash to its own file using the `>>` append operator:

```bash
sha256sum file1.txt >> file1hash
sha256sum file2.txt >> file2hash
```

Display both hash files to confirm they were written correctly:

```bash
cat file1hash
cat file2hash
```

Use the `cmp` command to compare the two files byte by byte:

```bash
cmp file1hash file2hash
```

Output:
```
file1hash file2hash differ: char 1, line 1
```

The command reports the exact position of the first difference — character 1, line 1 — confirming the hashes diverge from the very first byte.

---

## Key Concepts

### Cryptographic Hash Functions

A hash function takes an input of any size and produces a fixed-length output called a **digest** or **hash value**. Key properties:

| Property | Description |
|---|---|
| Deterministic | The same input always produces the same hash |
| Fixed-length output | SHA-256 always produces a 256-bit (64 hex character) digest |
| Avalanche effect | A single bit change in the input produces a completely different hash |
| One-way | It is computationally infeasible to reverse a hash back to its input |
| Collision resistant | It is computationally infeasible to find two different inputs with the same hash |

### SHA-256

**SHA-256 (Secure Hash Algorithm 256-bit)** is part of the SHA-2 family, standardized by NIST. It is widely used for:

- File integrity verification
- Digital signatures
- Password storage (usually combined with a salt)
- Blockchain and certificate transparency

### The EICAR Test File

The string in both files (`X5O!P%@AP[4\PZX54(P^)7CC)7}$EICAR-STANDARD-ANTIVIRUS-TEST-FILE!$H+H*`) is the **EICAR standard antivirus test file** — a harmless text string recognized by antivirus engines as if it were malware, used to test whether AV software is functioning without using real malicious code. Its presence here is a hint: files that look safe on the surface may differ in ways that matter.

### Output Redirection with `>>`

The `>>` operator appends the output of a command to a file without overwriting existing content. A single `>` would overwrite the file if it already exists.

### The `cmp` Command

`cmp` compares two files byte by byte and reports the first position where they differ. It is useful for:

- Confirming that two files are identical
- Pinpointing where a corruption or modification occurred
- Automating integrity checks in scripts (returns exit code 0 if identical, 1 if different)

---

## Personal Reflection

The most valuable insight from this lab is the gap between **appearance** and **truth**. Both files displayed the same string in the terminal — a human reviewer would have concluded they were identical. The hash values told a different story instantly.

This is the core use case for cryptographic hashing in security: you cannot trust what something looks like. A tampered file, a malicious binary swapped for a legitimate one, or a corrupted download can all appear normal to the eye. Hash comparison is an objective, mathematical verification that does not rely on perception.

In practice, this technique underpins software distribution (checksums published alongside downloads), forensic evidence handling (hash of a disk image before and after analysis), and file monitoring systems like AIDE or Tripwire that alert when a file's hash changes unexpectedly.

---

*Document created as part of a professional cybersecurity portfolio.*
