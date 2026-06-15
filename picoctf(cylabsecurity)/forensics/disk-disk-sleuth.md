# Disk, Disk, Sleuth!

**Platform:** CyLab Security Academy (formerly PicoCTF)
**Category:** Forensics
**Difficulty:** Medium
**Learning Path:** Forensics (Dedicated Path) — Problem Set 2b

---

## Objective

Search a disk image for a hidden flag using a string extraction tool optimised for forensic disk analysis.

---

## Concepts Learned

| Concept | Description |
|---|---|
| `srch_strings` | A Sleuth Kit utility equivalent to `strings` but optimised for disk images, capable of searching across entire raw disk images including unallocated space |
| Disk Image String Search | Extracting readable text from a raw disk image to locate embedded or hidden content |
| Unallocated Space | Areas of a disk not currently assigned to any file, which can still contain recoverable data from previously deleted files |
| Forensic Tool Selection | Choosing the right tool for the right context: `strings` for individual files, `srch_strings` for raw disk images |

---

## Approach

This challenge built directly on the disk image workflow established in Sleuthkit Intro, extending it to the task of locating a flag within a disk image's content rather than its partition structure.

**Decompressing and verifying the disk image:**
Following the same initial steps as the previous challenge, I decompressed the gzip archive and confirmed the file type:

```
gunzip dds1-alpine.flag.img.gz
file dds1-alpine.flag.img
```

**Searching the disk image for the flag:**
Rather than mounting the image and browsing its file system manually, I used `srch_strings`, a Sleuth Kit tool functionally similar to the standard `strings` command but designed to operate efficiently on raw disk images. It searches across the entire image including file system metadata and unallocated space:

```
srch_strings dds1-alpine.flag.img | grep "picoCTF"
```

This returned the flag directly, embedded as a readable string within the disk image.

---

## Key Takeaway

`srch_strings` extends the familiar `strings | grep` pattern into disk forensics. Its importance lies in its ability to search not just active file contents but the entirety of a disk image, including deleted file remnants in unallocated space. In real investigations, this technique is used to recover fragments of deleted documents, passwords, URLs, and other text-based evidence that may no longer exist as accessible files on the file system. Knowing when to use `srch_strings` over `strings` is part of selecting the right tool for the scope of the search.

---

## Reflection

The progression from Sleuthkit Intro to this challenge was well-structured: the first introduced partition analysis with `mmls`, this one introduced content searching with `srch_strings`. Together they represent the two fundamental questions in disk forensics: what is the structure of this disk, and what data does it contain? Both questions need answers before a thorough investigation can proceed.

---

## References

- [srch_strings man page](https://www.sleuthkit.org/sleuthkit/man/srch_strings.html)
- [The Sleuth Kit Documentation](https://www.sleuthkit.org/sleuthkit/)
- [Linux man page: strings](https://linux.die.net/man/1/strings)