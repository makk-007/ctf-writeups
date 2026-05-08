# Bandit Level 17 → Level 18

**Platform:** OverTheWire  
**Wargame:** Bandit  
**Date Completed:** 2026-05-08  
**Tags:** `diff` `file-comparison` `text-processing` `unified-diff`

---

## Objective

Two files exist in the home directory - `passwords.old` and `passwords.new`. The password for Level 18 is the only line that changed between them. Find it.

---

## Concepts Learned

- **`diff`:** Compares two files line by line and reports what changed between them. It is one of the most used tools in software development and system administration - for reviewing configuration changes, comparing log snapshots, and understanding what was modified between two versions of a file.
- **`diff` output format:** The default output uses a compact notation. A line like `42c42` means line 42 in the first file was _changed_ to line 42 in the second. Lines prefixed with `<` are from the first file (old); lines prefixed with `>` are from the second (new). The `---` separator divides them.
- **`diff -u` (unified format):** Produces output in the unified diff format - the same format used by `git diff` and most code review tools. It shows a few lines of context around each change, with `-` marking removed lines and `+` marking added lines. This format is more human-readable and is the standard in collaborative development workflows.
- **Practical security application:** Comparing files with `diff` is a technique used in file integrity monitoring - detecting unauthorised changes to configuration files, binaries, or system files between known-good snapshots and the current state.

---

## Approach

After logging in I ran `ls` and found the two password files. The task was clear: find the one line that differed between them. With potentially hundreds of lines in each file, reading both manually was not practical.

`diff passwords.old passwords.new` reported the change immediately - one line in the old file had been replaced by a different line in the new file. The new line was the password.

I also ran `diff -u` to observe the unified format output, which presents the same information with surrounding context lines and `+`/`-` prefixes. Recognising unified diff output is a practical skill - it is the standard format for patches, pull requests, and `git diff` output encountered daily in development and security workflows.

---

## Commands Used

```bash
# Check the contents of the home directory
ls

# Compare the two files and identify what changed
diff passwords.old passwords.new

# View the same comparison in unified diff format
diff -u passwords.old passwords.new
```

**Reading the default `diff` output:**

```
42c42         → line 42 was Changed between the two files
< oldline     → the line as it appeared in passwords.old
---
> newline     → the line as it appears in passwords.new
```

**Reading the unified `diff -u` output:**

```
--- passwords.old    → the original file
+++ passwords.new    → the new file
@@ -39,7 +39,7 @@   → context: starting at line 39
 unchanged line      → context (no prefix)
-oldline             → removed from old file
+newline             → added in new file
 unchanged line      → context (no prefix)
```

---

## Key Takeaway

`diff` is an underrated but powerful tool in a security professional's workflow. File integrity monitoring - detecting whether critical system files have been tampered with - relies fundamentally on the same comparison logic. Understanding both the default and unified output formats prepares you to read patches, review code changes, and spot unauthorised modifications in configuration files.

---

## References

- [OverTheWire Bandit](https://overthewire.org/wargames/bandit/)
- [Linux man page - `diff`](https://linux.die.net/man/1/diff)
- [GNU Diffutils Manual](https://www.gnu.org/software/diffutils/manual/)
