# PW Crack 3

**Platform:** CyLab Security Academy (formerly PicoCTF)
**Category:** General Skills
**Difficulty:** Medium
**Learning Path:** Beginner's Guide to the Challenge Library (Section 4)
**Series:** PW Crack (3 of 5)

---

## Objective

Crack a password-protected script where the password is validated by comparing hashes rather than plaintext values, using a custom cracking script against a known list of candidate passwords.

---

## Concepts Learned

| Concept | Description |
|---|---|
| Password Hashing | Transforming a password into a fixed-length digest using a one-way function, so the original value is not stored directly |
| Hash Comparison | Validating a password by hashing the input and comparing it to a stored hash, rather than comparing plaintext |
| Dictionary Attack | Testing a predefined list of candidate passwords against a target hash to find a match |
| Writing a Custom Cracking Script | Automating the process of testing multiple candidates programmatically rather than manually |
| MD5 / SHA Hashing | Common hashing algorithms used in password storage (and frequently misused for security) |

---

## Approach

Level 3 introduced a meaningful shift in the password storage mechanism. Rather than storing the password in plaintext or as an obfuscated string, the script stored a precomputed hash of the correct password and validated input by hashing it and comparing the result.

**Understanding the script:**
Opening the script in neovim revealed the new structure. The password check function now hashed the user's input and compared it against a hardcoded hash value. Crucially, the script also contained a list of seven candidate passwords, one of which was the correct one. The challenge was identifying which candidate produced a hash matching the stored value.

**The manual approach and why I automated it:**
With seven candidates, manually entering each one into the running script was technically feasible but inefficient and error-prone. More importantly, the series description explicitly states that each level builds on the previous one, signalling that the candidate list would grow. Writing an automated solution from the start was the more deliberate and transferable approach.

**Writing crack3.py:**
I wrote a short Python script that iterated over the seven candidate passwords, hashed each one using the same algorithm as the checker script, and compared the result to the stored hash. When a match was found, the script printed the correct password, which I then supplied to the level script to retrieve the flag.

This approach mirrors how real password cracking tools operate at their core: hash a candidate, compare, repeat.

---

## Key Takeaway

Hashing a password is a meaningful improvement over storing it in plaintext, but it does not make a password uncrackable. When the attacker has access to both the hash and the candidate list (as is the case here, since both are in the source code), a dictionary attack will always succeed given enough candidates and time. The strength of hashed password storage depends on the quality of the hash algorithm, the use of salting, and the unpredictability of the password itself.

---

## Reflection

Writing a dedicated cracking script rather than manually testing candidates was the correct instinct here. The moment a problem involves iterating over a list and comparing values, automation is the right tool. This is a pattern that scales directly into real security work: the same logic underlies tools like Hashcat and John the Ripper, which are simply more optimised implementations of the same fundamental approach.

---

## References

- [Python Docs: hashlib](https://docs.python.org/3/library/hashlib.html)
- [OWASP: Password Storage Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Password_Storage_Cheat_Sheet.html)