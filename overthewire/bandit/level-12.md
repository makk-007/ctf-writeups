# Bandit Level 12 → Level 13

**Platform:** OverTheWire  
**Wargame:** Bandit  
**Date Completed:** 2026-05-06  
**Tags:** `hexdump` `compression` `gzip` `bzip2` `tar` `file-forensics` `xxd`

---

## Objective

`data.txt` is a hexdump of a file that has been compressed multiple times using various compression formats. Reverse the hexdump, then iteratively decompress until you reach a plain ASCII file containing the password for Level 13.

---

## Concepts Learned

- **Hexdumps and `xxd -r`:** A hexdump is a human-readable representation of binary data in hexadecimal. `xxd` creates hexdumps; `xxd -r` reverses the process, converting hex back to binary. This is fundamental in binary analysis and forensics - hexdumps appear in memory dumps, packet captures, and firmware images.
- **`file` command (revisited):** When you do not know what format a binary is in, `file` reads its magic bytes (the first few bytes that identify the format) and tells you. This is the correct way to identify files when extensions are absent or misleading - a common situation in CTFs and incident response.
- **`mktemp -d`:** Creates a temporary directory with a unique name. Using a temp directory keeps your work isolated and avoids polluting the home directory - good hygiene, especially in shared or production environments.
- **Compression formats - `gzip`, `bzip2`, `tar`:** Three of the most common compression and archiving tools on Linux. Each has its own magic bytes, file extension convention, and decompression command. Encountering all three in sequence in this level builds the muscle memory to handle each one.
- **Iterative decompression:** Real-world forensic artefacts are sometimes compressed or packed multiple times, either deliberately (to slow analysis) or as a result of automated archiving pipelines. The workflow here - `file` → rename → decompress → repeat - is exactly the process used in malware unpacking and archive forensics.
- **Renaming before decompression:** Tools like `gzip` and `bzip2` rely on file extensions to determine format. Renaming a file to the correct extension before decompressing is necessary when the extension is missing or wrong.

---

## Approach

This was the most involved level so far. I started by creating a temporary working directory with `mktemp -d` and copying `data.txt` into it, keeping the original untouched.

The first step was reversing the hexdump. Running `xxd -r data.txt > dataConverted` produced a binary file. I immediately ran `file dataConverted` to identify it - gzip. I renamed it to `dataConverted.gz` and decompressed it with `gzip -d`.

What followed was a repeated cycle: run `file` on the output, identify the format, rename with the correct extension, decompress, and repeat. The formats I encountered in sequence were: gzip → bzip2 → gzip → tar → tar → (further iterations) → plain ASCII text.

Each time I hit a new format the process was the same - the only variable was which decompression command to call:

- **gzip:** `gzip -d filename.gz`
- **bzip2:** `bzip2 -d filename.bz2`
- **tar:** `tar -xf filename.tar`

After enough iterations, `file` finally reported ASCII text, and `cat` revealed the password.

I noted after finishing that the entire renaming and decompression loop could potentially be automated with a shell script - something I intend to revisit as my scripting skills develop.

---

## Commands Used

```bash
# Create a temporary working directory and move into it
mktemp -d
cd /tmp/tmp.XXXXXXXX

# Copy the original file into the temp directory
cp ~/data.txt .

# Reverse the hexdump to recover the binary
xxd -r data.txt > dataConverted

# Identify the file type using magic bytes
file dataConverted

# --- Repeat the following cycle until file reports ASCII text ---

# Rename to the correct extension (gzip example)
mv dataConverted dataConverted.gz

# Decompress gzip
gzip -d dataConverted.gz

# Rename and decompress bzip2
mv dataConverted dataConverted.bz2
bzip2 -d dataConverted.bz2

# Rename and extract tar archive
mv dataConverted dataConverted.tar
tar -xf dataConverted.tar

# Check type after each step
file dataConverted

# --- End cycle ---

# Read the final plaintext file
cat dataConverted
```

---

## Reflection

The repetitive nature of this level made it obvious that manual iteration does not scale. A shell script that loops - checking the file type, applying the right decompression tool, and repeating until the output is plain text - would reduce this to a single command. Writing that script is on my list for future practice.

---

## Key Takeaway

The `file` command and iterative decompression are core skills in binary forensics and malware analysis. Packed or multiply-compressed files are a common evasion technique - malware loaders often wrap a payload in several layers of compression or encoding to slow down automated scanners and manual analysis. Working through this level builds the exact instincts needed to unpack them methodically.

---

## References

- [OverTheWire Bandit](https://overthewire.org/wargames/bandit/)
- [Linux man page - `xxd`](https://linux.die.net/man/1/xxd)
- [Linux man page - `file`](https://linux.die.net/man/1/file)
- [Linux man page - `gzip`](https://linux.die.net/man/1/gzip)
- [Linux man page - `bzip2`](https://linux.die.net/man/1/bzip2)
- [Linux man page - `tar`](https://linux.die.net/man/1/tar)
- [Linux man page - `mktemp`](https://linux.die.net/man/1/mktemp)
