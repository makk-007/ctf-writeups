# Bandit Level 30 → Level 31

**Platform:** OverTheWire  
**Wargame:** Bandit  
**Date Completed:** 2026-05-18  
**Tags:** `git` `git-tag` `git-show` `version-control` `reconnaissance`

---

## Objective

A Git repository is hosted on the game server. Clone it and find the password for Level 31 - this time it is stored in a Git tag.

---

## Concepts Learned

- **Git tags:** A tag is a named reference to a specific point in a repository's history - most commonly used to mark release versions (e.g. `v1.0.0`, `v2.3.1`). Unlike branches, tags do not move as new commits are added; they are permanent markers. Tags can also carry attached data, making them a place where arbitrary content - including sensitive data - can be stored.
- **Annotated vs lightweight tags:** A lightweight tag is simply a pointer to a commit. An annotated tag is a full Git object with its own message, author, and date - and can carry additional data. `git show <tag>` reveals the full contents of an annotated tag, including any attached message or data.
- **`git tag`:** Lists all tags in the repository. This is a quick enumeration step that is easily overlooked when someone focuses only on commits and branches.
- **`git show <tag>`:** Displays the contents of a tag - its message, the commit it points to, and any attached data. This is where the password was stored in this level.
- **Exhaustive Git enumeration:** Levels 27 through 30 progressively introduce all the major places data can hide in a Git repository: current file contents, commit history, branches, and tags. Together they form a complete Git reconnaissance checklist.

---

## Approach

Following the same opening steps as previous levels, I cloned the repository, navigated into it, and read `README.md`. The file contained a note that was deliberately unhelpful - confirming that the information was not where it had been in previous levels.

I ran `git log` to check commit history - only one commit, nothing interesting. I ran `git branch -a` to check for other branches - again nothing useful. Having exhausted the approaches that worked in the previous levels, I reached for `git tag` to enumerate tags. A tag with a suspicious name appeared immediately.

Running `git show <tag_name>` displayed its contents - the password was stored directly in the tag's data.

---

## Commands Used

```bash
# Clone the repository
git clone ssh://bandit30-git@bandit.labs.overthewire.org:2220/home/bandit30-git/repo

# Navigate into the repository
cd repo

# Read the current README - not helpful here
cat README.md

# Check commit history - nothing interesting
git log

# Check all branches - nothing interesting
git branch -a

# Enumerate tags
git tag

# Reveal the contents of the suspicious tag
git show <tag_name>
```

---

## Key Takeaway

A thorough Git repository investigation requires checking all four hiding places in sequence: current file contents → commit history → branches → tags. Stopping after any one of these steps and concluding there is nothing to find is premature. In real-world security assessments and bug bounties, Git tags are a frequently overlooked source of leaked data precisely because most people check commits and branches and stop there.

---

## References

- [OverTheWire Bandit](https://overthewire.org/wargames/bandit/)
- [Git documentation - `git tag`](https://git-scm.com/docs/git-tag)
- [Git documentation - `git show`](https://git-scm.com/docs/git-show)
- [Git documentation - Tagging](https://git-scm.com/book/en/v2/Git-Basics-Tagging)
