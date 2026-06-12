# Static ain't always noise

**Platform:** CyLab Security Academy (formerly PicoCTF)
**Category:** General Skills
**Difficulty:** Easy
**Learning Path:** General Skills (Dedicated Path)

---

## Objective

Analyse a statically linked binary using a provided disassembly script to locate a flag embedded within it.

---

## Concepts Learned

| Concept | Description |
|---|---|
| Static Binaries | Executables compiled with all required libraries bundled in, rather than relying on shared libraries at runtime |
| Disassembly | The process of converting compiled machine code back into human-readable assembly instructions |
| File Permissions for Execution | Using `chmod +x` to grant a script execute permission before running it |
| `strings` Command | A utility that extracts printable character sequences from a binary or any file, useful for quickly surfacing readable text |
| Combining Tools with Pipelines | Using `grep` to filter the output of `strings` for relevant content |

---

## Approach

The challenge title is a play on words: "static" refers both to the static binary provided and to the idea that not all "noise" (raw binary data) is meaningless. The provided files were the static binary itself and a bash script, `ltdis.sh`.

**Understanding the disassembly script:**
Before running anything, I opened `ltdis.sh` in neovim to understand its purpose. The script was a disassembly helper, designed to take a binary as an argument and output its disassembled instructions in a more readable format than running a raw disassembler directly.

**Preparing and running the script:**
The script did not have execute permissions by default, so the first step was granting them:

```
chmod +x ltdis.sh
```

With execute permission granted, I ran the script against the provided static binary:

```
./ltdis.sh static
```

This produced a disassembly of the binary, which is useful for understanding program logic at the instruction level. However, for this particular challenge, a full disassembly was more detail than necessary to locate the flag.

**Extracting the flag with strings:**
Rather than reading through the entire disassembly output, I used the `strings` command directly on the binary to extract all printable text sequences, then filtered the output for the flag prefix:

```
strings static | grep -i pico
```

This immediately surfaced the flag without requiring a deep dive into the assembly output.

---

## Key Takeaway

The `strings` command is one of the fastest and most underrated tools in binary analysis. Before reaching for a disassembler or debugger, running `strings` on a binary often reveals useful information directly: hardcoded flags, file paths, error messages, library names, and sometimes even credentials. It is almost always the correct first step when examining an unfamiliar binary, since it requires no setup and provides immediate signal about what the binary might contain.

---

## Reflection

This challenge was a useful reminder that the most powerful approach is not always the most complex one. While the provided `ltdis.sh` script and full disassembly are valuable tools for deeper binary analysis, the flag here was retrievable with a simple `strings | grep` pipeline. Knowing when a lightweight tool is sufficient, versus when a deeper disassembly or debugging session is required, is itself a skill worth developing. The title's pun, that static data is not just meaningless noise, captures this well: even a quick scan of a binary's printable strings can reveal exactly what you are looking for.

---

## References

- [Linux man page: strings](https://linux.die.net/man/1/strings)
- [Linux man page: chmod](https://linux.die.net/man/1/chmod)
- [GNU Binutils: objdump and disassembly tools](https://www.gnu.org/software/binutils/)