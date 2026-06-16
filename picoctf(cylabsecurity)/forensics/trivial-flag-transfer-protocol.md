# Trivial Flag Transfer Protocol

**Platform:** CyLab Security Academy (formerly PicoCTF)
**Category:** Forensics
**Difficulty:** Medium
**Learning Path:** Forensics (Dedicated Path) - Problem Set 4c

---

## Objective

Reconstruct a multi-stage file transfer from a packet capture, decode hidden instructions, and extract a flag concealed within one of the transferred images using steganography.

---

## Concepts Learned

| Concept | Description |
|---|---|
| TFTP (Trivial File Transfer Protocol) | A simple, unauthenticated UDP-based file transfer protocol with no encryption, making all transferred files fully recoverable from a packet capture |
| Wireshark Object Export | A Wireshark GUI feature that reconstructs and exports files transferred over protocols like TFTP, HTTP, and FTP directly from a packet capture |
| Multi-Layer Forensics | Combining network forensics (packet analysis), encoding (ROT13), and steganography (steghide) in a single investigation |
| `steghide` | A steganography tool capable of embedding and extracting data hidden within image and audio files, optionally protected by a passphrase |
| ROT13 in Operational Context | Recognising ROT13 as the encoding used for both file contents and the passphrase, connecting the decode step to the extraction step |
| Systematic Enumeration | Testing each recovered file methodically rather than guessing which one contains the flag |

---

## Approach

This was the most complex challenge in the forensics path, requiring four distinct techniques applied in sequence.

**Step 1: Identifying the protocol:**
Running the protocol hierarchy summary confirmed that the capture contained TFTP traffic. TFTP is an unencrypted, unauthenticated protocol, meaning every file transferred is fully visible in the capture.

**Step 2: Recovering the transferred files:**
Rather than reconstructing the files manually from raw packet data, I used Wireshark's built-in object export feature, which automates the reassembly of files transferred over supported protocols:

```
File → Export Objects → TFTP → Save All
```

This recovered five files from the capture: two text files (`instructions.txt` and `plan`) and three BMP images (`picture1.bmp`, `picture2.bmp`, `picture3.bmp`).

**Step 3: Decoding the text files:**
Reading the text files revealed that their contents were ROT13-encoded. Decoding them produced readable instructions and a passphrase. The instructions described what to do with the images, and the decoded passphrase was needed for the next step.

**Step 4: Extracting the hidden flag with steghide:**
Following the decoded instructions, I applied `steghide` to each BMP image in turn, using the decoded passphrase:

```
steghide extract -sf picture1.bmp -p "PASSPHRASE"
steghide extract -sf picture2.bmp -p "PASSPHRASE"
steghide extract -sf picture3.bmp -p "PASSPHRASE"
```

One of the three images contained a hidden file, which `steghide` extracted successfully. Reading that file revealed the flag.

---

## Key Takeaway

This challenge demonstrated that real investigations rarely involve a single technique in isolation. The full solution required: network protocol analysis to identify TFTP traffic, GUI-based object reconstruction to recover transferred files, ROT13 decoding to read the instructions and passphrase, and passphrase-based steganographic extraction to retrieve the final flag. Each step depended on the output of the previous one. The ability to chain multiple techniques together, and to recognise which tool is appropriate at each stage, is the core skill that separates a methodical investigator from one who tries tools at random.

TFTP's complete lack of encryption also carries a real-world lesson: any organisation still using TFTP for file transfers is exposing every transferred file to anyone with access to the network path. This is not a theoretical risk; it is a complete absence of protection.

---

## Reflection

Trivial Flag Transfer Protocol was a fitting capstone for the forensics path. It integrated nearly every category of technique covered across the problem sets: file type analysis, encoding recognition, steganography, and network forensics. The multi-stage nature of the challenge required maintaining context across steps and using the output of each stage as the input for the next, which is exactly how complex real-world investigations unfold. Completing it without hints felt like a genuine validation of the skills built across the entire path.

---

## References

- [Wireshark: Export Objects](https://www.wireshark.org/docs/wsug_html_chunked/ChIOExportSection.html)
- [steghide Documentation](http://steghide.sourceforge.net/documentation.php)
- [Wikipedia: Trivial File Transfer Protocol](https://en.wikipedia.org/wiki/Trivial_File_Transfer_Protocol)
- [Wikipedia: ROT13](https://en.wikipedia.org/wiki/ROT13)