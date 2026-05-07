# Bandit Level 14 → Level 15

**Platform:** OverTheWire  
**Wargame:** Bandit  
**Date Completed:** 2026-05-07  
**Tags:** `netcat` `tcp` `networking` `ports` `raw-connections`

---

## Objective

Submit the current level's password to port 30000 on localhost over a raw TCP connection. The service will respond with the password for Level 15.

---

## Concepts Learned

- **`nc` (Netcat):** Often called the "Swiss Army knife of networking," Netcat opens raw TCP or UDP connections between two endpoints. It can act as a client connecting to a port or as a listener waiting for incoming connections. It is used extensively in CTFs, penetration testing, reverse shells, and network debugging.
- **Raw TCP connections:** Unlike HTTP or SSH, a raw TCP connection has no application-layer protocol. You send bytes; you receive bytes. This makes Netcat useful for interacting with custom services, testing open ports, and crafting manual protocol exchanges.
- **Ports as service endpoints:** Each port number on a host maps to a specific listening service. Port 30000 here hosts a custom challenge service that validates the submitted password. Understanding ports is foundational to networking - and to how attackers enumerate targets.
- **Interactive vs scripted Netcat use:** In this level Netcat is used interactively - you connect, type the password, and read the response. In offensive security contexts, Netcat is often used non-interactively in pipelines (e.g., `echo "data" | nc host port`) to automate exchanges.

---

## Approach

The level description indicated that submitting the current password to port 30000 on localhost would return the next one. I had not used Netcat before, so I read its man page to understand the basic syntax before attempting the connection.

The command is straightforward: `nc localhost 30000` opens a TCP connection to that port. Once connected, I pasted the bandit14 password and pressed Enter. The service validated it and responded immediately with the password for Level 15.

This level is simple in execution but significant in concept - it introduces the idea of interacting with a network service directly at the TCP layer, without the abstraction of a browser or protocol-specific client. That raw interaction model is central to how security professionals probe unknown services during assessments.

---

## Commands Used

```bash
# Open a raw TCP connection to port 30000 on localhost
nc localhost 30000

# Once connected, paste the current level's password and press Enter
# The service responds with the next password
```

---

## Key Takeaway

Netcat is an essential tool in any security professional's toolkit. The ability to open a raw TCP connection and interact with a service manually - before writing any code or using a specialised client - is invaluable for understanding what a service does, debugging network issues, and probing targets during penetration tests.

---

## References

- [OverTheWire Bandit](https://overthewire.org/wargames/bandit/)
- [Linux man page - `nc`](https://linux.die.net/man/1/nc)
- [GTFOBins - Netcat](https://gtfobins.github.io/gtfobins/nc/)
