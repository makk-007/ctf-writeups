# CTF Write-ups

Personal collection of Capture The Flag (CTF) solutions and wargame write-ups from OverTheWire, PicoCTF, TryHackMe, and HackTheBox.

## About

I'm a CCNA-certified Cybersecurity trainee with a First Class degree in Computer Engineering from Academic City University, Ghana. These write-ups document my hands-on approach to learning offensive and defensive security techniques, covering Linux fundamentals, networking, forensics, and more.

## Platforms

| Platform    | Progress                                                                     | Focus Areas                         |
| ----------- | ---------------------------------------------------------------------------- | ----------------------------------- |
| OverTheWire | ![OverTheWire](https://img.shields.io/badge/OverTheWire-In_Progress-blue)    | Linux, SSH, File Systems, Scripting |
| PicoCTF     | ![PicoCTF](https://img.shields.io/badge/PicoCTF-In_Progress-green)           | General Skills, Forensics, Web      |
| TryHackMe   | ![TryHackMe](https://img.shields.io/badge/TryHackMe-In_Progress-red)         | SOC, Networking, Blue Team          |
| HackTheBox  | ![HackTheBox](https://img.shields.io/badge/HackTheBox-In_Progress-darkgreen) | Penetration Testing                 |

## Skills Demonstrated

- Linux command line and system administration
- Network traffic analysis
- Web application security
- Incident detection and response
- OSINT techniques
- Cryptography and encoding fundamentals

## Write-up Index

### OverTheWire - Bandit

| Level         | Tags                                                                               | Difficulty | Write-up                                 |
| ------------- | ---------------------------------------------------------------------------------- | ---------- | ---------------------------------------- |
| Level 0 → 1   | `ssh` `remote-access` `linux-basics` `port-specification`                          | Easy       | [Link](./overthewire/bandit/level-00.md) |
| Level 1 → 2   | `special-characters` `file-naming` `linux-basics` `cat`                            | Easy       | [Link](./overthewire/bandit/level-01.md) |
| Level 2 → 3   | `spaces-in-filenames` `quoting` `escaping` `linux-basics`                          | Medium     | [Link](./overthewire/bandit/level-02.md) |
| Level 3 → 4   | `hidden-files` `find` `linux-basics` `directory-navigation`                        | Easy       | [Link](./overthewire/bandit/level-03.md) |
| Level 4 → 5   | `file-types` `human-readable` `find` `scripting-efficiency`                        | Medium     | [Link](./overthewire/bandit/level-04.md) |
| Level 5 → 6   | `find` `file-attributes` `size-filtering` `permissions` `pipeline`                 | Medium     | [Link](./overthewire/bandit/level-05.md) |
| Level 6 → 7   | `find` `file-ownership` `stderr-redirection` `linux-permissions`                   | Medium     | [Link](./overthewire/bandit/level-06.md) |
| Level 7 → 8   | `grep` `text-search` `large-files` `wc` `pipeline`                                 | Easy       | [Link](./overthewire/bandit/level-07.md) |
| Level 8 → 9   | `sort` `uniq` `text-processing` `pipelines` `deduplication`                        | Medium     | [Link](./overthewire/bandit/level-08.md) |
| Level 9 → 10  | `strings` `grep` `binary-files` `text-extraction` `pipeline`                       | Medium     | [Link](./overthewire/bandit/level-09.md) |
| Level 10 → 11 | `base64` `encoding` `decoding` `linux-basics`                                      | Easy       | [Link](./overthewire/bandit/level-10.md) |
| Level 11 → 12 | `rot13` `caesar-cipher` `tr` `text-substitution` `classical-cryptography`          | Medium     | [Link](./overthewire/bandit/level-11.md) |
| Level 12 → 13 | `hexdump` `compression` `gzip` `bzip2` `tar` `file-forensics` `xxd`                | Hard       | [Link](./overthewire/bandit/level-12.md) |
| Level 13 → 14 | `ssh` `ssh-keys` `scp` `file-permissions` `authentication`                         | Medium     | [Link](./overthewire/bandit/level-13.md) |
| Level 14 → 15 | `netcat` `tcp` `networking` `ports` `raw-connections`                              | Easy       | [Link](./overthewire/bandit/level-14.md) |
| Level 15 → 16 | `openssl` `tls` `ssl` `encrypted-connections` `s_client`                           | Medium     | [Link](./overthewire/bandit/level-15.md) |
| Level 16 → 17 | `nmap` `port-scanning` `ssl` `openssl` `service-enumeration` `ssh-keys`            | Hard       | [Link](./overthewire/bandit/level-16.md) |
| Level 17 → 18 | `diff` `file-comparison` `text-processing` `unified-diff`                          | Easy       | [Link](./overthewire/bandit/level-17.md) |
| Level 18 → 19 | `ssh` `bashrc` `command-execution` `shell-bypass` `persistence`                    | Medium     | [Link](./overthewire/bandit/level-18.md) |
| Level 19 → 20 | `setuid` `linux-permissions` `privilege-escalation` `binary-exploitation`          | Medium     | [Link](./overthewire/bandit/level-19.md) |
| Level 20 → 21 | `netcat` `tcp` `background-jobs` `tmux` `networking` `concurrency`                 | Medium     | [Link](./overthewire/bandit/level-20.md) |
| Level 21 → 22 | `cron` `crontab` `scheduled-tasks` `bash-scripting` `linux-internals`              | Medium     | [Link](./overthewire/bandit/level-21.md) |
| Level 22 → 23 | `cron` `bash-scripting` `md5sum` `hashing` `script-analysis`                       | Medium     | [Link](./overthewire/bandit/level-22.md) |
| Level 23 → 24 | `cron` `bash-scripting` `script-writing` `file-permissions` `privilege-escalation` | Hard       | [Link](./overthewire/bandit/level-23.md) |
| Level 24 → 25 | `brute-force` `bash-scripting` `netcat` `loops` `wordlist-generation`              | Hard       | [Link](./overthewire/bandit/level-24.md) |
| Level 25 → 26 | `ssh` `restricted-shell` `more` `pager` `passwd` `shell-escape`                    | Hard       | [Link](./overthewire/bandit/level-25.md) |
| Level 26 → 27 | `vim` `shell-escape` `restricted-shell` `privilege-escalation` `setuid`            | Hard       | [Link](./overthewire/bandit/level-26.md) |
| Level 27 → 28 | `git` `git-clone` `version-control` `ssh-protocol` `linux-basics`                  | Easy       | [Link](./overthewire/bandit/level-27.md) |
| Level 28 → 29 | `git` `git-log` `git-show` `commit-history` `sensitive-data-exposure`              | Medium     | [Link](./overthewire/bandit/level-28.md) |
| Level 29 → 30 | `git` `git-branch` `git-checkout` `version-control` `reconnaissance`               | Medium     | [Link](./overthewire/bandit/level-29.md) |
| Level 30 → 31 | `git` `git-tag` `git-show` `version-control` `reconnaissance`                      | Medium     | [Link](./overthewire/bandit/level-30.md) |

### PicoCTF

| Challenge       | Category | Difficulty | Write-up |
| --------------- | -------- | ---------- | -------- |
| _(coming soon)_ | -        | -          | -        |

### TryHackMe

| Room            | Category | Difficulty | Write-up |
| --------------- | -------- | ---------- | -------- |
| _(coming soon)_ | -        | -          | -        |

### HackTheBox

| Box             | OS  | Difficulty | Write-up |
| --------------- | --- | ---------- | -------- |
| _(coming soon)_ | -   | -          | -        |

## Connect

- LinkedIn: [Kelvin Kwofie Mawusenam Ackuaku](https://www.linkedin.com/in/mawusenam-ackuaku)
- TryHackMe: [makk007.gg](https://tryhackme.com/p/makk007.gg)
