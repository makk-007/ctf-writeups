# Bandit Level 22 → Level 23

**Platform:** OverTheWire  
**Wargame:** Bandit  
**Date Completed:** 2026-05-11  
**Tags:** `cron` `bash-scripting` `md5sum` `hashing` `script-analysis`

---

## Objective

Another cron job is running as bandit23. This time the script it executes uses a hash to generate a filename in `/tmp` where the password is stored. Understand the script, reproduce the hash it generates for bandit23, and read the password from the resulting file.

---

## Concepts Learned

- **Reading and reasoning about shell scripts:** Rather than just running commands, this level requires you to read a script, understand its logic, and simulate what it would produce when run as a different user. This is a core skill in both offensive security (understanding what malware or a cron job is doing) and defensive security (auditing automated processes).
- **`md5sum`:** Generates a 128-bit MD5 hash of its input. While MD5 is considered cryptographically broken for security-critical uses like password storage or digital signatures, it is still widely used as a checksum and for generating deterministic identifiers - exactly as it is used here to derive a unique filename.
- **`cut -d ' ' -f 1`:** The `cut` command extracts specific fields from text. `-d ' '` sets the delimiter to a space, and `-f 1` selects the first field. `md5sum` output includes the hash followed by a space and a filename - `cut` isolates just the hash.
- **Command substitution (`$(...) `):** Captures the output of a command and assigns it to a variable or uses it inline. This is a fundamental Bash construct for building dynamic scripts.
- **Deterministic output from fixed input:** Because `md5sum` always produces the same hash for the same input, and the input string is predictable (`I am user bandit23`), anyone who knows the algorithm can reproduce the exact filename without ever running the script as bandit23.

---

## Approach

Following the same pattern as the previous level, I checked `/etc/cron.d/` for a job associated with bandit23, then read the script it referenced. This time the script was more interesting:

```bash
#!/bin/bash

myname=$(whoami)
mytarget=$(echo I am user $myname | md5sum | cut -d ' ' -f 1)

echo "Copying passwordfile /etc/bandit_pass/$myname to /tmp/$mytarget"

cat /etc/bandit_pass/$myname > /tmp/$mytarget
```

Breaking this down line by line:

- `myname=$(whoami)` - captures the username of whoever is running the script. When run by cron as bandit23, this will be `bandit23`.
- `mytarget=$(echo I am user $myname | md5sum | cut -d ' ' -f 1)` - constructs a string `"I am user bandit23"`, hashes it with MD5, and extracts just the hex digest. This becomes the filename in `/tmp` where the password is written.
- `cat /etc/bandit_pass/$myname > /tmp/$mytarget` - writes the password into that hashed filename.

The key insight is that `md5sum` is deterministic - the same input always produces the same hash. Since I knew the script would run as bandit23, I could reproduce the exact filename by running the same hash command myself with `bandit23` substituted in:

```bash
echo I am user bandit23 | md5sum | cut -d ' ' -f 1
```

This returned the hash, which was the filename in `/tmp`. I then read the file with `cat` and retrieved the password.

---

## Commands Used

```bash
# Check system cron jobs
ls /etc/cron.d/

# Read the cron job definition for bandit23
cat /etc/cron.d/cronjob_bandit23

# Read the script being executed
cat /usr/bin/cronjob_bandit23.sh

# Reproduce the hash the script generates when run as bandit23
echo I am user bandit23 | md5sum | cut -d ' ' -f 1

# Read the password file at the generated hash path
cat /tmp/<hash_output_from_above>
```

---

## Key Takeaway

Understanding a script's logic well enough to simulate its output as a different user is a powerful technique. In real-world penetration testing, this kind of script analysis is used to predict where sensitive files are written, what filenames automated processes generate, and how to intercept or reproduce the outputs of privileged jobs - without ever needing to execute the script yourself.

---

## References

- [OverTheWire Bandit](https://overthewire.org/wargames/bandit/)
- [Linux man page - `md5sum`](https://linux.die.net/man/1/md5sum)
- [Linux man page - `cut`](https://linux.die.net/man/1/cut)
- [Bash manual - Command Substitution](https://www.gnu.org/software/bash/manual/bash.html#Command-Substitution)
- [Linux man page - `cron`](https://linux.die.net/man/8/cron)
