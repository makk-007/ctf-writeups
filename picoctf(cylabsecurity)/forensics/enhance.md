# Enhance!

**Platform:** CyLab Security Academy (formerly PicoCTF)
**Category:** Forensics
**Difficulty:** Medium
**Learning Path:** Beginner's Guide to the Challenge Library (Section 5)

---

## Objective

Locate a hidden flag embedded within an SVG image file by examining its raw markup rather than its rendered visual output.

---

## Concepts Learned

| Concept | Description |
|---|---|
| SVG File Structure | SVG (Scalable Vector Graphics) files are XML-based text documents, meaning their contents are human-readable and inspectable in any text editor |
| Visual Obfuscation | Hiding data in plain sight by using a small font size or a colour that blends with the background, making it invisible to the naked eye on the rendered image |
| Text Element Inspection | Locating `<tspan>` and `<text>` elements within SVG markup to find embedded string content |
| `grep` for Pattern Matching | Using `grep` with multiple patterns to locate relevant lines across a file when content is split across multiple lines |
| Forensics Mindset | Recognising that what a file displays and what a file contains are not always the same thing |

---

## Approach

The challenge title was an immediate hint: *Enhance!* references the trope of digitally zooming into an image to reveal hidden detail. The implication was that something was present in the image but not clearly visible.

**Reading the file as text:**
Rather than opening the SVG in a browser or image viewer, I opened it directly in neovim. This was the correct first instinct for forensics challenges: inspect the raw file contents before relying on any rendering layer. SVG files are XML documents, so every element, attribute, and text node is readable as plain text.

Reading through the markup revealed that the flag was present in the file, split across multiple `<tspan>` elements. The text was rendered invisible (or nearly so) in the image due to a small font size and a fill colour chosen to blend with the background. No image processing or enhancement was needed; the data was always there in the source.

**Extracting the flag:**
Because the flag characters were distributed across multiple lines via `<tspan>` elements, a simple search for the flag prefix alone was not sufficient to capture all parts in one view. I used `grep` with multiple patterns to locate all relevant lines simultaneously:

```
grep -i "pico\|CTF\|tspan" drawing.flag.svg
```

This returned all lines containing flag fragments or the `tspan` elements wrapping them, making it straightforward to assemble the complete flag.

---

## Key Takeaway

Rendered output is not a reliable representation of file contents. An SVG, PDF, or HTML file can contain text, metadata, or embedded data that is completely invisible when viewed normally but fully accessible when the raw file is inspected. In forensics work, always examine the file at the byte or markup level before drawing conclusions from its visual appearance.

---

## Reflection

The instinct to open the file in a text editor rather than an image viewer was the right call and the right habit to build. In real digital forensics investigations, files are frequently examined for content that was deliberately or accidentally obscured in the rendered view. Steganography, hidden metadata, and invisible text are all variations of the same principle this challenge demonstrated.

---

## References

- [MDN Web Docs: SVG](https://developer.mozilla.org/en-US/docs/Web/SVG)
- [MDN Web Docs: tspan element](https://developer.mozilla.org/en-US/docs/Web/SVG/Element/tspan)
- [GNU grep Manual](https://www.gnu.org/software/grep/manual/grep.html)