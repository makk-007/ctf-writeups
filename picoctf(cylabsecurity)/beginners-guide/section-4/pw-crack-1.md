# PW Crack 1

**Platform:** CyLab Security Academy (formerly PicoCTF)
**Category:** General Skills
**Difficulty:** Easy
**Learning Path:** Beginner's Guide to the Challenge Library (Section 4)
**Series:** PW Crack (1 of 5)

---

## Objective

Recover a flag from a password-protected Python script by analysing the source code to identify how the password check is implemented.

---

## Concepts Learned

| Concept | Description |
|---|---|
| Source Code Analysis | Reading a script to understand its logic before attempting to interact with it |
| XOR Encryption | A symmetric bitwise operation used to encrypt and decrypt data using a key |
| Hardcoded Credentials | Passwords or secrets embedded directly in source code, a common and serious vulnerability |
| Static Analysis | Examining code without executing it to understand its behaviour and extract useful information |

---

## Approach

The challenge provided two files: a Python password checker script and an encrypted flag. Before running anything, I opened the script in neovim to read through its logic.

**Understanding the script:**
The script contained two functions. The first was a utility function implementing XOR encryption, used to decrypt the encrypted flag by XORing it against the supplied password. The second was the password check function, which compared user input directly against a hardcoded value stored in plaintext within the script itself.

This is the critical finding: the password was not hidden, hashed, or obfuscated in any way. It was sitting in the source code as a literal string. Reading the script was sufficient to retrieve it.

**Execution:**
With the password identified through static analysis, running the script and supplying the password at the prompt decrypted the flag successfully.

---

## Key Takeaway

Hardcoded credentials in source code are one of the most prevalent and easily exploited vulnerabilities in software. Any user with access to the script file has immediate access to the password, regardless of any runtime checks. This challenge illustrates why secrets must never be embedded directly in code, and why source code review is a fundamental step in any security assessment.

---

## Reflection

This level establishes the foundation for the entire PW Crack series: read the code before you run it. The habit of opening a script in an editor before executing it is not just good security practice, it is often the most direct path to a solution. What the script is supposed to protect is sometimes less well-protected than the author intended.

---

## References

- [OWASP: Use of Hard-coded Credentials](https://owasp.org/www-community/vulnerabilities/Use_of_hard-coded_password)
- [Python Docs: Built-in Functions](https://docs.python.org/3/library/functions.html)