# Bandit Level 10 → Level 11

**Platform:** OverTheWire  
**Wargame:** Bandit  
**Date Completed:** 2026-05-06  
**Tags:** `base64` `encoding` `decoding` `linux-basics`

---

## Objective

The password for Level 11 is stored in `data.txt`, which contains Base64-encoded data. Decode it to retrieve the password.

---

## Concepts Learned

- **Base64 encoding:** A method of representing binary data using only printable ASCII characters. It is not encryption - it is encoding. Base64 is widely used to transmit binary content over text-based channels (email attachments, JWTs, HTTP headers, embedded images in HTML). Recognising Base64 output (typically a long string of alphanumeric characters ending in `=` or `==`) is a core CTF and real-world security skill.
- **`base64 -d`:** The `-d` flag instructs the `base64` utility to decode rather than encode. Without it, the tool encodes whatever you feed it.
- **Encoding vs encryption:** A common misconception is that encoded data is protected data. Base64 offers zero confidentiality - anyone with the encoded string and the knowledge that it is Base64 can decode it in seconds. This distinction matters enormously in security: credentials stored as Base64 in config files or source code are effectively plaintext.

---

## Approach

After logging in I ran `cat data.txt`, which returned a long string of alphanumeric characters with a trailing `=`. The `=` padding and character set are characteristic signatures of Base64 encoding. There was no ambiguity about what encoding was in use.

The `base64` command is built into Linux and accepts a `-d` flag for decoding. I passed `data.txt` directly as the argument and it printed the decoded content - including the password - to stdout in one step.

This level is straightforward technically, but the concept it introduces - recognising common encoding schemes on sight - is one of the most frequently tested skills in CTF competitions and real-world security assessments.

---

## Commands Used

```bash
# Read the file to confirm it contains Base64-encoded content
cat data.txt

# Decode the Base64 content
base64 -d data.txt
```

---

## Key Takeaway

Base64 is not security. Seeing it in a configuration file, HTTP header, or source code repository should always prompt you to decode and inspect the contents. Credentials, tokens, and private keys are routinely found Base64-encoded in places developers assumed would never be read by an attacker.

---

## References

- [OverTheWire Bandit](https://overthewire.org/wargames/bandit/)
- [Linux man page - `base64`](https://linux.die.net/man/1/base64)
- [RFC 4648 - The Base16, Base32, and Base64 Data Encodings](https://datatracker.ietf.org/doc/html/rfc4648)
