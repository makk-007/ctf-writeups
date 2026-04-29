# Bandit Level 4 → Level 5

**Platform:** OverTheWire  
**Wargame:** Bandit  
**Date Completed:** 2025-04-28  
**Tags:** `file-types` `human-readable` `find` `scripting-efficiency`

---

## Objective

Among multiple files in the `inhere` directory, identify the only human-readable (ASCII text) file and read its contents to get the password for Level 5. All filenames begin with a dash.

---

## Concepts Learned

- **`file` command:** Determines the type of a file by examining its contents rather than its name. This is crucial in security work, file extensions can be faked, but the `file` command reads the magic bytes at the head of a file to determine what it actually contains.
- **ASCII vs binary data:** Human-readable text files contain printable ASCII characters. Other files may contain binary data, which looks like garbled output when you `cat` them. Knowing how to distinguish between the two protects you from corrupting your terminal.
- **Looping with `find` and `-exec`:** Rather than running `file` on each of nine files manually, you can chain `find` with `-exec` to process all matching files in a single command. This is a building block of efficient shell scripting.
- **Piping with `grep`:** Filtering the output of `file` through `grep` for the word "text" quickly narrows down results to the human-readable file.

---

## Approach

After navigating into `inhere`, I found nine files all named with a dash prefix (`-file00` through `-file08`). Only one contained human-readable text; the others held binary data.

My initial approach was to use `file ./-file00`, then repeat the command for each file individually, checking them one by one. This worked but was inefficient. I noted in my journal that I wanted to find a better method.

The efficient approach is to use `find` with `-exec` and pipe the output to `grep`. The command `find . -type f -exec file {} + | grep text` runs the `file` command on every file in the directory at once and filters for the one described as containing ASCII text. This eliminates the need to manually check each file and is the kind of one-liner that matters in time-sensitive CTF or live engagement scenarios.

The `{}` is a placeholder that `find` replaces with each matched filename, and `+` groups the results into a single `file` invocation rather than calling it separately for every file.

---

## Commands Used

```bash
# Navigate into the directory
cd inhere

# Check all files and filter for human-readable text in one command
find . -type f -exec file {} + | grep text

# Read the identified human-readable file
cat ./-fileXX   # replace XX with the number identified above
```

---

## Reflection

My initial approach of opening files one by one was functional but slow. The key lesson I took from this level was to invest in understanding `find` and `-exec` piping. In a real engagement, a directory might contain hundreds of files, manual inspection does not scale.

---

## Key Takeaway

The `file` command is indispensable when dealing with unknown files. In digital forensics and malware analysis, never trust a file extension, always verify the actual content type. Combining `find`, `-exec`, and `grep` into a single pipeline is a foundational scripting pattern worth memorising.

---

## References

- [OverTheWire Bandit](https://overthewire.org/wargames/bandit/)
- [Linux man page - `file`](https://linux.die.net/man/1/file)
- [Linux man page - `find`](https://linux.die.net/man/1/find)
- [Bash Handbook - Pipes and Redirects](https://www.gnu.org/software/bash/manual/bash.html#Pipelines)
