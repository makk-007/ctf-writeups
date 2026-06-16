# Extensions

**Platform:** CyLab Security Academy (formerly PicoCTF)
**Category:** Forensics
**Difficulty:** Medium
**Learning Path:** Forensics (Dedicated Path) - Problem Set 3a

---

## Objective

Identify a file that has been given an incorrect extension, determine its true type, rename it accordingly, and open it to retrieve the flag.

---

## Concepts Learned

| Concept | Description |
|---|---|
| File Magic Bytes | Byte sequences at the start of a file that identify its true format, independent of the filename or extension |
| `file` Command | A utility that reads a file's magic bytes to determine its actual type, ignoring the extension |
| File Extension Mismatch | A file deliberately or accidentally given an extension that does not match its actual format |
| `mv` for Renaming | Using the `mv` command to rename a file with the correct extension so it can be opened by the appropriate application |
| Image Viewers on Linux | Tools like `eog` (Eye of GNOME) used to open and display image files on Linux systems |

---

## Approach

The challenge description called the file "a really weird text file," which was an immediate signal that the extension was misleading and the file was not actually plain text.

**Determining the true file type:**
Rather than attempting to read the file as text, I ran the `file` command first:

```
file flag.txt
```

The output identified the file as a PNG image, despite the `.txt` extension. File extensions on Linux are purely cosmetic and have no effect on how the operating system handles a file. The `file` command reads the actual content to make its determination.

**Renaming the file:**
With the true type confirmed, I renamed the file to give it the correct extension:

```
mv flag.txt flag.png
```

**Viewing the image:**
Opening the renamed file with an image viewer displayed the flag visually within the image:

```
eog flag.png
```

---

## Key Takeaway

File extensions are not a reliable indicator of file type, especially in a forensics context where files may have been deliberately mislabelled. The `file` command should always be the first step when examining an unknown file, as it reads the file's actual content signatures rather than trusting the name. This principle applies broadly: a `.jpg` could be a ZIP archive, a `.pdf` could be an executable, and a `.txt` could be an image. Always verify before assuming.

---

## Reflection

This challenge reinforced a habit that has appeared consistently throughout the forensics path: verify the file type before doing anything else. Every problem set has started with `file`, and this challenge made the reason explicit by building the entire puzzle around a type mismatch. In real investigations, mislabelled files are common, whether through human error, obfuscation attempts, or malware trying to disguise itself.

---

## References

- [Linux man page: file](https://linux.die.net/man/1/file)
- [Wikipedia: List of file signatures (magic bytes)](https://en.wikipedia.org/wiki/List_of_file_signatures)
- [GNOME Eye of GNOME (eog)](https://wiki.gnome.org/Apps/EyeOfGnome)