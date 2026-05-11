# Bandit Level 21 → Level 22

**Platform:** OverTheWire  
**Wargame:** Bandit  
**Date Completed:** 2026-05-11  
**Tags:** `cron` `crontab` `scheduled-tasks` `bash-scripting` `linux-internals`

---

## Objective

A cron job is running automatically as bandit22. Investigate it, understand what it does, and use that knowledge to retrieve the password for Level 22.

---

## Concepts Learned

- **Cron:** A time-based job scheduler built into Linux. It runs commands or scripts automatically at specified intervals. System-wide cron jobs are defined in files under `/etc/cron.d/`. User-level jobs are managed with `crontab -e`.
- **Crontab syntax:** Each line in a crontab follows the format `minute hour day month weekday user command`. The `* * * * *` pattern means "every minute, every hour, every day, every month, every day of the week." The `@reboot` directive runs a job once at system startup.
- **`&> /dev/null`:** Redirects both stdout (file descriptor 1) and stderr (file descriptor 2) to `/dev/null`, silencing all output from the scheduled script. This is commonly seen in cron jobs where output is not needed and would otherwise generate email notifications.
- **World-readable files in `/tmp`:** When a privileged process writes data to a world-readable file in `/tmp`, any user on the system can read it. This is exactly the vulnerability class this level demonstrates - a scheduled job running as a privileged user leaving readable artefacts accessible to everyone.
- **Cron as a persistence mechanism:** In real-world offensive security, cron jobs are a common persistence technique. An attacker who gains write access to a crontab or to a script referenced by cron can execute code repeatedly under that user's privileges.

---

## Approach

The level hinted that a cron job was involved. My first step was to check `/etc/cron.d/` for any jobs related to bandit22.

```
cat /etc/cron.d/cronjob_bandit22
```

The output showed:

```
@reboot bandit22 /usr/bin/cronjob_bandit22.sh &> /dev/null
* * * * * bandit22 /usr/bin/cronjob_bandit22.sh &> /dev/null
```

This means the script runs as bandit22 once at boot and then every minute continuously. The `&> /dev/null` suppresses all terminal output so nothing is visible from the outside.

I then read the script itself:

```bash
#!/bin/bash
chmod 644 /tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv
cat /etc/bandit_pass/bandit22 > /tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv
```

Breaking this down line by line:

- `chmod 644 /tmp/<filename>` - sets the file permissions to `rw-r--r--`, making it readable by all users on the system.
- `cat /etc/bandit_pass/bandit22 > /tmp/<filename>` - reads bandit22's password (which only bandit22 can access directly) and writes it into the world-readable temp file.

Because the script runs every minute as bandit22, the file is always fresh and always readable. I simply used `cat` on the temp file path to retrieve the password.

---

## Commands Used

```bash
# Check system cron jobs
ls /etc/cron.d/

# Read the cron job definition for bandit22
cat /etc/cron.d/cronjob_bandit22

# Read the script being executed by the cron job
cat /usr/bin/cronjob_bandit22.sh

# Read the world-readable temp file the script creates
cat /tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv
```

---

## Key Takeaway

Cron jobs running as privileged users that write sensitive data to world-readable locations are a real and common misconfiguration. During a Linux privilege escalation assessment, enumerating `/etc/cron.d/`, `/etc/crontab`, and individual user crontabs is a standard step - you are looking for exactly this: a scheduled process that leaks data or executes a script you can influence.

---

## References

- [OverTheWire Bandit](https://overthewire.org/wargames/bandit/)
- [Linux man page - `cron`](https://linux.die.net/man/8/cron)
- [Linux man page - `crontab`](https://linux.die.net/man/5/crontab)
- [Cron HowTo - Ubuntu](https://help.ubuntu.com/community/CronHowto)
