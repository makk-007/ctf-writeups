# Bandit Level 25 → Level 26

**Platform:** OverTheWire  
**Wargame:** Bandit  
**Date Completed:** 2026-05-12  
**Tags:** `ssh` `restricted-shell` `more` `pager` `passwd` `shell-escape`

---

## Objective

An SSH key for bandit26 is available in the bandit25 home directory. However, bandit26 uses a non-standard login shell that immediately closes the session. Identify what that shell does, find a way to pause it before it exits, and break out into an interactive session.

---

## Concepts Learned

- **`/etc/passwd` and login shells:** Every user account on a Linux system has a login shell defined in `/etc/passwd`. The format of each entry is `username:x:uid:gid:comment:home_directory:shell`. The final field defines what runs when that user logs in. It does not have to be `bash` or `sh` - it can be any executable, including a custom script.
- **`more`:** A terminal pager - a program that displays text one screenful at a time and waits for user input before continuing. Crucially, `more` only pauses if the content exceeds the height of the terminal window. If the terminal is tall enough to display everything at once, `more` exits immediately without waiting.
- **Terminal size as an attack surface:** Because `more` pauses only when the content overflows the terminal, shrinking the terminal window below the height of the displayed text forces `more` into interactive mode. This is the prerequisite for the escape that follows.
- **Vim invocation from `more`:** When `more` is paused waiting for input, pressing `v` launches the default editor - typically `vim` - on the file being displayed. This is documented behaviour, not an exploit, but it is a classic shell escape technique because vim is a powerful environment with its own command execution capabilities.
- **`scp` for key retrieval:** As established in Level 13, `scp` allows files to be pulled from the game server to the local machine using existing SSH credentials - useful when the login shell prevents interactive use.

---

## Approach

After logging in as bandit25, I found the SSH private key for bandit26 in the home directory. Before using it I investigated what would happen at login by checking bandit26's entry in `/etc/passwd`:

```
bandit26:x:11026:11026:bandit level 26:/home/bandit26:/usr/bin/showtext
```

The login shell was not `bash` - it was `/usr/bin/showtext`. I read that script:

```bash
#!/bin/sh
export TERM=linux
exec more ~/text.txt
exit 0
```

The script uses `exec more ~/text.txt` to display a text file through the `more` pager, then exits. Because `exec` replaces the current process rather than spawning a child, when `more` finishes the entire session ends. If the terminal is large enough to display `text.txt` in one screen, `more` exits instantly - which is why the session closes immediately on a normal-sized terminal.

The solution was to make the terminal small enough that `more` could not display the file in one screen and was forced to pause and wait. I shrunk my terminal window vertically until `more` stopped and showed its prompt.

With `more` paused, I pressed `v` to launch vim. From inside vim I had access to a full command execution environment - which is where Level 26 picks up.

I also used `scp` to pull the private key to my local machine, set `chmod 600` on it, and used it to SSH in with the small terminal ready.

---

## Commands Used

```bash
# Check the home directory for the bandit26 key
ls

# Identify bandit26's login shell
cat /etc/passwd | grep bandit26

# Read the custom shell script
cat /usr/bin/showtext

# Pull the private key to the local machine
scp -P 2220 bandit25@bandit.labs.overthewire.org:~/bandit26.sshkey ./bandit26.private

# Set correct permissions on the key
chmod 600 bandit26.private

# Shrink the terminal window vertically before connecting
# (forces more to pause instead of exiting immediately)

# Connect as bandit26
ssh -i bandit26.private -p 2220 bandit26@bandit.labs.overthewire.org

# Once more is paused, press v to enter vim
```

**The `showtext` script explained:**

```bash
#!/bin/sh
export TERM=linux       # set terminal type
exec more ~/text.txt    # replace the shell process with more - when more exits, the session ends
exit 0                  # never reached due to exec
```

---

## Key Takeaway

Login shell restrictions are a real access control mechanism - and `more`, `less`, `vi`, and `man` are all well-known escape vectors when they appear in restricted environments. Checking `/etc/passwd` to understand what shell a user runs is a standard step in both privilege escalation enumeration and account auditing. The lesson here is that any interactive pager or editor reachable from a restricted context is a potential breakout point.

---

## References

- [OverTheWire Bandit](https://overthewire.org/wargames/bandit/)
- [Linux man page - `more`](https://linux.die.net/man/1/more)
- [Linux man page - `passwd` (file format)](https://linux.die.net/man/5/passwd)
- [GTFOBins - more](https://gtfobins.github.io/gtfobins/more/)
- [GTFOBins - vim](https://gtfobins.github.io/gtfobins/vim/)
