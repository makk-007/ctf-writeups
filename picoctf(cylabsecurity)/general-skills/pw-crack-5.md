# PW Crack 5

**Platform:** CyLab Security Academy (formerly PicoCTF)
**Category:** General Skills
**Difficulty:** Medium
**Learning Path:** Beginner's Guide to the Challenge Library (Section 4)
**Series:** PW Crack (5 of 5)

---

## Objective

Complete the PW Crack series by cracking a hashed password using an external dictionary file rather than a candidate list embedded in the checker script itself.

---

## Concepts Learned

| Concept | Description |
|---|---|
| Wordlist-Based Dictionary Attack | Using an external file of candidate passwords to drive a cracking script, rather than a hardcoded list |
| File I/O in Python | Reading lines from an external file to feed into a processing loop |
| Real-World Cracking Workflow | Understanding how professional tools like Hashcat and John the Ripper use wordlists such as rockyou.txt |
| External Wordlist vs Embedded Candidate List | The shift from a script-internal list to a separate dictionary file, reflecting more realistic attack scenarios |

---

## Approach

Level 5 introduced a structural change from the previous two levels. Rather than embedding the candidate password list directly inside the checker script, the challenge provided a separate dictionary file containing all possible passwords. The hash validation mechanism remained the same.

**Understanding the shift:**
In Levels 3 and 4, the candidate list was part of the source code being analysed. Here, the candidates lived in an external file, decoupling the wordlist from the checker. This more closely mirrors real-world password cracking scenarios, where an attacker obtains a hash (from a database dump, for example) and runs it against a known wordlist like rockyou.txt using a dedicated tool.

**Writing the cracking script:**
I wrote a dedicated cracking script that opened the dictionary file, iterated over each candidate password line by line, hashed each one using the same algorithm as the checker, and compared the result to the target hash. When a match was found, the script returned the correct password.

The key addition over previous levels was reading candidates from a file rather than from a hardcoded list, a small but meaningful change in how the input was sourced.

**Execution:**
With the correct password identified by the cracking script, I supplied it to the level checker to retrieve the flag.

---

## Key Takeaway

This final level in the series ties the PW Crack progression to real-world password cracking methodology. Professional cracking tools are, at their core, optimised implementations of the same loop: read a candidate, hash it, compare it, repeat. The use of an external wordlist is standard practice because it separates the cracking logic from the candidate data, allowing the same tool to be used against different targets with different wordlists. Understanding this workflow is foundational to both offensive password cracking and defensive password policy design.

---

## Reflection

The PW Crack series as a whole traced a clear and well-constructed progression: plaintext storage, obfuscated storage, hashed storage with a small candidate list, hashed storage with a larger list, and finally hashed storage with an external wordlist. Each level addressed the weakness of the previous approach while introducing a new concept. By Level 5, the cracking script I wrote shared the same fundamental logic as industry tools, just without the optimisations. That is a satisfying place to end a series.

---

## References

- [Python Docs: hashlib](https://docs.python.org/3/library/hashlib.html)
- [Python Docs: Reading and Writing Files](https://docs.python.org/3/tutorial/inputoutput.html#reading-and-writing-files)
- [OWASP: Password Storage Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Password_Storage_Cheat_Sheet.html)
- [Hashcat: Official Documentation](https://hashcat.net/wiki/)