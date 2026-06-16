# Information

**Platform:** CyLab Security Academy (formerly PicoCTF)
**Category:** Forensics
**Difficulty:** Easy
**Learning Path:** Forensics (Dedicated Path) - Problem Set 1a

---

## Objective

Locate a flag hidden within the metadata of a JPEG image file rather than in its visible content.

---

## Concepts Learned

| Concept | Description |
|---|---|
| File Verification with `file` | Using the `file` command to confirm a file's actual type based on its magic bytes, not just its extension |
| EXIF Metadata | Exchangeable Image File Format data embedded in image files, storing information such as camera settings, timestamps, GPS coordinates, and custom fields |
| Base64 Encoding | A method of encoding binary or text data as an ASCII string, commonly used to embed data in text-based formats |
| Base64 Decoding | Reversing base64 encoding to recover the original data using `base64 -d` |
| Metadata as a Forensic Surface | Understanding that files carry more information than their visible content, and that metadata is a common place to hide or accidentally expose sensitive data |

---

## Approach

The challenge description hinted that a file had been secretly modified, pointing toward something beyond the visible image content.

**Verifying the file type:**
Before doing anything else, I confirmed the file was a legitimate JPEG using the `file` command, which reads the file's magic bytes rather than trusting the extension:

```
file cat.jpg
```

This confirmed the file was a valid JPEG image, ruling out the possibility of a file type mismatch or disguise.

**Inspecting the raw contents:**
Opening the file in neovim allowed me to scan the raw data. While most of a JPEG's binary content is not human-readable, text embedded in the file's metadata section is visible as readable strings. This revealed a base64-encoded value embedded in the file's metadata.

**Decoding the flag:**
With the encoded value identified, I decoded it using the standard base64 utility:

```
echo "<encoded_value>" | base64 -d
```

This produced the plaintext flag directly.

---

## Key Takeaway

Image files are not just pixel data. EXIF and other metadata fields are embedded directly in the file and are fully readable by anyone who inspects the raw content. These fields can contain GPS coordinates, device identifiers, software versions, and, as this challenge demonstrates, arbitrary data including flags or sensitive information. Metadata inspection is a standard first step in image forensics, and tools like `exiftool` are purpose-built for this kind of analysis.

---

## Reflection

The `file` command as a first step is a habit worth ingraining across all forensics challenges. Confirming actual file type before proceeding prevents wasted effort on the wrong analysis path. The combination of raw file inspection in neovim followed by base64 decoding was a straightforward pipeline here, but it mirrors the approach used in real investigations where encoded or obfuscated data is embedded in otherwise normal-looking files.

---

## References

- [Linux man page: file](https://linux.die.net/man/1/file)
- [ExifTool Documentation](https://exiftool.org/)
- [Linux man page: base64](https://linux.die.net/man/1/base64)