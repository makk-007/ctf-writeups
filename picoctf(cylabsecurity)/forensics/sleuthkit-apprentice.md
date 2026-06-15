# Sleuthkit Apprentice

**Platform:** CyLab Security Academy (formerly PicoCTF)
**Category:** Forensics
**Difficulty:** Medium
**Learning Path:** Forensics (Dedicated Path) — Problem Set 2d

---

## Objective

Locate a flag file across multiple partitions in a disk image by systematically searching each partition's file system and correctly identifying the target file among similar-looking entries.

---

## Concepts Learned

| Concept | Description |
|---|---|
| Multi-Partition Disk Images | Disk images containing more than one partition, each requiring separate analysis |
| Searching Across Partitions | Applying `fls` independently to each partition offset to ensure full coverage of the disk |
| Reallocated Inodes | Inodes marked as reallocated indicate a file entry that has been reused after a previous file was deleted; the active file is the one without a realloc marker |
| Inode Disambiguation | Distinguishing between multiple entries for the same filename by checking allocation status before extracting content |

---

## Approach

This challenge consolidated all the Sleuthkit skills developed across the previous three challenges and added a layer of ambiguity that required careful interpretation of tool output.

**Decompressing and verifying the disk image:**
```
gunzip disk.flag.img.gz
file disk.flag.img
```

**Mapping the partition layout:**
```
mmls disk.flag.img
```

The output revealed three Linux partitions, each with its own starting offset. Unlike the previous challenges which involved a single partition, this required searching all three.

**Searching each partition for the flag file:**
I ran `fls` recursively against each Linux partition offset, filtering for any file with "flag" in its name:

```
fls -o <partition_offset> -r disk.flag.img | grep -i "flag"
```

This was repeated for all three partition offsets. One partition returned results for a flag-related file, but two entries appeared: one standard inode entry and one marked as reallocated.

**Interpreting the realloc marker:**
A realloc marker indicates that the inode was previously used by a deleted file and has since been reassigned. The active, current file is the entry without the realloc marker. Selecting the wrong entry would recover deleted content rather than the intended file.

**Extracting the flag:**
Using the inode number of the non-reallocated entry, I retrieved the file contents with `icat`:

```
icat -o <partition_offset> disk.flag.img <inode_number>
```

This printed the flag.

---

## Key Takeaway

Real disk images typically contain multiple partitions, and a thorough forensic investigation must account for all of them. Skipping a partition risks missing evidence entirely. The realloc detail in this challenge also introduces an important concept: disk forensics tools surface both active and deleted file remnants, and an analyst must correctly distinguish between them. Mistaking a reallocated inode for the active file would recover stale or irrelevant data. Reading tool output carefully, rather than acting on the first result returned, is a discipline this challenge reinforced well.

---

## Reflection

Sleuthkit Apprentice felt like a genuine capstone for the disk forensics problem set. It required applying every tool introduced across the series (`mmls`, `fls`, `icat`) while adding the complication of multiple partitions and ambiguous results. The realloc marker was a deliberate trap for anyone who acted too quickly on the first matching result. Slowing down to read the full output before choosing an inode was the decisive step. That kind of careful, methodical interpretation is what separates effective forensic analysis from rushed guesswork.

---

## References

- [fls man page](https://www.sleuthkit.org/sleuthkit/man/fls.html)
- [icat man page](https://www.sleuthkit.org/sleuthkit/man/icat.html)
- [mmls man page](https://www.sleuthkit.org/sleuthkit/man/mmls.html)
- [The Sleuth Kit Documentation](https://www.sleuthkit.org/sleuthkit/)