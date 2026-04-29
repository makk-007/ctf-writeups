# Bandit Level 3 → Level 4

**Platform:** OverTheWire  
**Wargame:** Bandit  
**Date Completed:** 2025-04-28  
**Tags:** `hidden-files` `find` `linux-basics` `directory-navigation`

---

## Objective

Navigate into a subdirectory called `inhere` and locate a hidden file within it to retrieve the password for Level 4.

---

## Concepts Learned

- **Hidden files in Linux:** Files and directories whose names begin with a dot (`.`) are hidden from standard `ls` output. They are widely used for configuration files (e.g., `.bashrc`, `.ssh/config`) and are a common place attackers hide artefacts during post-exploitation.
- **`ls -a` and `ls -la`:** The `-a` flag reveals hidden files. Adding `-l` gives a detailed listing including permissions, ownership, and timestamps, critical information in security investigations.
- **`find` command:** A powerful utility for locating files by name, type, size, permissions, and more. Far more flexible than `ls` alone when searching across directory trees.
- **`cd`:** Changing directories is fundamental. Understanding relative vs absolute paths becomes increasingly important as levels grow in complexity.

---

## Approach

After logging in, I ran `ls` and saw a directory named `inhere`. I navigated into it with `cd inhere`. Running a plain `ls` returned nothing, the directory appeared empty. This is the first instinct to train: when a directory looks empty, check for hidden files before assuming it actually is.

I used `find` to search for all files in the directory, including hidden ones. The hidden file was immediately revealed. I then used `cat` with the full filename (including the leading dots) to read its contents.

An equally valid approach is `ls -la`, which would have shown the hidden file directly. Both methods are worth knowing, `ls -la` is faster for a quick look, while `find` is more powerful when you need to search recursively or apply filters.

---

## Commands Used

```bash
# List the home directory to find the inhere folder
ls

# Navigate into the subdirectory
cd inhere

# Standard ls shows nothing - directory appears empty
ls

# Use find to reveal hidden files
find . -type f

# Read the hidden file (filename begins with dots)
cat ./.hidden
```

---

## Key Takeaway

Hidden files are not a security mechanism, they are only hidden from casual `ls` output. In real-world incident response and forensics, checking for dot-prefixed files in home directories, `/tmp`, and other writable locations is a standard step when hunting for attacker persistence or exfiltrated data.

---

## References

- [OverTheWire Bandit](https://overthewire.org/wargames/bandit/)
- [Linux man page - `find`](https://linux.die.net/man/1/find)
- [Linux man page - `ls`](https://linux.die.net/man/1/ls)
