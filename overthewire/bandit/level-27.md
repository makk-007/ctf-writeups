# Bandit Level 27 → Level 28

**Platform:** OverTheWire  
**Wargame:** Bandit  
**Date Completed:** 2026-05-18  
**Tags:** `git` `git-clone` `version-control` `ssh-protocol` `linux-basics`

---

## Objective

A Git repository is hosted on the game server. Clone it and find the password for Level 28 inside.

---

## Concepts Learned

- **`git clone`:** Downloads a complete copy of a remote repository - including all files, commit history, and branches - to your local machine (or in this case, your working directory on the game server). It is the standard starting point for interacting with any remote Git repository.
- **Git over SSH:** Git supports multiple transport protocols. The `ssh://` scheme uses SSH for authentication and transport, meaning you can authenticate using existing SSH credentials. The URL format `ssh://user@host:port/path/to/repo` is the full form used when the server runs on a non-standard port.
- **Git as an information store:** This level introduces a recurring theme across the next several levels - Git repositories frequently contain far more information than just the current file contents. Commit history, branches, tags, and stashed changes are all places where sensitive data can be inadvertently stored or deliberately hidden.
- **Basic repository navigation:** After cloning, the workflow is familiar Linux navigation - `cd` into the repo, `ls -la` to inspect contents, and `cat` to read files. The difference is that the directory is now a Git repository with a hidden `.git` folder containing the full version history.

---

## Approach

The level description pointed to a Git repository on the game server accessible via SSH. I constructed the clone URL using the `ssh://` scheme, specifying the bandit27-git user, the game server hostname, the non-standard port 2220, and the repository path.

After cloning I navigated into the repository directory, checked its contents with `ls -la`, and found a `README` file. Reading it with `cat` revealed the password directly - no history exploration or branch switching required at this stage. This level is the entry point into a series of Git-focused challenges, each of which adds a layer of complexity on top of the basics established here.

---

## Commands Used

```bash
# Clone the remote repository over SSH on the non-standard port
git clone ssh://bandit27-git@bandit.labs.overthewire.org:2220/home/bandit27-git/repo

# Navigate into the cloned repository
cd repo

# Inspect the contents
ls -la

# Read the password file
cat README
```

---

## Key Takeaway

Git repositories are treasure troves of information - and this level is just the beginning of exploring them. In real-world security assessments, public or misconfigured Git repositories are a common source of leaked credentials, API keys, and internal infrastructure details. Knowing how to clone and navigate a repository is the foundation for all the Git-focused investigation that follows in the next levels.

---

## References

- [OverTheWire Bandit](https://overthewire.org/wargames/bandit/)
- [Git documentation - `git clone`](https://git-scm.com/docs/git-clone)
- [Git documentation - Git Protocols](https://git-scm.com/book/en/v2/Git-on-the-Server-The-Protocols)
