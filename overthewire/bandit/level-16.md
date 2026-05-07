# Bandit Level 16 → Level 17

**Platform:** OverTheWire  
**Wargame:** Bandit  
**Date Completed:** 2026-05-07  
**Tags:** `nmap` `port-scanning` `ssl` `openssl` `service-enumeration` `ssh-keys`

---

## Objective

Somewhere in the port range 31000–32000 on localhost, one service speaks SSL and will return an RSA private key when given the current password. Identify the correct port, submit the password, save the returned private key, and use it to access Level 17.

---

## Concepts Learned

- **`nmap` (Network Mapper):** The industry-standard tool for network discovery and port scanning. It can identify open ports, running services, software versions, and operating system details. It is foundational to both offensive security reconnaissance and defensive network auditing.
- **`nmap -p` (port range):** Restricts the scan to a specific range of ports rather than scanning all 65,535. Essential for targeted enumeration when you know the range of interest.
- **`nmap -sV` (version detection):** Probes open ports to determine the service name and version running on each. This is what distinguishes a port scan from service enumeration - knowing that port 31518 is running `ssl/echo` vs `ssl/unknown` tells you which one to interact with.
- **Service enumeration mindset:** Brute-forcing all 1000 ports with `openssl s_client` manually would be impractical. The correct approach is to enumerate first with `nmap`, narrow down candidates, and then interact only with relevant ports. This is exactly the methodology used in real penetration tests.
- **Saving an RSA private key:** When a service returns a private key rather than a password, that key must be written to a file, given strict permissions, and then used with SSH. The workflow is identical to Level 13.
- **`chmod 600` (revisited):** SSH enforces that private key files are not group- or world-readable. This is not optional - SSH will refuse to use the key otherwise.

---

## Approach

The challenge here was finding the right port among a range of 1000. My first instinct was to try `openssl s_client` on each port - but even at a few seconds per connection, that would be extremely slow for 1000 ports. The correct tool was `nmap`.

I started with a basic port scan across the range with `nmap -p 31000-32000 localhost` to identify which ports were open. I then added the `-sV` flag to determine which services were running SSL. The scan revealed a small number of open ports, two of which were running SSL. That narrowed the problem from 1000 ports to two.

I tested the two SSL ports using `openssl s_client -connect localhost:<port> -quiet` and submitted the current password to each. One of them echoed the password back - indicating it was a simple echo service, not the challenge service. The other returned an RSA private key - that was the one I needed.

I saved the returned key using `nano`, named the file `bandit17.private`, set the correct permissions with `chmod 600`, and the level was complete. The key would be used in the next level to authenticate as bandit17.

---

## Commands Used

```bash
# Scan the port range to find open ports
nmap -p 31000-32000 localhost

# Add version detection to identify services running SSL
nmap -p 31000-32000 -sV localhost

# Test each SSL port - submit the current password and observe the response
openssl s_client -connect localhost:<port> -quiet

# Save the returned RSA private key to a file
nano bandit17.private
# Paste the full key including headers, save and exit

# Set strict permissions so SSH accepts the key
chmod 600 bandit17.private
```

---

## Reflection

This level brought together several concepts from previous levels - SSL connections from Level 15, private key handling from Level 13, and introduced `nmap` as the right tool for service enumeration. The decision to use `nmap` instead of brute-forcing with `openssl s_client` was the key insight, and it reflects a broader principle: always enumerate intelligently before interacting.

---

## Key Takeaway

`nmap` is one of the most important tools in a security professional's arsenal. The workflow of scanning a range, identifying services with version detection, and then targeting only relevant ports is the foundation of nearly every penetration test and vulnerability assessment. Combining it with `openssl s_client` for TLS service interaction is a pattern that appears regularly in real-world engagements.

---

## References

- [OverTheWire Bandit](https://overthewire.org/wargames/bandit/)
- [Nmap Official Documentation](https://nmap.org/book/man.html)
- [Linux man page - `nmap`](https://linux.die.net/man/1/nmap)
- [OpenSSL Documentation - s_client](https://www.openssl.org/docs/manmaster/man1/openssl-s_client.html)
- [Linux man page - `chmod`](https://linux.die.net/man/1/chmod)
