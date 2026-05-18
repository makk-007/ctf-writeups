# Bandit Level 29 → Level 30

**Platform:** OverTheWire  
**Wargame:** Bandit  
**Date Completed:** 2026-05-18  
**Tags:** `git` `git-branch` `git-checkout` `version-control` `reconnaissance`

---

## Objective

A Git repository is hosted on the game server. Clone it and find the password for Level 30 - this time it is not in the commit history of the default branch but is hidden in a separate branch.

---

## Concepts Learned

- **Git branches:** A branch is an independent line of development within a repository. The default branch (often `main` or `master`) is just one of potentially many. Developers use branches for features, experiments, and environments (e.g. `dev`, `staging`, `production`). Each branch can contain entirely different file contents and commit histories.
- **`git branch`:** Lists all local branches. By default it only shows branches that have been checked out or explicitly fetched to the local machine.
- **`git branch -a`:** Lists all branches - both local and remote. The `-a` flag is essential for discovering branches that exist on the remote server but have not been checked out locally. Remote branches appear prefixed with `remotes/origin/`.
- **`git checkout <branch>`:** Switches the working directory to the specified branch, updating all tracked files to match that branch's state. This is how you access content that exists on a different branch.
- **Branch-based information hiding:** Sensitive data stored on a non-default branch is not visible to anyone who only looks at the main branch. However, anyone with read access to the repository can enumerate and check out all branches - making this a false sense of security rather than a genuine access control.

---

## Approach

After cloning and navigating into the repository, I read `README.md` as a starting point. It contained a note stating the password was not stored in production - a strong hint that other branches or environments existed.

I ran `git log` to check the commit history on the current branch - nothing useful there. I then ran `git branch` to see local branches, which only showed the default branch I was already on.

The key step was `git branch -a`, which revealed all remote branches. Among them was one with a name suggesting a development or non-production environment. I switched to it with `git checkout <branch>` and read the password file in that branch's working directory.

---

## Commands Used

```bash
# Clone the repository
git clone ssh://bandit29-git@bandit.labs.overthewire.org:2220/home/bandit29-git/repo

# Navigate into the repository
cd repo

# Read the current README for clues
cat README.md

# Check commit history on the current branch
git log

# List local branches only
git branch

# List all branches including remote
git branch -a

# Switch to the branch that looks promising
git checkout <branch_name>

# Read the password file on the new branch
cat README.md
```

---

## Key Takeaway

When investigating a Git repository, always enumerate all branches with `git branch -a` - the default branch is rarely the whole picture. In real-world security assessments, development and staging branches regularly contain hardcoded credentials, debug endpoints, and infrastructure details that were never intended for production but are fully accessible to anyone with repository read access.

---

## References

- [OverTheWire Bandit](https://overthewire.org/wargames/bandit/)
- [Git documentation - `git branch`](https://git-scm.com/docs/git-branch)
- [Git documentation - `git checkout`](https://git-scm.com/docs/git-checkout)
- [Git documentation - Branches in a Nutshell](https://git-scm.com/book/en/v2/Git-Branching-Branches-in-a-Nutshell)
