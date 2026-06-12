# Big Zip

**Platform:** CyLab Security Academy (formerly PicoCTF)
**Category:** General Skills
**Difficulty:** Easy
**Learning Path:** Beginner's Guide to the Challenge Library (Section 5)

---

## Objective

Extract a flag hidden somewhere within a large archive containing many nested directories and files, using recursive search rather than manual browsing.

---

## Concepts Learned

| Concept | Description |
|---|---|
| Archive Extraction | Using `unzip` to decompress a `.zip` archive and access its contents |
| Recursive Directory Search | Searching through an entire directory tree and all its subdirectories in a single command |
| `grep -r` | The recursive flag for `grep`, which searches all files under a given directory path |
| Efficiency at Scale | Recognising when manual file browsing is impractical and automation is the only sensible approach |

---

## Approach

The challenge name was a direct signal: a large zip archive containing many directories, making manual inspection impractical from the outset.

**Extraction:**
The first step was extracting the archive using `unzip`. The resulting directory structure contained a large number of subdirectories, confirming that browsing manually to find the flag would be inefficient and unreliable.

**Recursive search:**
Rather than navigating the directory tree by hand, I used `grep` with the `-r` (recursive) flag to search through the entire extracted directory and all its subdirectories simultaneously, targeting the known flag prefix:

```
grep -r "picoCTF" .
```

This scanned every file in every subdirectory in a single command and returned the file path and line containing the flag immediately.

---

## Key Takeaway

When the search space is large, manual enumeration is not a strategy. `grep -r` is one of the most practical tools in a security professional's toolkit precisely because it scales effortlessly: the same command that searches ten files searches ten thousand. In real-world investigations, searching through large codebases, log archives, or file system dumps for indicators of compromise follows exactly this pattern.

---

## Reflection

The challenge flag itself, `gr3p_15_m4g1c`, makes the lesson explicit. `grep` is one of those tools that consistently proves its value the moment the dataset grows beyond what is comfortable to browse manually. Reaching for it instinctively when faced with a large directory is the right habit to develop early.

---

## References

- [GNU grep Manual](https://www.gnu.org/software/grep/manual/grep.html)
- [Linux man page: unzip](https://linux.die.net/man/1/unzip)