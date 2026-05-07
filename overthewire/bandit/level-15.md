# Bandit Level 15 → Level 16

**Platform:** OverTheWire  
**Wargame:** Bandit  
**Date Completed:** 2026-05-07  
**Tags:** `openssl` `tls` `ssl` `encrypted-connections` `s_client`

---

## Objective

Submit the current level's password to port 30001 on localhost - but this time the connection must be made over SSL/TLS. The service will respond with the password for Level 16.

---

## Concepts Learned

- **SSL/TLS:** Transport Layer Security (TLS), formerly SSL, is the cryptographic protocol that secures most internet traffic - HTTPS, SMTPS, IMAPS, and many others. It wraps a plaintext TCP connection in encryption, ensuring confidentiality and integrity. Understanding TLS is essential for any security professional.
- **`openssl s_client`:** A diagnostic tool included in the OpenSSL toolkit that acts as a TLS client. It connects to a server, performs the TLS handshake, and then allows interactive or scripted exchange over the encrypted channel - much like Netcat, but with TLS on top.
- **Why Netcat is insufficient here:** Plain `nc` sends and receives raw, unencrypted bytes. A port protected by TLS will not respond meaningfully to unencrypted input - the TLS handshake must happen first. `openssl s_client` handles that handshake automatically.
- **The OpenSSL toolkit:** A broad collection of cryptographic utilities covering key generation, certificate management, hashing, encoding, and protocol testing. Familiarity with it is directly applicable to certificate troubleshooting, TLS configuration auditing, and security assessments.

---

## Approach

Having solved the previous level with plain Netcat, this level introduced a layer of encryption. The port (30001) was running the same type of challenge service, but over TLS. I read up on the OpenSSL toolkit - specifically the `s_client` subcommand - to understand how to connect to a TLS-protected service from the command line.

The `-connect` flag takes a `host:port` argument. Once the TLS handshake completed, the terminal was ready for input just as with Netcat. I submitted the current password and the service returned the next one.

This level draws a clear line between the previous one - it shows exactly why encryption matters at the transport layer. The same interaction model applies, but the underlying channel is now confidential and authenticated.

---

## Commands Used

```bash
# Connect to port 30001 over TLS using openssl s_client
openssl s_client -connect localhost:30001

# Once connected and the handshake completes, paste the current password
# The service responds with the next password
```

---

## Key Takeaway

`openssl s_client` is the standard command-line tool for inspecting and interacting with TLS-protected services. In real-world security work it is used to test certificate validity, check supported cipher suites, and verify TLS configurations - skills directly relevant to web application security assessments and infrastructure hardening.

---

## References

- [OverTheWire Bandit](https://overthewire.org/wargames/bandit/)
- [OpenSSL Documentation - s_client](https://www.openssl.org/docs/manmaster/man1/openssl-s_client.html)
- [Linux man page - `openssl`](https://linux.die.net/man/1/openssl)
