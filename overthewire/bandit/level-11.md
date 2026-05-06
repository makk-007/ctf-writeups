# Bandit Level 11 → Level 12

**Platform:** OverTheWire  
**Wargame:** Bandit  
**Date Completed:** 2026-05-06  
**Tags:** `rot13` `caesar-cipher` `tr` `text-substitution` `classical-cryptography`

---

## Objective

The password for Level 12 is stored in `data.txt`, where all alphabetic characters have been encoded using ROT13. Decode it to retrieve the password.

---

## Concepts Learned

- **ROT13:** A simple Caesar cipher that shifts every letter 13 positions forward in the alphabet. Because the alphabet has 26 letters, applying ROT13 twice returns the original text - making it its own inverse. It is not a secure cipher in any sense; it is used to obscure spoilers, joke punchlines, and occasionally to confuse casual observers.
- **`tr` (translate):** A command that performs character-by-character substitution on its input. It takes two sets of equal length and maps each character in the first set to the corresponding character in the second set. It is powerful for tasks like case conversion, character deletion, and simple cipher work.
- **Character set expansion in `tr`:** Ranges like `A-Z` and `a-z` expand to their full alphabetic sequences. This makes building a substitution table concise - `tr 'A-Za-z' 'N-ZA-Mn-za-m'` encodes the entire ROT13 mapping in a single readable expression.
- **Classical cryptography awareness:** While ROT13 and Caesar ciphers are trivially broken, understanding substitution ciphers builds intuition for more sophisticated classical schemes (Vigenère, Playfair) and helps with frequency analysis - a foundational concept in cryptanalysis.

---

## Approach

Running `cat data.txt` returned text that was clearly readable in structure but made no semantic sense - a reliable sign of a simple substitution cipher. The level description confirmed it was ROT13.

The `tr` command is the standard Linux tool for this. The key is constructing the two character sets correctly. ROT13 maps:

- `A–M` → `N–Z` and `N–Z` → `A–M` (uppercase)
- `a–m` → `n–z` and `n–z` → `a–m` (lowercase)

Expressed as a `tr` command, Set 1 is `A-Za-z` (the full alphabet in both cases) and Set 2 is `N-ZA-Mn-za-m` (the same alphabet shifted by 13). The `tr` utility maps each character in Set 1 to its corresponding position in Set 2, performing the ROT13 decode in a single pass.

Since ROT13 is its own inverse, the same command both encodes and decodes - which is an elegant property worth noting.

---

## Commands Used

```bash
# Read the file to observe the encoded output
cat data.txt

# Decode ROT13 using tr with the full substitution mapping
cat data.txt | tr 'A-Za-z' 'N-ZA-Mn-za-m'
```

**How the character mapping works:**

| Set 1 expands to | A B C … M N O … Z a b c … m n o … z |
| ---------------- | ----------------------------------- |
| Set 2 expands to | N O P … Z A B … M n o p … z a b … m |

Each letter in Set 1 maps to the letter directly below it in Set 2, shifting by 13 positions.

---

## Key Takeaway

`tr` is a deceptively powerful tool. Beyond cipher work, it is used in real scripts for normalising data - stripping newlines, converting case, removing unwanted characters from input streams. Understanding how character set expansion works in `tr` unlocks a wide range of text manipulation tasks that would otherwise require more verbose tools.

---

## References

- [OverTheWire Bandit](https://overthewire.org/wargames/bandit/)
- [Linux man page - `tr`](https://linux.die.net/man/1/tr)
- [Wikipedia - ROT13](https://en.wikipedia.org/wiki/ROT13)
- [Wikipedia - Caesar cipher](https://en.wikipedia.org/wiki/Caesar_cipher)
