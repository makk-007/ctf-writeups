# Bandit Level 31 → Level 32

**Platform:** OverTheWire  
**Wargame:** Bandit  
**Date Completed:** 2026-05-19  
**Tags:** `git` `git-push` `gitignore` `git-add` `version-control`

---

## Objective

A Git repository is hosted on the game server. Clone it, follow the instructions in the README to create and push a specific file to the remote repository, and receive the password for Level 32 in return.

---

## Concepts Learned

- **`git push`:** Uploads local commits to a remote repository. `git push origin master` pushes the current local commits to the `master` branch of the `origin` remote. This is the counterpart to `git clone` and `git pull` - it is how changes flow back upstream.
- **`.gitignore`:** A configuration file that tells Git which files and patterns to exclude from tracking. Files matching the patterns in `.gitignore` are not staged by `git add` under normal circumstances. This is widely used to prevent build artefacts, environment files, and secrets from being committed - though in this level it was used to block the file you needed to add.
- **`git add -f` (force add):** Overrides `.gitignore` rules and stages a file that would otherwise be excluded. The `-f` flag explicitly tells Git to track the file regardless of any ignore rules. This is useful in legitimate scenarios but also illustrates why `.gitignore` alone is not a security control - it can always be bypassed with `-f`.
- **`git config`:** Sets Git configuration values. Before committing, Git requires a user name and email to associate with the commit. `git config --global` sets these values for all repositories on the current system.
- **`git status`:** Shows the current state of the working directory and staging area - which files are modified, which are staged, and which are untracked. Running it before committing is good practice to confirm the expected files are staged.
- **The full Git workflow:** This level is the first to exercise the complete Git contribution cycle: create a file → stage it → commit it → push it. Understanding this end-to-end workflow is foundational to any developer or security role that involves working with code repositories.

---

## Approach

After cloning and reading the README, the instructions were clear: create a file with specific content and push it to the remote repository. I created the file with `echo`, then attempted to stage it with `git add`. Nothing happened - the file was not being tracked.

I checked `.gitignore` and found that all `.txt` files were excluded by default. The standard `git add` was silently ignoring my file because of this rule. The fix was to force Git to track it with `git add -f file.txt`.

I ran `git status` to confirm the file was now staged. Before committing, Git required identity configuration - I set a name and email with `git config --global`. I then committed the change with a message and pushed it to the remote with `git push origin master`. The remote server processed the push and responded with the password for the next level.

---

## Commands Used

```bash
# Clone the repository
git clone ssh://bandit31-git@bandit.labs.overthewire.org:2220/home/bandit31-git/repo

# Navigate into the repository
cd repo

# Read the instructions
cat README.md

# Create the required file with the specified content
echo "May I come in?" > file.txt

# Check what .gitignore is excluding
cat .gitignore

# Force-add the file, overriding the .gitignore rule
git add -f file.txt

# Confirm the file is staged
git status

# Configure Git identity (required before committing)
git config --global user.email "bandit@overthewire.org"
git config --global user.name "Bandit"

# Commit the staged file
git commit -m "add file.txt"

# Push to the remote repository
git push origin master
```

---

## Key Takeaway

`.gitignore` is a convenience tool, not a security boundary. Any file can be force-added to a repository regardless of ignore rules using `git add -f`. This is an important distinction: developers sometimes assume that adding a secrets file to `.gitignore` prevents it from ever being committed, but a single `git add -f` undoes that protection entirely. Secrets should be kept out of repositories by process and tooling - not by relying on `.gitignore` alone.

---

## References

- [OverTheWire Bandit](https://overthewire.org/wargames/bandit/)
- [Git documentation - `git push`](https://git-scm.com/docs/git-push)
- [Git documentation - `.gitignore`](https://git-scm.com/docs/gitignore)
- [Git documentation - `git add`](https://git-scm.com/docs/git-add)
- [Git documentation - `git config`](https://git-scm.com/docs/git-config)
