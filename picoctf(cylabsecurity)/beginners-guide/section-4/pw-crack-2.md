# PW Crack 2

**Platform:** CyLab Security Academy (formerly PicoCTF)
**Category:** General Skills
**Difficulty:** Easy
**Learning Path:** Beginner's Guide to the Challenge Library (Section 4)
**Series:** PW Crack (2 of 5)

---

## Objective

Recover a flag from a password-protected Python script where the password has been lightly obfuscated using character encoding, rather than stored as a plaintext string.

---

## Concepts Learned

| Concept | Description |
|---|---|
| Character Encoding | Representing characters as numerical values, such as hexadecimal or decimal ASCII codes |
| `chr()` Function | A Python built-in that converts an integer (Unicode code point) to its corresponding character |
| Hexadecimal Notation | Base-16 number representation, commonly used in programming and low-level data analysis |
| Obfuscation vs Encryption | The difference between making data harder to read (obfuscation) and making it mathematically secure (encryption) |
| One-liner Script Evaluation | Using `python3 -c` to execute a short Python expression directly from the terminal |

---

## Approach

This level introduced a step beyond Level 1: instead of a plaintext hardcoded password, the script stored the password as a sequence of `chr()` calls using hexadecimal values. The intent was to make the password less immediately readable, but the protection this provides is minimal.

**Understanding the script:**
Opening the script in neovim revealed the same two-function structure as Level 1. The key difference was in how the password was stored. Rather than a literal string, the password was constructed by concatenating the results of multiple `chr()` calls, each taking a hexadecimal argument. For example, a character might be represented as `chr(0x34)` rather than the character `'4'` directly.

This is obfuscation, not encryption. The logic to reconstruct the original password is fully visible in the source code.

**Decoding the password:**
To reconstruct the plaintext password, I used Python's one-liner execution mode from the terminal to evaluate the same `chr()` expression the script used internally, printing the resulting string directly to the terminal.

```
python3 -c "print(chr(0x34) + chr(0x65) + chr(0x63) + chr(0x39))"
```

This printed the cleartext password, which I then supplied to the running script to retrieve the flag.

---

## Key Takeaway

Obfuscation is not a security control. Replacing a plaintext password with `chr()` calls or similar encoding tricks does not prevent a determined reader from recovering the original value. Any transformation that can be reversed by reading the source code provides no meaningful protection. True credential security requires the password to never appear in the source code at all, stored instead in environment variables, secure vaults, or as a properly hashed value that cannot be reversed.

---

## Reflection

The progression from Level 1 to Level 2 mirrors a pattern seen in real-world code: developers sometimes attempt to obscure sensitive values with light encoding, perhaps believing it will slow down an attacker. In practice, it adds seconds of effort at most. The underlying vulnerability, a secret recoverable from the source code, remains identical.

---

## References

- [Python Docs: chr()](https://docs.python.org/3/library/functions.html#chr)
- [Python Docs: Command Line Interface](https://docs.python.org/3/using/cmdline.html#cmdoption-c)