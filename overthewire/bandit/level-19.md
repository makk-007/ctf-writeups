# Bandit Level 19 → Level 20

**Platform:** OverTheWire  
**Wargame:** Bandit  
**Date Completed:** 2026-05-08  
**Tags:** `setuid` `linux-permissions` `privilege-escalation` `binary-exploitation`

---

## Objective

A setuid binary named `bandit20-do` is present in the home directory. Use it to read the password file for bandit20, which is normally only accessible to that user.

---

## Concepts Learned

- **Setuid (Set User ID):** A special permission bit on an executable that causes it to run with the privileges of the file's _owner_ rather than the user who executed it. When `bandit20-do` is owned by bandit20 and has the setuid bit set, anyone who runs it effectively runs it as bandit20 - regardless of who they are logged in as.
- **`ls -la` and permission strings:** The permission string on a setuid binary looks like `-rwsr-xr-x`. The `s` in the owner execute position (where `x` would normally appear) indicates the setuid bit is set. Recognising this in a permission listing is important in both privilege escalation and security auditing.
- **Privilege escalation via setuid binaries:** Setuid binaries are one of the most studied privilege escalation vectors in Linux. Legitimate uses include `sudo`, `passwd`, and `ping` - programs that need elevated access for specific tasks. Misconfigured or custom setuid binaries, however, are a common source of local privilege escalation vulnerabilities.
- **`whoami`:** A simple command that prints the effective user identity of the current process. Running it through the setuid binary confirms that the binary truly executes with bandit20's privileges - a useful verification step before using it for the actual task.

---

## Approach

After logging in I ran `ls -la` to inspect the directory and its contents carefully, including permissions. The `bandit20-do` binary stood out - its permission string contained an `s` in the owner execute position, and it was owned by bandit20. This is the setuid bit.

Before using it I ran `./bandit20-do` without any arguments to read its usage instructions. It behaved like a command runner - it accepted a command as an argument and executed it with bandit20's effective privileges.

To verify this I ran `./bandit20-do whoami`, which returned `bandit20` despite me being logged in as bandit19. With that confirmed, I used the binary to read bandit20's password file directly: `./bandit20-do cat /etc/bandit_pass/bandit20`.

I noted that the setuid concept still felt unfamiliar and planned to read more about it - particularly how it relates to privilege escalation techniques in real-world Linux environments.

---

## Commands Used

```bash
# Inspect the directory - note the setuid bit on the binary
ls -la

# Check how the binary is used
./bandit20-do

# Verify effective privilege - should return bandit20
./bandit20-do whoami

# Use the binary to read the protected password file
./bandit20-do cat /etc/bandit_pass/bandit20
```

**Identifying the setuid bit in a permission string:**

```
-rwsr-xr-x
    ^
    s in the owner execute position = setuid bit is set
    The binary runs as its owner (bandit20), not the caller (bandit19)
```

---

## Reflection

The setuid mechanism is one I want to understand more deeply. In CTF and real-world penetration testing, finding a misconfigured setuid binary on a target system is often the path to root. Tools like `find / -perm -4000 2>/dev/null` are used specifically to enumerate setuid binaries across a file system during privilege escalation enumeration - a technique I plan to explore further.

---

## Key Takeaway

Setuid binaries are a fundamental Linux privilege escalation vector. Any security professional doing Linux assessments must be able to identify setuid permissions in `ls -la` output and understand how they can be abused. This level is a clean, controlled introduction to a concept that becomes significantly more complex - and consequential - in real engagement scenarios.

---

## References

- [OverTheWire Bandit](https://overthewire.org/wargames/bandit/)
- [Linux man page - `chmod`](https://linux.die.net/man/1/chmod)
- [Linux Privilege Escalation - GTFOBins](https://gtfobins.github.io/)
- [Wikipedia - Setuid](https://en.wikipedia.org/wiki/Setuid)
