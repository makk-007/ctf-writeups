# Sleuthkit Intro

**Platform:** CyLab Security Academy (formerly PicoCTF)
**Category:** Forensics
**Difficulty:** Medium
**Learning Path:** Forensics (Dedicated Path) - Problem Set 2a

---

## Objective

Analyse a compressed disk image using digital forensics tools to identify the size of a Linux partition, then submit the result to a remote verification service to retrieve the flag.

---

## Concepts Learned

| Concept | Description |
|---|---|
| Disk Image Files | Binary copies of an entire storage device or partition, used in forensics to analyse storage media without altering the original |
| Gzip Compression | A common file compression format; `.gz` files are decompressed using `gunzip` |
| The Sleuth Kit (TSK) | An open-source collection of command-line tools for digital forensics and disk image analysis |
| `mmls` | A Sleuth Kit tool that reads and displays the partition layout of a disk image, including partition types, offsets, and sizes |
| Partition Tables | Structures on a disk that define the boundaries, sizes, and types of each partition |
| Remote Flag Verification | Submitting a computed answer to a netcat service that validates the result and returns a flag |

---

## Approach

This challenge introduced The Sleuth Kit, a professional digital forensics toolkit, and marked a clear step up in tooling complexity from the earlier image-based challenges.

**Verifying and decompressing the disk image:**
The provided file was a gzip-compressed disk image. I first confirmed its type with the `file` command, then decompressed it:

```
gunzip disk.img.gz
```

This produced the raw disk image file ready for analysis.

**Reading the partition layout with mmls:**
The Sleuth Kit's `mmls` tool reads the partition table of a disk image and displays each partition's type, starting offset, ending offset, and size in sectors:

```
mmls disk.img
```

The output listed all partitions on the disk. I identified the Linux partition by its type identifier and read its size in sectors from the corresponding column.

**Submitting the answer:**
With the partition size identified, I connected to the remote verification service using netcat and submitted the value:

```
nc saturn.picoctf.net 59881
```

The service validated the answer and returned the flag.

---

## Key Takeaway

`mmls` is typically one of the first commands run when a disk image is received during a forensic investigation. Before examining file systems or recovering data, an analyst needs to understand the disk's partition structure: how many partitions exist, what type each is, where they start and end, and how large they are. This structural overview informs every subsequent step of the investigation. The Sleuth Kit is a standard toolkit in the digital forensics field and appears in professional investigations alongside commercial tools like Autopsy.

---

## Reflection

This challenge marked the beginning of disk forensics within the learning path and introduced a qualitatively different class of tools. Working with disk images rather than individual files requires thinking about storage at a lower level: partitions, offsets, sectors, and file systems rather than files and directories. The remote validation mechanic also reinforced that forensic analysis often produces a specific, verifiable answer rather than an open-ended finding.

---

## References

- [The Sleuth Kit Documentation](https://www.sleuthkit.org/sleuthkit/man/)
- [mmls man page](https://www.sleuthkit.org/sleuthkit/man/mmls.html)
- [Linux man page: gunzip](https://linux.die.net/man/1/gunzip)