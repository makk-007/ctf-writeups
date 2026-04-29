# Bandit Level 1 → Level 2

**Platform:** OverTheWire  
**Wargame:** Bandit  
**Date Completed:** 2025-04-28  
**Tags:** `special-characters` `file-naming` `linux-basics` `cat`

---

## Objective

Read the contents of a file whose name is a single dash (`-`) to retrieve the password for the next level. This file lives in the home directory of the bandit1 user.

---

## Concepts Learned

- **Special character filenames:** In Linux, `-` has a special meaning to many commands, it typically means "read from standard input (stdin)" rather than referring to an actual file. This makes opening a file literally named `-` non-trivial with a naive `cat -` call.
- **Explicit path references:** Prefixing a filename with `./` forces the shell to treat it as a relative path rather than a special argument. This is a small but critical habit that avoids ambiguity.
- **Standard input/output redirection:** Understanding how commands interpret `-` as stdin is foundational to shell scripting and pipeline construction, skills that matter directly in automation and exploit development.

---

## Approach

After logging in as bandit1, I ran `ls` to confirm the file existed in the home directory. The file was named `-`. My first instinct was to run `cat -`, but this caused the terminal to hang and wait for input, confirming that the shell was interpreting `-` as "read from stdin" rather than as a filename.

The fix was straightforward once I understood the underlying reason: by prefixing the filename with `./`, I give the shell an unambiguous relative path, forcing `cat` to treat it as a file on disk rather than a special stream indicator. Running `cat ./-` immediately printed the password.

This is a deceptively simple level, but it teaches something that trips up many newcomers: shells and command-line tools often assign special meaning to certain characters, and understanding those conventions is what separates a careful operator from someone who blindly types commands.

---

## Commands Used

```bash
# List directory contents to confirm the file exists
ls

# Attempt that demonstrates the problem (hangs waiting for stdin)
# cat -

# Correct approach: use an explicit relative path
cat ./-
```

---

## Key Takeaway

Never assume a filename is safe to pass directly to a command. Files with special characters in their names are common in CTF environments and can appear in the wild too, for example in log files or automated output. The `./` prefix is a reliable way to disambiguate a filename from a shell or command option.

---

## References

- [OverTheWire Bandit](https://overthewire.org/wargames/bandit/)
- [GNU Coreutils - `cat`](https://www.gnu.org/software/coreutils/manual/coreutils.html#cat-invocation)
- [Linux man page - `cat`](https://linux.die.net/man/1/cat)
