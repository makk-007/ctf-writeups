# Bandit Level 9 → Level 10

**Platform:** OverTheWire  
**Wargame:** Bandit  
**Date Completed:** 2026-05-06  
**Tags:** `strings` `grep` `binary-files` `text-extraction` `pipeline`

---

## Objective

The password for Level 10 is stored in `data.txt` among a mix of binary and human-readable data. It is one of the few human-readable strings in the file and is preceded by several `=` characters. Extract it.

---

## Concepts Learned

- **`strings`:** Scans a binary file and prints sequences of printable characters above a minimum length (default: 4). It is a standard first-look tool in malware analysis and binary reverse engineering - used to extract embedded URLs, error messages, hardcoded credentials, and other readable artefacts from compiled binaries or mixed-content files.
- **Binary files and `grep`:** Running `grep` directly on a binary file returns only "Binary file matches" - it detects the binary content and refuses to print it by default. Adding the `-a` flag forces `grep` to treat the file as text, but `strings` is usually a cleaner approach for this scenario.
- **Combining `strings` with `grep`:** Piping `strings` output through `grep` lets you filter for specific patterns within the readable portions of a binary file - a technique used regularly in CTF binary challenges and real-world forensics.
- **`cat` on binary files:** Running `cat` on a binary file prints raw bytes to the terminal, producing garbled output and sometimes corrupting your terminal session. Recognising this and pivoting to `strings` is an important instinct to develop.

---

## Approach

After logging in I ran `cat data.txt`, which immediately returned garbled, unreadable output - a clear sign the file contained binary data. I then tried `grep "====" data.txt` hoping to search for the `=` prefix mentioned in the level description, but `grep` responded with "Binary file matches" and printed nothing useful.

I knew from context that what I needed was a way to extract only the human-readable portions. `strings` was the right tool - it scanned the binary content and returned only printable character sequences. However, it returned several lines containing `=` characters, not just one. Piping the `strings` output through `grep "===="` filtered the results down to the lines with multiple consecutive equals signs, and the password was among them.

---

## Commands Used

```bash
# Attempt cat - confirms file is binary (garbled output)
cat data.txt

# Attempt grep directly - returns "Binary file matches" with no useful output
grep "====" data.txt

# Extract human-readable strings from the binary file
strings data.txt

# Filter strings output for lines containing the === pattern
strings data.txt | grep "===="
```

---

## Key Takeaway

`strings` is an indispensable tool any time you are handed an unknown or binary file. In malware analysis it is often the very first command run on a suspicious executable - revealing hardcoded IPs, registry keys, function names, and error messages without needing to disassemble anything. Pairing it with `grep` makes it even more precise.

---

## References

- [OverTheWire Bandit](https://overthewire.org/wargames/bandit/)
- [Linux man page - `strings`](https://linux.die.net/man/1/strings)
- [Linux man page - `grep`](https://linux.die.net/man/1/grep)
