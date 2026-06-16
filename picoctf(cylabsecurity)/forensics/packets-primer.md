# Packets Primer

**Platform:** CyLab Security Academy (formerly PicoCTF)
**Category:** Forensics
**Difficulty:** Medium
**Learning Path:** Forensics (Dedicated Path) - Problem Set 4a

---

## Objective

Analyse a packet capture file to locate a flag hidden in a raw data payload, using command-line packet analysis tools to extract and decode the hex-encoded content.

---

## Concepts Learned

| Concept | Description |
|---|---|
| PCAP Files | Packet capture files that record network traffic at the raw packet level, used in network forensics and analysis |
| `tshark` | The command-line version of Wireshark, used to read, filter, and extract data from packet captures without a GUI |
| Protocol Hierarchy Statistics | A `tshark` feature that summarises which protocols appear in a capture and at what volume, useful for identifying unusual traffic |
| Raw Data Frames | Packets carrying unstructured binary payloads with no recognised application-layer protocol |
| Hex Payload Extraction | Using `tshark` field filters to extract the raw hexadecimal payload from selected packets |
| `xxd -r -p` | A command that converts a plain hex string back into its binary or ASCII representation |

---

## Approach

This challenge introduced network forensics and packet capture analysis, a significant expansion from file and disk forensics.

**Initial inspection:**
Standard file inspection and `strings` were attempted first and did not yield useful results, which is expected for binary packet capture files where the data of interest is encapsulated within network protocol structures rather than sitting as a plaintext string in the file.

**Understanding the capture structure:**
Rather than reading packets blindly, I used `tshark` to generate a protocol hierarchy summary, which provides an overview of every protocol present in the capture and the proportion of traffic each represents:

```
tshark -r network-dump.flag.pcap -z io,phs -q
```

The output identified a raw data frame, which stood out as unusual since most legitimate application traffic conforms to a recognised protocol. This pointed directly at the packet of interest.

**Extracting the hex payload:**
With the suspicious traffic identified, I extracted the raw hex payload from the relevant packets using tshark's field extraction mode:

```
tshark -r network-dump.flag.pcap -T fields -e data
```

This returned the packet payload as a hexadecimal string.

**Decoding the payload:**
The hex string was converted back to readable text using `xxd` in reverse mode:

```
echo "hex_payload" | xxd -r -p
```

This produced the plaintext flag.

---

## Key Takeaway

Network forensics requires a different analytical approach than file forensics. The data of interest is embedded within packet structures, and navigating those structures requires tools that understand network protocols. `tshark` is the command-line equivalent of Wireshark and is essential in environments where a GUI is unavailable. The protocol hierarchy summary is an efficient first step when analysing an unfamiliar capture: it narrows the field of investigation immediately by showing what is present and what is anomalous.

---

## Reflection

The jump from disk forensics to network forensics felt significant. Previous challenges involved static files and disk images where the data was physically present on storage. Packet captures introduce the dimension of time and communication: the data represents an exchange between systems, and understanding what was communicated requires reconstructing that exchange from raw network frames. The `tshark` pipeline used here is one I expect to use repeatedly across network forensics challenges going forward.

---

## References

- [tshark man page](https://www.wireshark.org/docs/man-pages/tshark.html)
- [Wireshark Display Filters Reference](https://www.wireshark.org/docs/dfref/)
- [Linux man page: xxd](https://linux.die.net/man/1/xxd)