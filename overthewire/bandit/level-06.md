# Bandit Level 6 → Level 7

**Platform:** OverTheWire  
**Wargame:** Bandit  
**Date Completed:** 2026-05-05  
**Tags:** `find` `file-ownership` `stderr-redirection` `linux-permissions`

---

## Objective

Locate a file stored somewhere on the server that is owned by user `bandit7`, belongs to group `bandit6`, and is exactly 33 bytes in size. Read it to retrieve the password for Level 7.

---

## Concepts Learned

- **File ownership in Linux:** Every file has both an owning user and an owning group. The `find` command can filter by both using `-user` and `-group` flags - essential knowledge for privilege escalation research and file system auditing.
- **`find` from the root directory:** Searching from `/` scans the entire file system. This is more powerful than searching from a subdirectory but also generates many "Permission denied" errors for directories the current user cannot read.
- **Stderr redirection with `2>/dev/null`:** File descriptor 2 is stderr (standard error). Redirecting it to `/dev/null` discards all error messages, leaving only useful output. This is one of the most practical shell techniques for cleaning up noisy command output.
- **File descriptors:** Understanding that Linux uses numbered descriptors - 0 for stdin, 1 for stdout, and 2 for stderr - is foundational to shell scripting and pipeline construction.

---

## Approach

The level description made clear the file could be anywhere on the system, so I navigated to the root directory with `cd /` to start a global search. Running `find` with the `-user`, `-group`, and `-size` flags in combination gave me the right filter. However, my first attempt without stderr redirection returned a massive flood of "Permission denied" messages that buried the actual result.

I had recently completed a short introductory Unix course on boot.dev, where I had learned about file descriptors and stderr redirection. That knowledge clicked here immediately: appending `2>/dev/null` to the command silenced all the permission errors and left only the path to the target file. I then used `cat` with that path to read the password.

This level reinforced why stderr redirection is not just a cosmetic trick - it is a practical necessity when running broad searches across a system where your user lacks read access to many directories.

---

## Commands Used

```bash
# Confirm current location
pwd

# Move to root to search the entire file system
cd /

# Find the file matching all three criteria, silencing permission errors
find / -user bandit7 -group bandit6 -size 33c 2>/dev/null

# Read the located file
cat /path/returned/by/find
```

---

## Key Takeaway

`2>/dev/null` is an essential tool whenever a command generates noisy error output that obscures useful results. In real-world security work - whether running `find` during post-exploitation enumeration or scanning for misconfigurations - clean output is the difference between spotting the signal and missing it entirely.

---

## References

- [OverTheWire Bandit](https://overthewire.org/wargames/bandit/)
- [Linux man page - `find`](https://linux.die.net/man/1/find)
- [Bash manual - Redirections](https://www.gnu.org/software/bash/manual/bash.html#Redirections)
- [boot.dev - Introduction to Unix-like Systems](https://www.boot.dev)
