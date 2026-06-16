# Glory of the Garden

**Platform:** CyLab Security Academy (formerly PicoCTF)
**Category:** Forensics
**Difficulty:** Easy
**Learning Path:** Forensics (Dedicated Path) - Problem Set 1b

---

## Objective

Extract a flag appended to the end of a JPEG image file by searching its raw content for readable strings.

---

## Concepts Learned

| Concept | Description |
|---|---|
| File Verification with `file` | Confirming a file's actual type by reading its magic bytes rather than relying on the extension |
| Data Appended to Image Files | Hiding arbitrary data by appending it after the valid image content, which most image viewers ignore silently |
| `strings` on Image Files | Extracting human-readable text sequences from a binary file to surface hidden or embedded content |
| `grep` for Flag Extraction | Filtering `strings` output to locate a specific pattern within a large volume of text |
| Steganography Concepts | The broader practice of concealing data within ordinary-looking files |

---

## Approach

The challenge prompt stated the file "contains more than it seems," a direct hint that content beyond the visible image was present.

**Verifying the file type:**
I confirmed the file was a genuine JPEG using the `file` command, establishing a clean starting point before deeper inspection.

**Inspecting raw contents:**
Opening the file in neovim revealed a large volume of binary and string data, as expected for a JPEG. Visually scanning through it was not practical, but it confirmed that readable text was present within the file.

**Extracting the flag:**
Rather than reading through the raw content manually, I used `strings` to extract all printable character sequences from the file and piped the output through `grep` to filter for the flag prefix:

```
strings garden.jpg | grep -i pico
```

This immediately returned the flag, which had been appended to the end of the image file after the valid JPEG data.

---

## Key Takeaway

Image files have a defined structure, but most software that reads them stops processing at the end of the valid image data and silently ignores anything that follows. This makes it trivial to append arbitrary data to an image without affecting how it displays. The `strings` command combined with `grep` is an effective and fast technique for detecting and extracting such hidden content without needing specialised steganography tools.

---

## Reflection

The approach here was very similar to what worked for Enhance! and Information: inspect the raw file rather than trusting what the rendered output shows. The specific tool differed (strings and grep here versus neovim and grep in Enhance!), but the underlying principle was identical. Building a consistent instinct to look at what a file actually contains, not just what it displays, is the core skill forensics challenges are training.

---

## References

- [Linux man page: strings](https://linux.die.net/man/1/strings)
- [Linux man page: file](https://linux.die.net/man/1/file)
- [GNU grep Manual](https://www.gnu.org/software/grep/manual/grep.html)