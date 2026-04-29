# Bandit Level 0 → Level 1

**Platform:** OverTheWire  
**Wargame:** Bandit  
**Date Completed:** 2025-05-28  
**Tags:** `ssh` `remote-access` `linux-basics` `port-specification`

---

## Objective

Establish an SSH connection to the Bandit game server using the provided credentials on a non-standard port, and locate the password stored in the home directory to advance to Level 1.

---

## Concepts Learned

- **SSH (Secure Shell):** A cryptographic protocol for secure remote login over an unsecured network. It is foundational to system administration and penetration testing;nearly every remote server interaction in the real world relies on it.
- **Non-standard ports:** Services do not always run on default ports. Bandit runs SSH on port 2220 instead of the standard port 22. Recognising this is important in CTFs and real-world assessments where port scanning (e.g., with `nmap`) is often needed to discover services.
- **The `-p` flag:** Allows you to specify a port when connecting via SSH. Understanding flags and options is central to effective command-line use.
- **Basic navigation:** `pwd` (print working directory) and `ls` (list directory contents) are the first tools you reach for when you land on an unfamiliar system.
- **`cat`:** Used to read the contents of a file. In security contexts this is frequently used to quickly inspect configuration files, logs, and credential stores.

---

## Approach

This level is the entry point; it is designed to verify that you can connect to the game server at all. The challenge here is not finding the password but understanding how SSH works and how to handle a service running on a non-default port.

I started by reading the level description on the OverTheWire website, which told me the hostname, username, password, and port. From there I constructed the SSH command, using the `-l` flag to specify the username and `-p` to specify port 2220. Once connected, I used `pwd` to confirm where I had landed in the file system, then `ls` to see what was present in the home directory. A file was immediately visible. I used `cat` to read its contents and retrieved the password for the next level.

The key realisation here is that in the real world, default ports are often changed for security through obscurity, and a good security professional always checks whether services are running on expected ports.

---

## Commands Used

```bash
# Connect to the remote server on a non-standard port
ssh -l bandit0 bandit.labs.overthewire.org -p 2220

# Confirm current working directory after login
pwd

# List the contents of the home directory
ls

# Read the contents of the file
cat readme
```

---

## Key Takeaway

SSH is the backbone of remote Linux administration, and knowing how to connect to services on non-standard ports is an essential skill. This level reinforces that every engagement, whether a CTF or a real penetration test, starts with reconnaissance and access establishment.

---

## References

- [OverTheWire Bandit](https://overthewire.org/wargames/bandit/)
- [SSH man page - `man ssh`](https://linux.die.net/man/1/ssh)
- [OpenSSH Documentation](https://www.openssh.com/manual.html)
