# St3g0

**Platform:** CyLab Security Academy (formerly PicoCTF)
**Category:** Forensics
**Difficulty:** Medium
**Learning Path:** Forensics (Dedicated Path) - Problem Set 3b

---

## Objective

Extract a flag hidden within a PNG image using LSB (Least Significant Bit) steganography techniques and a dedicated steganography analysis tool.

---

## Concepts Learned

| Concept | Description |
|---|---|
| Steganography | The practice of concealing data within an ordinary-looking carrier file (image, audio, video) without visibly altering it |
| LSB Steganography | A technique that hides data by replacing the least significant bit of each pixel's colour channel value with bits of the secret message, producing imperceptible visual changes |
| `zsteg` | A Ruby-based tool for detecting and extracting data hidden in PNG and BMP images using various steganographic encoding schemes |
| Limits of Basic Forensics Tools | Understanding when `strings`, `exiftool`, and `grep` are insufficient and a specialised tool is required |
| Colour Channel Encoding | How pixel data is structured in RGB channels, and how the least significant bits of those channels can carry hidden information |

---

## Approach

Initial attempts using familiar tools did not yield results, which was itself informative: when `strings` and `exiftool` return nothing useful, the data is likely hidden using a technique that does not rely on plaintext embedding or metadata fields.

**What LSB steganography is:**
Every pixel in a PNG image is represented by colour channel values (Red, Green, Blue, and sometimes Alpha). Each value is stored as a byte. The least significant bit of each byte contributes the smallest possible change to the colour: flipping it shifts a colour value by just one unit out of 256, a change completely invisible to the human eye. By replacing these least significant bits across many pixels with the bits of a hidden message, data can be concealed in an image with no perceptible visual difference.

Detecting this requires a tool that knows to look at and interpret these bit patterns rather than scanning for readable strings.

**Extracting the hidden data:**
`zsteg` automates the detection and extraction of LSB-encoded data in PNG and BMP files. Running it against the provided image:

```
zsteg pico.flag.png
```

The tool scanned the image's colour channels for recognisable encoded content and returned the hidden flag directly.

---

## Key Takeaway

LSB steganography is one of the most widely used techniques for hiding data in images because the visual changes it produces are genuinely imperceptible without specialised analysis. Tools like `zsteg` and `stegsolve` exist precisely because manual detection is not feasible. In forensics and CTF contexts, reaching for a steganography-specific tool is the correct move when standard inspection methods return nothing. Knowing which tool addresses which hiding technique is as important as knowing the techniques themselves.

---

## Reflection

This challenge marked a genuine conceptual expansion in the forensics path. Every previous challenge involved data that was accessible through some form of direct inspection: reading raw bytes, searching strings, or navigating a file system. LSB steganography is different because the hidden data is structurally embedded in the image in a way that is designed to be invisible to standard analysis. Recognising when to escalate to a specialised tool, rather than continuing to retry familiar tools, was the key skill this challenge developed.

---

## References

- [zsteg GitHub Repository](https://github.com/zed-0xff/zsteg)
- [Wikipedia: Steganography](https://en.wikipedia.org/wiki/Steganography)
- [Wikipedia: Least significant bit steganography](https://en.wikipedia.org/wiki/Steganography#Digital_messages)