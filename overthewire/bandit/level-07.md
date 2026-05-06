# Bandit Level 7 → Level 8

**Platform:** OverTheWire  
**Wargame:** Bandit  
**Date Completed:** 2026-05-05  
**Tags:** `grep` `text-search` `large-files` `wc` `pipeline`

---

## Objective

The password for Level 8 is stored in `data.txt`, next to the word "millionth." Search through the file and extract the relevant line.

---

## Concepts Learned

- **`grep`:** Searches a file (or stream) for lines matching a given pattern and prints only those lines. It is one of the most used tools in Linux - for log analysis, configuration auditing, and CTF challenges alike.
- **`wc -l`:** Counts the number of lines in a file. Using this before attempting `cat` on an unknown file is a good habit - it tells you immediately whether the file is human-browsable or requires a targeted search tool.
- **Scaling your approach to file size:** `cat` is appropriate for small files. For files with tens of thousands of lines, tools like `grep`, `awk`, or `sed` are necessary. Recognising which tool fits the scale of the problem is a mark of growing proficiency.

---

## Approach

After logging in, I ran `ls` and saw `data.txt`. My first instinct was to `cat` it, which printed an overwhelming wall of text. I then used `wc -l` to get a line count - over 90,000 lines. Clearly `cat` was not the right approach here.

Since the level description said the password was next to the word "millionth," `grep` was the obvious and precise solution. I passed the search term as a string pattern and the filename as the argument. `grep` scanned all 90,000+ lines and returned only the matching one instantly.

This experience reinforced an important workflow habit: always check the size or nature of a file before deciding how to read it. Reaching for `cat` on an unknown file is fine as a first attempt, but having `wc` and `grep` ready as immediate fallbacks is part of thinking efficiently at the command line.

---

## Commands Used

```bash
# Confirm location and check directory contents
pwd
ls

# Check line count before attempting to read the file directly
wc -l data.txt

# Search for the line containing the keyword
grep "millionth" data.txt
```

---

## Key Takeaway

`grep` is arguably the most important text-processing tool in Linux. In security work it appears everywhere - filtering logs for suspicious activity, extracting credentials from configuration files, and searching source code for vulnerable patterns. Knowing when to reach for it instead of `cat` is a fundamental efficiency skill.

---

## References

- [OverTheWire Bandit](https://overthewire.org/wargames/bandit/)
- [Linux man page - `grep`](https://linux.die.net/man/1/grep)
- [Linux man page - `wc`](https://linux.die.net/man/1/wc)
