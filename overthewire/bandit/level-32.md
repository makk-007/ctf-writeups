# Bandit Level 32 → Level 33

**Platform:** OverTheWire  
**Wargame:** Bandit  
**Date Completed:** 2026-05-19  
**Tags:** `restricted-shell` `shell-escape` `uppercase-shell` `shell-variables` `sh`

---

## Objective

Upon logging in as bandit32 you are dropped into an "uppercase shell" - a custom restricted shell that converts every command you type to uppercase before executing it, causing all standard commands to fail. Escape it and retrieve the password for Level 33.

---

## Concepts Learned

- **Custom restricted shells:** As seen in Level 25 with `showtext`, the login shell for a user does not have to be a standard shell. Here bandit32 runs a custom binary that intercepts input, converts it to uppercase, and passes it to the system - making every lowercase command name unreachable by typing it directly.
- **`$0` - the shell's own name:** In shell scripting, `$0` is a special variable that holds the name (or path) of the currently running script or shell. When typed interactively inside a shell process, `$0` causes the shell to re-invoke itself. Crucially, `$0` is not a command name - it is a variable reference, and variable references are not subject to the uppercase conversion. This makes it the escape route.
- **`sh` invoked via `$0`:** When the uppercase shell processes `$0`, it expands the variable to the path of the shell binary currently running (in this case `/bin/sh`) and executes it - spawning a new standard shell. Variable expansions happen before the uppercase transformation is applied, so `$0` reaches the interpreter intact.
- **Shell special variables:** `$0`, `$1`, `$?`, `$$`, and others are built-in shell variables with specific meanings. Understanding them is essential for shell scripting, debugging, and - as demonstrated here - escaping restricted environments.
- **Persistence through enumeration:** When every command fails, the instinct should be to think about what is _not_ a command - shell built-ins, special variables, and syntax constructs that the restriction layer may not intercept.

---

## Approach

Logging in as bandit32 presented an unfamiliar prompt with `>>` instead of the standard `$`. Every command I tried - `ls`, `cat`, `clear` - was immediately rejected with errors showing the command converted to uppercase: `LS: not found`, `CAT: not found`. The pattern was clear: the shell was uppercasing all input before execution.

Standard command names were entirely unreachable. The key insight was to think beyond command names and consider what the shell processes _before_ applying any transformation. Shell special variables like `$0` are expanded by the interpreter at a lower level than command name resolution - before the uppercase conversion could intercept them.

Typing `$0` caused the shell to expand it to the path of the running shell binary and invoke it, dropping me into a standard `/bin/sh` session. From there all commands worked normally. I read the bandit32 password with `cat /etc/bandit_pass/bandit32` out of curiosity, then read bandit33's password from the same directory to complete the level.

---

## Commands Used

```bash
# --- Inside the uppercase shell ---

# These all fail - commands are uppercased before execution
>> ls        # sh: 1: LS: not found
>> cat       # sh: 1: CAT: not found
>> clear     # sh: 1: CLEAR: not found

# Escape using the $0 special variable - not subject to uppercase conversion
>> $0

# --- Now in a standard /bin/sh session ---

# Read the password for the next level
cat /etc/bandit_pass/bandit33
```

**Why `$0` works:**

```
Input: $0
Shell expansion: /bin/sh   ← variable is expanded before uppercase filter runs
Result: a new standard shell is spawned
```

---

## Reflection

This level is a fitting conclusion to the Bandit wargame. It ties together two major themes that appear throughout - restricted shells and creative escapes - and resolves them with something deceptively simple: a single shell variable. The lesson is that restricted shells are only as strong as the assumptions their authors make about what input is "safe." `$0` works here because the restriction was applied to command names, not to variable expansions - a gap the author of the uppercase shell did not close.

Throughout Bandit, the escape routes have consistently come from understanding the _mechanics_ of the tools involved rather than memorising commands. That principle - understand the system, find the gap - is the one that carries forward into real security work.

---

## Key Takeaway

Restricted shells are frequently bypassable because they operate at the wrong layer of abstraction. Filtering command names while leaving shell variable expansion untouched is a common oversight. In penetration testing and CTF environments, when a shell blocks commands, the first questions to ask are: what does it _not_ filter, and what can reach the interpreter without being treated as a command name? `$0`, shell built-ins, and heredocs are all worth trying when direct commands fail.

---

## References

- [OverTheWire Bandit](https://overthewire.org/wargames/bandit/)
- [Bash manual - Special Parameters](https://www.gnu.org/software/bash/manual/bash.html#Special-Parameters)
- [GTFOBins - Shell escapes](https://gtfobins.github.io/)
- [Linux man page - `sh`](https://linux.die.net/man/1/sh)
