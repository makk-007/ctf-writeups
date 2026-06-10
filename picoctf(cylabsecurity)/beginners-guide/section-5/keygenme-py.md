# keygenme-py

**Platform:** CyLab Security Academy (formerly PicoCTF)
**Category:** Reverse Engineering
**Difficulty:** Medium
**Learning Path:** Beginner's Guide to the Challenge Library (Section 5)
**Note:** This challenge falls under Reverse Engineering, encountered as part of the Beginner's Guide learning path.

---

## Objective

Reverse engineer a Python key validation script to understand how a valid licence key is constructed, then write a keygen script to derive the correct key from a given username.

---

## Concepts Learned

| Concept | Description |
|---|---|
| Key Generation Logic | Understanding how software licence keys are algorithmically constructed from user-specific input |
| SHA-256 Hashing | Using the SHA-256 algorithm to derive a deterministic hash from an input string |
| String Indexing | Extracting characters from specific positions within a string to construct a derived value |
| Writing a Keygen | Replicating a program's key construction logic in a separate script to generate valid keys |
| Reverse Engineering a Validation Routine | Working backwards from a checker to understand what a valid input looks like |

---

## Approach

The challenge provided a Python trial application that validated a licence key. Rather than cracking a password, the objective was to understand how the application constructed and verified valid keys, then reproduce that process.

**Reading the validation script:**
Opening the script in neovim revealed that the key had two components: static parts that were fixed string literals, and dynamic parts that were derived from specific character positions within the SHA-256 hash of the username. The validation routine checked that the supplied key matched a value assembled from these components.

**Understanding the key structure:**
The static portions of the key were visible directly in the source code. The dynamic portions required computing the SHA-256 hash of the target username and extracting characters from particular indices of the resulting hash string. The validation logic specified exactly which indices to use, making it possible to reconstruct the expected key precisely.

**Writing solve_keygenme.py:**
Rather than working out the key manually, I wrote a dedicated script that replicated the key construction logic: computed the SHA-256 hash of the username, extracted the required characters at the specified indices, and assembled them with the static parts to produce the valid key. Running this script against the target username produced the correct licence key, which the trial application accepted.

---

## Key Takeaway

Licence key validation logic, when embedded in client-side code, is inherently reversible. Any key construction algorithm that runs on the user's machine can be read, understood, and replicated. This is why software protection schemes that rely solely on client-side key validation are fundamentally limited: a determined analyst can always write a keygen by studying the validation routine. Robust licence enforcement requires server-side validation where the construction logic is not exposed.

---

## Reflection

This was the most technically involved challenge in the Beginner's Guide and a genuine introduction to reverse engineering as a discipline. The process of reading a validation routine and working backwards to understand what constitutes a valid input is a transferable skill that appears in vulnerability research, malware analysis, and software auditing. Writing the keygen script rather than manually computing the key also reinforced the automation-first mindset developed through the PW Crack series.

---

## References

- [Python Docs: hashlib (SHA-256)](https://docs.python.org/3/library/hashlib.html)
- [Python Docs: String indexing and slicing](https://docs.python.org/3/tutorial/introduction.html#strings)
- [Wikipedia: Key generation](https://en.wikipedia.org/wiki/Key_generation)