# Bandit Level 8 → Level 9

**Platform:** OverTheWire  
**Wargame:** Bandit  
**Date Completed:** 2026-05-06  
**Tags:** `sort` `uniq` `text-processing` `pipelines` `deduplication`

---

## Objective

The password for Level 9 is the only line that appears exactly once in `data.txt`. All other lines are duplicated. Find and read it.

---

## Concepts Learned

- **`uniq`:** Filters out adjacent duplicate lines from its input. The critical constraint is the word _adjacent_ - `uniq` only compares consecutive lines, so duplicates scattered throughout a file will not be caught unless the file is sorted first.
- **`sort`:** Sorts lines alphabetically (or numerically with `-n`). When piped into `uniq`, it guarantees that all identical lines are grouped together, making `uniq` effective.
- **`uniq -c`:** Prefixes each line with a count of how many times it appeared. Useful for frequency analysis - a technique that appears in log review, traffic analysis, and forensics.
- **`uniq -u`:** Prints only lines that appear exactly once after sorting. This is the clean, direct solution once you understand what the tool is doing.
- **`sort -n`:** Sorts numerically. Combining `sort data.txt | uniq -c | sort -n` produces a frequency-sorted list with the least common lines at the top - a powerful pattern for anomaly detection.
- **Reading man pages:** Discovering the `-u` flag through `man uniq` rather than guessing is the right habit. Man pages are authoritative and always available offline.

---

## Approach

I started by checking the file with `wc -l` to understand its size, then attempted `cat` to get a feel for the content. The file contained many duplicate lines with one unique entry mixed in - not something you could spot visually.

My first attempt was to run `uniq -c` directly on the file, hoping to count occurrences and identify the line with a count of 1. This returned mostly unsorted output and did not reliably group duplicates because `uniq` only collapses _adjacent_ identical lines - a fact I confirmed when the result looked incorrect.

The fix was to pipe through `sort` first. `sort data.txt | uniq -c | sort -n` gave me a frequency-ordered list where the unique line appeared at the top with a count of 1. This confirmed my answer.

I then refined the approach by consulting the `uniq` man page, which revealed the `-u` flag - it prints only lines that appear exactly once, making the pipeline cleaner: `sort data.txt | uniq -u`. One line. Done.

---

## Commands Used

```bash
# Check line count
wc -l data.txt

# Exploratory: count occurrences of each line, sorted by frequency
sort data.txt | uniq -c | sort -n

# Clean solution: print only the line that appears exactly once
sort data.txt | uniq -u
```

---

## Key Takeaway

`sort | uniq` is a classic Unix pipeline pattern that appears constantly in real work - from deduplicating IP address lists in network logs to identifying anomalous entries in authentication records. Always sort before piping to `uniq`, and reach for the man page when you know a tool does something but are not sure of the exact flag.

---

## References

- [OverTheWire Bandit](https://overthewire.org/wargames/bandit/)
- [Linux man page - `uniq`](https://linux.die.net/man/1/uniq)
- [Linux man page - `sort`](https://linux.die.net/man/1/sort)
