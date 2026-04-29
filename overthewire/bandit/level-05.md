# Bandit Level 5 → Level 6

**Platform:** OverTheWire  
**Wargame:** Bandit  
**Date Completed:** 2025-04-28  
**Tags:** `find` `file-attributes` `size-filtering` `permissions` `pipeline`

---

## Objective

Locate a specific file hidden somewhere within the `inhere` directory tree. The file meets all three of the following criteria: it is human-readable, exactly 1033 bytes in size, and not executable. Read it to get the password for Level 6.

---

## Concepts Learned

- **`find` with multiple conditions:** The `find` command supports chaining multiple filter flags in a single invocation. This level requires combining `-type f` (regular file), `-size` (exact byte count), `! -executable` (not executable), and `-exec file {} +` with a `grep` pipe for human-readability, all at once.
- **File size units in `find`:** The `c` suffix in `-size 1033c` specifies bytes (characters). Other units include `k` for kilobytes and `M` for megabytes. Getting this right matters when you need precise file matching.
- **File permissions and executability:** The `! -executable` flag filters out files that have the executable bit set. This is relevant to security because executable permissions determine what can be run on a system, a common post-exploitation check is scanning for world-writable or unexpectedly executable files.
- **Absolute path with `cat`:** Using the full path in `cat` means you do not have to navigate into deeply nested directories manually, useful for efficiency and for scripting.
- **When to use external resources:** Recognising when your current knowledge does not cover a task and seeking reliable documentation is a professional skill. Using man pages and reputable guides is part of the workflow.

---

## Approach

After logging in as bandit5, I ran `ls` to see the `inhere` directory, then began thinking about how to filter for a file matching three simultaneous criteria across multiple subdirectories.

I knew from the previous level that `find` and `file` could work together. This level added the additional constraints of file size and permissions. Rather than approaching them separately, I looked up how to combine multiple flags in `find` and how to specify an exact byte count using the `c` suffix.

The resulting one-liner handles everything in a single pass: it searches the `inhere` directory recursively for regular files, filters for exactly 1033 bytes, excludes executable files, runs `file` on each match, and pipes through `grep` to confirm human-readability. Only one file satisfied all conditions.

I noted that this level pushed me to look up `find` flags more carefully and that a broader Linux study would strengthen my efficiency on future levels. The key insight was that `find` is not just a search tool, it is a filter and executor that can replace many manual steps.

---

## Commands Used

```bash
# Single command satisfying all three conditions
find inhere -type f -size 1033c ! -executable -exec file {} + | grep text

# Read the file using its absolute path (no need to cd into it)
cat /path/to/inhere/subdirectory/filename
```

---

## Key Takeaway

Mastering `find` is one of the highest-leverage investments in Linux proficiency. In penetration testing and incident response, the ability to quickly locate files by size, permission, type, or modification time across large file systems is essential, and `find` handles all of it. One well-constructed command replaces dozens of manual steps.

---

## References

- [OverTheWire Bandit](https://overthewire.org/wargames/bandit/)
- [Linux man page - `find`](https://linux.die.net/man/1/find)
- [GNU Findutils Documentation](https://www.gnu.org/software/findutils/manual/html_mono/find.html)
- [Linux man page - `file`](https://linux.die.net/man/1/file)
