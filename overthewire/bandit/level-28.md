# Bandit Level 28 → Level 29

**Platform:** OverTheWire  
**Wargame:** Bandit  
**Date Completed:** 2026-05-18  
**Tags:** `git` `git-log` `git-show` `commit-history` `sensitive-data-exposure`

---

## Objective

A Git repository is hosted on the game server. Clone it and find the password for Level 29 - this time it is not sitting in the current file contents but has been removed from a previous commit.

---

## Concepts Learned

- **`git log`:** Displays the commit history of a repository in reverse chronological order. Each entry shows the commit hash, author, date, and commit message. This is the starting point for investigating what has changed over time in a repository.
- **`git show <commit>`:** Displays the full details of a specific commit - the metadata, the diff of what changed, lines added (prefixed with `+`), and lines removed (prefixed with `-`). A line removed in a commit is still permanently visible in the repository history unless the history itself has been rewritten.
- **Commit history as a forensic record:** Git is designed to never lose data by default. Deleting a line from a file and committing that change does not erase the original content - it is preserved in the commit that introduced the deletion. This makes Git history a reliable forensic record of everything that was ever committed, regardless of whether it was later "removed."
- **Sensitive data committed to Git:** One of the most common real-world security issues is credentials, API keys, or passwords being committed to a Git repository - even briefly. Even if the developer removes the secret in a follow-up commit, the secret remains accessible in the history to anyone with read access to the repository. This is why secrets must be rotated immediately when this happens, not just removed from the current files.

---

## Approach

After cloning the repository and navigating into it, I ran `cat README.md` as I had in the previous level. This time the file contained a placeholder where the password had been - a clear hint that it had existed at some point and been removed.

I ran `git log` to view the commit history. The log showed multiple commits, and the commit messages were informative - one described adding credentials while another described removing or fixing them. The suspicious commit was the one that removed data.

I ran `git show <commit hash>` on that commit. The diff output showed lines prefixed with `-` (removed) and `+` (added). The removed line contained the password. Because Git preserves history, the deletion was fully visible and the password was recoverable.

---

## Commands Used

```bash
# Clone the repository
git clone ssh://bandit28-git@bandit.labs.overthewire.org:2220/home/bandit28-git/repo

# Navigate into the repository
cd repo

# Check current file contents - reveals a placeholder, not the password
cat README.md

# Review the full commit history
git log

# Inspect the commit where the password was removed
git show <commit_hash>
```

**Reading `git show` output:**

```
-password: <the actual password>    ← removed in this commit (still visible in history)
+password: xxxxxxxxxx               ← replaced with a placeholder
```

---

## Key Takeaway

Deleting sensitive data from a Git repository does not remove it from history. This is one of the most exploited misconfigurations in real-world security assessments - attackers routinely run `git log` and `git show` on exposed repositories to recover credentials that developers believed were safely deleted. The correct response to an accidentally committed secret is to rotate it immediately and, if necessary, rewrite the Git history using tools like `git filter-branch` or `git filter-repo`.

---

## References

- [OverTheWire Bandit](https://overthewire.org/wargames/bandit/)
- [Git documentation - `git log`](https://git-scm.com/docs/git-log)
- [Git documentation - `git show`](https://git-scm.com/docs/git-show)
- [GitHub - Removing sensitive data from a repository](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/removing-sensitive-data-from-a-repository)
