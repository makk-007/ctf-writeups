# What Lies Within

**Platform:** CyLab Security Academy (formerly PicoCTF)
**Category:** Forensics
**Difficulty:** Medium
**Learning Path:** Forensics (Dedicated Path) - Problem Set 3c

---

## Objective

Retrieve a flag hidden within an image using LSB steganography, reinforcing the technique and tooling introduced in St3g0.

---

## Concepts Learned

| Concept | Description |
|---|---|
| LSB Steganography (Reinforced) | Applying the Least Significant Bit hiding technique identified in the previous challenge to a new image |
| Pattern Recognition Across Challenges | Recognising when a new challenge uses the same underlying technique as a previous one and applying the established approach efficiently |
| `zsteg` (Reinforced) | Confirming `zsteg` as the reliable tool of choice for LSB analysis of PNG images |

---

## Approach

Having worked through St3g0 immediately before this challenge, the approach here was a direct application of the same technique. Standard inspection tools (strings, exiftool) were tried first as a matter of habit and to confirm the same class of hiding was in use, then `zsteg` was applied to extract the hidden content:

```
zsteg buildings.png
```

The tool returned the flag embedded via LSB steganography in the image's colour channel data.

For a detailed explanation of how LSB steganography works and why `zsteg` is the appropriate tool, refer to the [St3g0 write-up](./st3g0.md).

---

## Key Takeaway

Encountering the same technique across consecutive challenges is not redundancy but deliberate reinforcement. Recognising a steganography challenge on sight, skipping the tools that will not work, and going directly to the right instrument is itself a skill. In real investigations, efficiency matters: knowing which tool to reach for without extensive trial and error saves significant time.

---

## Reflection

The back-to-back placement of St3g0 and What Lies Within in the learning path served a clear pedagogical purpose: the first challenge introduced the concept and the tool, the second confirmed that the lesson had been absorbed. The speed with which this challenge resolved compared to St3g0 was itself a measure of progress.

---

## References

- [zsteg GitHub Repository](https://github.com/zed-0xff/zsteg)
- [St3g0 write-up](./st3g0.md)