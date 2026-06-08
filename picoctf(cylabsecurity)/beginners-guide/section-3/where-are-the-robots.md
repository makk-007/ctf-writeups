# Where Are the Robots

**Platform:** CyLab Security Academy (formerly PicoCTF)  
**Category:** Web Exploitation  
**Difficulty:** Easy  
**Learning Path:** Beginner's Guide to the Challenge Library (Section 3)

---

## Objective

Locate a hidden page on a web application by understanding how web crawlers and site administrators communicate about restricted content.

---

## Concepts Learned

| Concept | Description |
|---|---|
| robots.txt | A standard text file placed at the root of a web server that instructs web crawlers which pages to index or avoid |
| Web Crawlers / Bots | Automated programs that browse the web to index content for search engines |
| Disallow Directive | A robots.txt instruction telling crawlers not to index a specific path, which unintentionally documents sensitive or hidden URLs |
| Security Through Obscurity (and its limits) | The flawed assumption that hiding a page from search engines makes it inaccessible to a determined user |

---

## Approach

The challenge title, *Where Are the Robots*, was an unmistakable hint pointing toward the `robots.txt` file, a standard component of web server configuration.

**Understanding robots.txt:**  
The `robots.txt` file lives at the root of a web server (e.g., `http://example.com/robots.txt`) and is part of the Robots Exclusion Protocol. Its original purpose is to guide search engine crawlers, telling them which pages to index and which to skip. A `Disallow` entry signals that a specific path should not be crawled.

The critical security implication: `robots.txt` is a publicly readable file. Any path listed under `Disallow` is not hidden from human visitors. It is, in fact, a documented map of paths the site owner considers sensitive or non-public. This makes it one of the first files a security professional checks during web application reconnaissance.

**Investigation steps:**

1. Navigated to the `robots.txt` file at the root of the provided URL. The file contained a `Disallow` entry listing a specific HTML page path.

2. Recognized that the `Disallow` directive does not restrict access; it only advises crawlers. Any user can navigate directly to a disallowed path in their browser.

3. Navigated directly to the disallowed path, which loaded a page containing the flag.

---

## Key Takeaway

`robots.txt` is a reconnaissance goldmine. Security professionals routinely check it during the information-gathering phase of a web assessment because site administrators often list sensitive directories, admin panels, or staging pages under `Disallow` directives, believing this keeps them hidden. It does not. Listing a path in `robots.txt` with the intent to hide it is a textbook example of security through obscurity, which is not a reliable security control.

---

## Reflection

This challenge reinforced an important principle: information meant to control automated systems is often visible to, and exploitable by, human investigators. In the same way that a network administrator's ACL comments or SNMP community strings can leak topology details, `robots.txt` leaks the internal structure of a web application. Reconnaissance is always about reading what the system was designed to say to someone else.

---

## References

- [Google Search Central: robots.txt Introduction](https://developers.google.com/search/docs/crawling-indexing/robots/intro)
- [OWASP: Testing for Sensitive Information in robots.txt](https://owasp.org/www-project-web-security-testing-guide/)