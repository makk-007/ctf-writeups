# Bandit Level 20 → Level 21

**Platform:** OverTheWire  
**Wargame:** Bandit  
**Date Completed:** 2026-05-11  
**Tags:** `netcat` `tcp` `background-jobs` `tmux` `networking` `concurrency`

---

## Objective

A setuid binary called `suconnect` connects to a port on localhost, reads a value from it, and if that value matches the current level's password, returns the password for Level 21. The challenge is that you must be listening on that port yourself - meaning two things need to run simultaneously.

---

## Concepts Learned

- **`nc` in listening mode (`-lp`):** The `-l` flag puts Netcat into listen mode, waiting for an incoming connection rather than initiating one. The `-p` flag specifies the port. Combined with input redirection (`<`), it serves a file's contents to whoever connects.
- **Background jobs with `&`:** Appending `&` to a command runs it as a background process, freeing the terminal for further input. This is a core shell feature for running multiple tasks in a single terminal session.
- **`tmux` (terminal multiplexer):** A tool that allows a single terminal window to be split into multiple independent panes or windows, each running its own shell. It is widely used by developers and security professionals for managing complex multi-process workflows. `Ctrl+b` followed by `%` splits the current pane vertically.
- **Client-server interaction:** This level is a practical demonstration of the client-server model - one process listens and serves data, another connects and consumes it. Understanding this interaction at the TCP level is foundational to networking and security.
- **Input redirection to a network socket:** Redirecting a file into `nc` (`nc -lp 1234 < file`) causes the file's contents to be sent to the first client that connects. This is a lightweight way to serve data over a network without any application-layer protocol.

---

## Approach

The core challenge here was that `suconnect` needed something to connect _to_ - a listener serving the bandit20 password. That meant two processes had to run at the same time: the listener and `suconnect`. I solved this two ways.

**Approach 1 - Background jobs (`&`):**
I first confirmed the location of the bandit20 password file from notes taken in a previous level. I then launched Netcat in listen mode on an unused port, redirecting the password file as input, and sent it to the background with `&`. With the terminal free, I ran `./suconnect` pointing at the same port. It connected, received the password, validated it, and printed the Level 21 password.

**Approach 2 - `tmux`:**
I wanted to visualise what was actually happening rather than relying on the background process abstraction. I opened `tmux` and split the terminal into two side-by-side panes with `Ctrl+b %`. In the left pane I ran the Netcat listener (without `&`, since it had its own pane). In the right pane I ran `./suconnect`. I could watch both sides of the interaction play out simultaneously - the listener serving the password and `suconnect` receiving and validating it.

Both approaches are equally correct. The `tmux` approach made the client-server model concrete and visible.

---

## Commands Used

```bash
# Confirm the location of the current password
ls /etc/bandit_pass/

# --- Approach 1: Background jobs ---

# Start a Netcat listener serving the password file, run in background
nc -lp 1234 < /etc/bandit_pass/bandit20 &

# Run suconnect - it connects to the listener and validates the password
./suconnect 1234


# --- Approach 2: tmux ---

# Open tmux
tmux

# Split the terminal into two vertical panes
# Ctrl+b then %

# Left pane: start the listener (no & needed - it has its own pane)
nc -lp 1234 < /etc/bandit_pass/bandit20

# Right pane: run suconnect
./suconnect 1234
```

---

## Key Takeaway

Managing concurrent processes is an essential skill at the command line. Whether through background jobs or a terminal multiplexer like `tmux`, the ability to run and coordinate multiple processes simultaneously is used constantly in security work - from running listeners alongside exploit scripts to monitoring logs while conducting active testing. `tmux` in particular is worth investing time in; it is a staple of professional security and development workflows.

---

## References

- [OverTheWire Bandit](https://overthewire.org/wargames/bandit/)
- [Linux man page - `nc`](https://linux.die.net/man/1/nc)
- [tmux man page](https://man7.org/linux/man-pages/man1/tmux.1.html)
- [Bash manual - Job Control](https://www.gnu.org/software/bash/manual/bash.html#Job-Control)
