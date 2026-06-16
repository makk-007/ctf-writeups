# Disk, Disk, Sleuth! II

**Platform:** CyLab Security Academy (formerly PicoCTF)
**Category:** Forensics
**Difficulty:** Medium
**Learning Path:** Forensics (Dedicated Path) - Problem Set 2c

---

## Objective

Locate and retrieve a specific named file from within a disk image by navigating its file system using Sleuth Kit tools, without mounting the image.

---

## Concepts Learned

| Concept | Description |
|---|---|
| Partition Offset | The starting sector of a partition within a disk image, required to tell file system tools where a partition begins |
| `fls` | A Sleuth Kit tool that lists files and directories within a disk image partition, similar to `ls` but operating directly on the image |
| Inode Number | A unique identifier assigned to each file in a Unix file system, used to reference a file independently of its name or path |
| `icat` | A Sleuth Kit tool that reads and outputs the contents of a file from a disk image using its inode number |
| Recursive File Listing | Using the `-r` flag with `fls` to list all files across all subdirectories within a partition |
| Targeted File Recovery | Locating a specific file by name within a disk image and extracting its contents without mounting the image |

---

## Approach

This challenge introduced a more targeted approach to disk forensics: rather than searching the entire image for a string, the task was to locate a specific named file and read its contents.

**Decompressing and verifying the disk image:**
```
gunzip dds2-alpine.flag.img.gz
file dds2-alpine.flag.img
```

**Finding the partition offset:**
To work with the file system inside the disk image, I first needed to identify where the Linux partition began. `mmls` provided the partition layout including the starting sector, which is the offset value required by subsequent Sleuth Kit commands:

```
mmls dds2-alpine.flag.img
```

**Locating the target file:**
With the partition offset identified, I used `fls` with the recursive flag to list all files within the partition and filtered the output for the known filename:

```
fls -o 2048 -r dds2-alpine.flag.img | grep "down-at-the-bottom.txt"
```

The `-o 2048` flag tells `fls` where the partition starts within the disk image. The output returned the file's entry along with its inode number.

**Reading the file contents:**
Using the inode number returned by `fls`, I extracted the file's contents directly from the disk image with `icat`:

```
icat -o 2048 dds2-alpine.flag.img 18291
```

This printed the contents of `down-at-the-bottom.txt`, which contained the flag.

---

## Key Takeaway

`fls` and `icat` together form the core file recovery workflow in Sleuth Kit: `fls` finds files and their inode numbers within a partition, and `icat` retrieves file contents by inode. This approach works even when a file system cannot be mounted, when files have been deleted (inodes may still be recoverable), or when working in environments where mounting disk images is not possible or appropriate. The partition offset, obtained from `mmls`, is the critical linking value that ties these tools together correctly.

---

## Reflection

This challenge represented a meaningful step up in the Sleuthkit series. Rather than a single-command search, it required chaining three tools in sequence: `mmls` to find the offset, `fls` to locate the file and its inode, and `icat` to read the content. This multi-step pipeline mirrors how real disk forensics investigations proceed: establish structure first, then navigate, then extract. Each tool's output feeds the next, which is a pattern that appears throughout professional forensic toolkits.

---

## References

- [fls man page](https://www.sleuthkit.org/sleuthkit/man/fls.html)
- [icat man page](https://www.sleuthkit.org/sleuthkit/man/icat.html)
- [mmls man page](https://www.sleuthkit.org/sleuthkit/man/mmls.html)
- [The Sleuth Kit Documentation](https://www.sleuthkit.org/sleuthkit/)