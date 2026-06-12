# PW Crack 4

**Platform:** CyLab Security Academy (formerly PicoCTF)
**Category:** General Skills
**Difficulty:** Medium
**Learning Path:** Beginner's Guide to the Challenge Library (Section 4)
**Series:** PW Crack (4 of 5)

---

## Objective

Extend the hash-based cracking approach from Level 3 to a significantly larger candidate list of 100 passwords, reinforcing why automation is not optional at scale.

---

## Concepts Learned

| Concept | Description |
|---|---|
| Scalable Automation | Designing a solution that handles larger inputs without requiring changes to the core logic |
| Dictionary Attack at Scale | Applying hash comparison across a larger candidate set to demonstrate the limits of weak password lists |
| Script Reusability | Adapting an existing cracking script to a new target with minimal modification |

---

## Approach

Level 4 used the same hash-based validation mechanism as Level 3, with one key difference: the candidate password list grew from 7 to 100 entries. The core logic of the attack remained identical.

**Why this level matters:**
The jump from 7 to 100 candidates is deliberate. It makes the point that manual testing, which was already unappealing at Level 3, is now clearly impractical. Anyone who tried to enter passwords one by one at Level 3 would find that approach unworkable here. The correct solution is the same automated approach introduced in the previous level, now applied to a larger input.

**Execution:**
I adapted the cracking script from Level 3 to target the Level 4 checker. The script iterated over all 100 candidate passwords, hashed each one, and identified the match against the stored hash. The correct password was then supplied to the level script to retrieve the flag.

The solution required no fundamental changes in approach, only pointing the script at the new candidate list and hash value.

---

## Key Takeaway

This level reinforces a core principle in security and in programming generally: a well-designed automated solution scales with the problem. A script that works for 7 candidates works for 100 and for 100,000 with the same logic. The attacker's effort does not grow linearly with the size of the candidate list, which is precisely why weak or predictable passwords remain dangerous even when hashed.

---

## Reflection

The progression from Level 3 to Level 4 felt like a deliberate confirmation that the approach taken at Level 3 was the right one. Had I solved Level 3 manually, Level 4 would have forced a rethink. Building the automated solution one level earlier meant Level 4 was straightforward. This is a useful reminder that investing time in a reusable, scalable solution pays off quickly.

---

## References

- [Python Docs: hashlib](https://docs.python.org/3/library/hashlib.html)
- [OWASP: Password Storage Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Password_Storage_Cheat_Sheet.html)