# Bandit Level 18 → Level 19

**Platform:** OverTheWire  
**Wargame:** Bandit  
**Date Completed:** 2026-05-08  
**Tags:** `ssh` `bashrc` `command-execution` `shell-bypass` `persistence`

---

## Objective

The `.bashrc` file on the bandit18 account has been modified to log you out immediately upon login. Retrieve the password stored in the `readme` file in the home directory without ever getting an interactive shell.

---

## Concepts Learned

- **`.bashrc`:** A shell initialisation script that runs automatically every time an interactive Bash session starts. It is commonly used to set aliases, environment variables, and custom prompts. Adversaries and CTF designers alike can abuse it to execute arbitrary commands at login - including one that terminates the session immediately.
- **Non-interactive SSH command execution:** SSH can run a single command on a remote host and return the output without ever dropping into an interactive shell. The syntax is `ssh user@host command`. This is widely used in automation, scripting, and - as demonstrated here - bypassing a hostile shell environment.
- **Shell bypass techniques:** When an interactive shell is restricted or sabotaged, there are several approaches to still get work done: passing commands directly via SSH, using `scp` to retrieve files, or specifying an alternative shell with `ssh -t bash`. Knowing multiple bypass routes is a practical skill in both CTF play and real-world scenarios involving restricted shells.
- **Persistence via shell initialisation files:** Modifying `.bashrc`, `.bash_profile`, or `.profile` is a known persistence technique used by attackers to execute malicious code every time a user logs in. Recognising this as a threat vector is directly applicable to defensive security and incident response.

---

## Approach

My first step was to confirm the behaviour by attempting a normal SSH login. As expected, the session opened and closed almost instantly - the modified `.bashrc` was running an `exit` command on login.

Rather than trying to fight the `.bashrc`, I bypassed it entirely by passing the commands I needed directly as SSH arguments. SSH supports appending a command after the connection parameters, which executes that command on the remote host and returns the output - without ever starting an interactive shell, and therefore without triggering `.bashrc`.

I first ran `ssh user@host ls` to confirm the `readme` file was present in the home directory, then ran `ssh user@host cat readme` to read its contents. The password was returned directly to my local terminal.

---

## Commands Used

```bash
# Confirm the immediate logout behaviour
ssh -p 2220 bandit18@bandit.labs.overthewire.org

# Run a command on the remote host without an interactive shell
ssh -p 2220 bandit18@bandit.labs.overthewire.org ls

# Read the readme file before the session can terminate
ssh -p 2220 bandit18@bandit.labs.overthewire.org cat readme
```

---

## Key Takeaway

Non-interactive SSH command execution is an essential technique - both for automation and for operating in environments where the interactive shell is restricted or hostile. The abuse of `.bashrc` for persistence is a real-world attacker technique, and knowing how to recognise and work around it is directly relevant to incident response, shell escape challenges, and restricted shell bypass scenarios encountered in penetration testing.

---

## References

- [OverTheWire Bandit](https://overthewire.org/wargames/bandit/)
- [Linux man page - `ssh`](https://linux.die.net/man/1/ssh)
- [Bash manual - Bash Startup Files](https://www.gnu.org/software/bash/manual/bash.html#Bash-Startup-Files)
