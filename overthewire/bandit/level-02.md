# Bandit Level 2 → Level 3

**Platform:** OverTheWire  
**Wargame:** Bandit  
**Date Completed:** 2025-04-28  
**Tags:** `spaces-in-filenames` `quoting` `escaping` `linux-basics`

---

## Objective

Read the contents of a file whose name contains spaces to retrieve the password for Level 3. The file is located in the home directory of the bandit2 user.

---

## Concepts Learned

- **Spaces in filenames:** The Linux shell uses spaces as argument delimiters by default. A filename with spaces will be interpreted as multiple separate arguments unless you handle it explicitly.
- **Quoting filenames:** Wrapping a filename in double or single quotes tells the shell to treat everything inside as a single token, including any spaces.
- **Backslash escaping:** An alternative to quoting, you can escape each individual space with a backslash (`\ `) to prevent the shell from splitting the argument.
- **Tab completion:** Pressing `Tab` after typing the first few characters of a filename with spaces will auto-complete and automatically insert the necessary escape characters. This is a practical habit that prevents errors.

---

## Approach

After logging in as bandit2, I ran `ls` to confirm the filename. The file was named `spaces in this filename`. Running `cat spaces in this filename` without any quoting caused the shell to interpret this as four separate arguments, returning "No such file or directory" errors for each word.

The solution was to wrap the entire filename in quotes so the shell treats it as a single argument. I used double quotes here, but single quotes would work equally well. An alternative I was aware of was to escape each space individually with a backslash. Both approaches produce the same result.

This is a very practical skill; file and directory names with spaces are extremely common in user-generated environments (downloads folders, shared drives, etc.), and scripts that do not account for this will break unpredictably.

---

## Commands Used

```bash
# List contents to confirm the filename
ls

# Read the file using quoted filename
cat "spaces in this filename"

# Alternative: escape each space individually
cat spaces\ in\ this\ filename
```

---

## Key Takeaway

Always account for spaces when handling filenames in scripts or command-line operations. Unquoted variables containing filenames with spaces are a common source of bugs and even security vulnerabilities in shell scripts, a lesson directly applicable to writing robust Bash automation.

---

## References

- [OverTheWire Bandit](https://overthewire.org/wargames/bandit/)
- [Bash manual - Quoting](https://www.gnu.org/software/bash/manual/bash.html#Quoting)
- [Linux man page - `cat`](https://linux.die.net/man/1/cat)
