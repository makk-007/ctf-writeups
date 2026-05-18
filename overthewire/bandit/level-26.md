# Bandit Level 26 → Level 27

**Platform:** OverTheWire  
**Wargame:** Bandit  
**Date Completed:** 2026-05-12  
**Updated:** 2026-05-18  
**Tags:** `vim` `shell-escape` `restricted-shell` `privilege-escalation` `setuid`

---

## Objective

Continuing directly from Level 25 - you are inside vim, having escaped the `more` pager. Use vim's built-in command execution capabilities to spawn an interactive shell as bandit26, then use a setuid binary to retrieve the password for Level 27.

---

## Concepts Learned

- **Vim as a shell escape vector:** Vim is far more than a text editor. Its command mode supports executing arbitrary shell commands, setting environment variables, and spawning interactive shells. When vim is reachable from a restricted environment, it is almost always a viable escape route. This is well documented on GTFOBins and is tested regularly in CTF and real-world restricted shell scenarios.
- **`:set shell`:** A vim command that defines which shell binary vim uses when executing shell commands or spawning a shell. The default may be `/bin/sh` or whatever the current `$SHELL` is set to - but it can be overridden to any accessible shell binary.
- **`:shell`:** A vim command that spawns an interactive shell using whatever is set as `shell`. Because this shell runs in the context of the current process (bandit26), you inherit that user's environment and privileges.
- **Setuid binaries (revisited):** As introduced in Level 19, a setuid binary runs with the privileges of its owner rather than the caller. The `bandit27-do` binary in this level is owned by bandit27 and has the setuid bit set - meaning it can read bandit27's password file even though you are logged in as bandit26.
- **Separation of levels 25 and 26:** Level 25 ends with entering vim via `more`. Level 26 begins inside vim. They are treated as separate write-ups because the skills involved are distinct - one is about understanding restricted shells and pager behaviour, the other is about vim command execution and privilege escalation via a setuid binary.

---

## Approach

This level picks up exactly where Level 25 left off - inside vim, having pressed `v` while `more` was paused. At this point I was in vim running as bandit26 but without an interactive shell.

Vim's command mode allowed me to set the shell it would use and then spawn it. I entered command mode with `:` and ran:

```
:set shell=/bin/bash
```

This told vim to use `/bin/bash` for any shell operations, overriding the restricted `showtext` script that would otherwise be inherited. I then ran:

```
:shell
```

This spawned a fully interactive bash session as bandit26. From there I ran `ls -la` and found the `bandit27-do` binary - a setuid executable owned by bandit27. Using it to run `cat` on the bandit27 password file returned the password with bandit27's effective privileges.

> **Note:** An earlier version of this write-up incorrectly read the bandit26 password directly from `/etc/bandit_pass/bandit26`. While that file is readable as bandit26, the actual goal of this level is to retrieve the password for Level 27 using the `bandit27-do` setuid binary. This was corrected after attempting to log into Level 27 confirmed the original approach was wrong.

---

## Commands Used

```bash
# --- Inside vim (entered via more escape from Level 25) ---

# Set the shell vim will use
:set shell=/bin/bash

# Spawn an interactive shell
:shell

# --- Now in an interactive bash session as bandit26 ---

# Check the home directory for available binaries
ls -la

# Use the setuid binary to read the bandit27 password
./bandit27-do cat /etc/bandit_pass/bandit27
```

---

## Key Takeaway

Vim is one of the most powerful shell escape vectors available. Any environment that grants access to vim - whether through a misconfigured `sudo` rule, a pager escape, or a restricted shell - should be treated as a full shell escape. Once inside a shell, always enumerate the environment carefully: the setuid binary here was the intended next step, and missing it meant retrieving the wrong password entirely. Careful enumeration prevents that mistake.

---

## References

- [OverTheWire Bandit](https://overthewire.org/wargames/bandit/)
- [GTFOBins - vim](https://gtfobins.github.io/gtfobins/vim/)
- [Vim documentation - `:shell`](https://vimdoc.sourceforge.net/htmldoc/various.html#:shell)
- [Vim documentation - `:set shell`](https://vimdoc.sourceforge.net/htmldoc/options.html#'shell')
- [Wikipedia - Setuid](https://en.wikipedia.org/wiki/Setuid)
