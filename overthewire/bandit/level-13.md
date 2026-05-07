# Bandit Level 13 → Level 14

**Platform:** OverTheWire  
**Wargame:** Bandit  
**Date Completed:** 2026-05-07  
**Tags:** `ssh` `ssh-keys` `scp` `file-permissions` `authentication`

---

## Objective

Instead of a password, the home directory of bandit13 contains a private SSH key that grants access to bandit14. Use it to log in as bandit14 and read the password stored at the known system path for that level.

---

## Concepts Learned

- **SSH key-based authentication:** Rather than a password, SSH can authenticate using a cryptographic key pair - a private key held by the user and a public key registered on the server. The server verifies that the connecting party holds the private key without the key ever being transmitted. This is the standard authentication method for servers in production environments.
- **`ssh -i`:** The `-i` flag specifies an identity file (private key) for SSH authentication. Without it, SSH falls back to password authentication or checks default key locations like `~/.ssh/id_rsa`.
- **`scp` (Secure Copy Protocol):** Transfers files securely between hosts over SSH. The `-P` flag specifies a non-standard port. It uses the same authentication mechanisms as SSH, making it straightforward to use once you have valid credentials.
- **`chmod 600` on private keys:** SSH deliberately refuses to use a private key file that is readable by anyone other than the owner. The `600` permission (`rw-------`) ensures only the owning user can read or write the file. This is a security enforcement - a world-readable private key is effectively compromised.
- **Adapting to blocked routes:** When the direct approach fails, stepping back to understand why and finding an alternative path is a core problem-solving skill in security work.

---

## Approach

After logging in as bandit13, I ran `ls` and found `sshkey.private`. My first attempt was the most direct one: use the key to SSH into bandit14 on localhost with `ssh -i sshkey.private bandit14@localhost -p 2220`. This failed - connecting via localhost was blocked.

Rather than giving up, I reconsidered the problem. The key was valid; the issue was the route. From a second terminal, while still authenticated as bandit13 on the game server, I used `scp` to pull the private key from the server to my local machine, authenticating as bandit13. This gave me the key locally.

From there the path was straightforward: set the correct permissions on the downloaded key with `chmod 600`, then SSH directly to the game server as bandit14 using the key as the credential. Once logged in, I read the password from its known path with `cat`.

The key lesson here was not the commands themselves but the mindset - when a direct route is blocked, understand the constraint and find a path that works around it.

---

## Commands Used

```bash
# Check contents of the home directory
ls

# First attempt - connecting via localhost (blocked)
# ssh -i sshkey.private bandit14@localhost -p 2220

# From a second local terminal: pull the private key using scp
scp -P 2220 bandit13@bandit.labs.overthewire.org:~/sshkey.private ./bandit14.private

# Set correct permissions - SSH will reject keys readable by others
chmod 600 bandit14.private

# SSH directly to the game server using the private key
ssh -i bandit14.private -p 2220 bandit14@bandit.labs.overthewire.org

# Read the password for this level
cat /etc/bandit_pass/bandit14
```

---

## Key Takeaway

SSH key authentication is the standard for securing remote access in professional environments - passwords alone are considered insufficient for most production systems. Understanding how to handle private key files correctly, including permission requirements and transfer via `scp`, is a fundamental skill for any systems or security role.

---

## References

- [OverTheWire Bandit](https://overthewire.org/wargames/bandit/)
- [Linux man page - `ssh`](https://linux.die.net/man/1/ssh)
- [Linux man page - `scp`](https://linux.die.net/man/1/scp)
- [Linux man page - `chmod`](https://linux.die.net/man/1/chmod)
- [OpenSSH - Public Key Authentication](https://www.openssh.com/manual.html)
