# Wireshark doo dooo do doo...

**Platform:** CyLab Security Academy (formerly PicoCTF)
**Category:** Forensics
**Difficulty:** Medium
**Learning Path:** Forensics (Dedicated Path) - Problem Set 4b

---

## Objective

Extract a ROT13-encoded flag from plain text data embedded in a packet capture by filtering for suspicious traffic and applying a character substitution cipher to decode it.

---

## Concepts Learned

| Concept | Description |
|---|---|
| `data-text-lines` Protocol Filter | A Wireshark and tshark filter that targets packets containing HTTP or TCP payloads with plain text content |
| Plain Text Protocol Identification | Recognising that certain protocols transmit data unencrypted, making payload content readable directly from a capture |
| ROT13 Encoding | A Caesar cipher that shifts each letter 13 positions in the alphabet; applying it twice returns the original text |
| `tr` for ROT13 Decoding | Using the `tr` command with character range substitution to decode ROT13 directly in the terminal |
| Recognising Encoded Output | Identifying that readable but nonsensical text in a packet payload is likely encoded rather than corrupted |

---

## Approach

**Initial inspection:**
As with the previous challenge, standard file inspection and `strings` did not yield useful results from the packet capture file directly.

**Analysing the protocol hierarchy:**
Running the protocol hierarchy summary with `tshark` identified `data-text-lines` traffic as suspicious. This filter targets packets carrying plain text payloads, which in a forensics context often means unencrypted application data worth examining closely:

```
tshark -r network-dump.flag.pcap -z io,phs -q
```

**Extracting the plain text payload:**
Using `tshark` with a display filter targeting the `data-text-lines` protocol and extracting the text field directly:

```
tshark -r shark1.pcapng -Y "data-text-lines" -T fields -e text
```

The output was readable text but clearly encoded: the characters were alphabetic but the content was nonsensical, which is a strong indicator of a simple substitution cipher rather than corruption or binary data.

**Decoding with ROT13:**
The pattern was consistent with ROT13, a Caesar cipher that shifts each letter by 13 positions. Since the alphabet has 26 letters, ROT13 is its own inverse: applying it once encodes, applying it again decodes. I decoded the extracted payload using `tr`:

```
echo "encoded_flag" | tr 'A-Za-z' 'N-ZA-Mn-za-m'
```

This produced the plaintext flag.

---

## Key Takeaway

Plain text protocols in packet captures are a significant forensic surface because their payloads are immediately readable without decryption. Filtering for `data-text-lines` traffic is an efficient way to surface human-readable content in a capture that would otherwise be obscured by the volume of binary protocol data. The additional layer of ROT13 encoding in this challenge reflects a realistic scenario: data transmitted over a network may be lightly obfuscated even when the transport itself is unencrypted, and recognising common encoding schemes on sight is a valuable skill.

---

## Reflection

Combining network traffic analysis with encoding recognition made this a more layered challenge than Packets Primer. The two-step process of identifying the right traffic and then decoding its content mirrors how real network investigations often proceed: first isolate the suspicious stream, then analyse what it actually contains. The ROT13 connection back to the Bandit wargame (Level 11) was also a satisfying reminder that these techniques recur across different contexts.

---

## References

- [tshark man page](https://www.wireshark.org/docs/man-pages/tshark.html)
- [Wireshark Display Filters: data-text-lines](https://www.wireshark.org/docs/dfref/)
- [Linux man page: tr](https://linux.die.net/man/1/tr)
- [Wikipedia: ROT13](https://en.wikipedia.org/wiki/ROT13)