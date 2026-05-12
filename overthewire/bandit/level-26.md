# Bandit Level 26 → Level 27

**Platform:** OverTheWire  
**Wargame:** Bandit  
**Date Completed:** 2026-05-12  
**Tags:** `vim` `shell-escape` `restricted-shell` `privilege-escalation` `setuid`

---

## Objective

Continuing directly from Level 25 - you are inside vim, having escaped the `more` pager. Use vim's built-in command execution capabilities to spawn an interactive shell as bandit26, then retrieve the password.

---

## Concepts Learned

- **Vim as a shell escape vector:** Vim is far more than a text editor. Its command mode supports executing arbitrary shell commands, setting environment variables, and spawning interactive shells. When vim is reachable from a restricted environment, it is almost always a viable escape route. This is well documented on GTFOBins and is tested regularly in CTF and real-world restricted shell scenarios.
- **`:set shell`:** A vim command that defines which shell binary vim uses when executing shell commands or spawning a shell. The default may be `/bin/sh` or whatever the current `$SHELL` is set to - but it can be overridden to any accessible shell binary.
- **`:shell`:** A vim command that spawns an interactive shell using whatever is set as `shell`. Because this shell runs in the context of the current process (bandit26), you inherit that user's environment and privileges.
- **Vim command mode:** Accessed by pressing `:` from normal mode. Commands typed here are interpreted by vim's internal scripting engine rather than sent to the terminal. This is the mechanism that makes the escape possible.
- **Separation of levels 25 and 26:** Level 25 ends with entering vim via `more`. Level 26 begins inside vim. They are treated as separate write-ups because the skills involved are distinct - one is about understanding restricted shells and pager behaviour, the other is about vim command execution and shell spawning.

---

## Approach

This level picks up exactly where Level 25 left off - inside vim, having pressed `v` while `more` was paused. At this point I was in vim running as bandit26, but without an interactive shell.

Vim's command mode allowed me to set the shell it would use and then spawn it. I entered command mode with `:` and ran:

```
:set shell=/bin/bash
```

This told vim to use `/bin/bash` for any shell operations, overriding the restricted `showtext` script that would otherwise be inherited. I then ran:

```
:shell
```

This spawned a fully interactive bash session as bandit26. From there I was in a normal shell environment and could read the password directly from the standard password file location.

---

## Commands Used

```bash
# --- Inside vim (entered via more escape from Level 25) ---

# Set the shell vim will use
:set shell=/bin/bash

# Spawn an interactive shell
:shell

# --- Now in an interactive bash session as bandit26 ---

# Read the current level's password
cat /etc/bandit_pass/bandit26

# Read the password for the next level using the setuid binary
ls -la
./bandit27-do cat /etc/bandit_pass/bandit27
```

---

## Key Takeaway

Vim is one of the most powerful shell escape vectors available. Any environment that grants access to vim - whether through a misconfigured `sudo` rule, a pager escape, or a restricted shell - should be treated as a full shell escape. `:set shell` and `:shell` are the two commands every security professional should know. GTFOBins maintains a comprehensive list of similar escapes for editors, pagers, and other common binaries.

---

## References

- [OverTheWire Bandit](https://overthewire.org/wargames/bandit/)
- [GTFOBins - vim](https://gtfobins.github.io/gtfobins/vim/)
- [Vim documentation - `:shell`](https://vimdoc.sourceforge.net/htmldoc/various.html#:shell)
- [Vim documentation - `:set shell`](https://vimdoc.sourceforge.net/htmldoc/options.html#'shell')
