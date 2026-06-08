# Insp3ct0r

**Platform:** CyLab Security Academy (formerly PicoCTF)  
**Category:** Web Exploitation  
**Difficulty:** Easy  
**Learning Path:** Beginner's Guide to the Challenge Library (Section 3)

---

## Objective

Investigate a provided web application to uncover a hidden flag by examining its underlying source components.

---

## Concepts Learned

| Concept | Description |
|---|---|
| Browser Developer Tools | Built-in browser utilities for inspecting the structure and assets of a web page |
| HTML Source Inspection | Viewing the raw markup of a webpage to find embedded content not visible on the rendered page |
| CSS Source Inspection | Examining stylesheet files that control visual presentation, which can contain embedded comments or data |
| JavaScript Source Inspection | Reading client-side scripts for logic, comments, or hidden values |
| Client-Side Resource Enumeration | The practice of systematically reviewing all assets a browser loads to fully understand a web application |

---

## Approach

The challenge name itself was the first clue: *Insp3ct0r* is a deliberate leet-speak reference to browser inspection tools. This immediately pointed toward the browser's built-in Developer Tools as the primary attack surface.

When assessing any web application, the rendered page is only one layer. A browser actually receives and processes multiple resource types to display a page: the HTML document, CSS stylesheets, and JavaScript files. Each of these is readable by anyone with access to the page, making them common hiding spots in web exploitation challenges.

My enumeration followed a logical order, working through each resource type systematically:

1. **HTML source** - The starting point for any web inspection. The raw HTML markup reveals the document structure, comments, and any content not rendered visually on screen.

2. **CSS source** - Stylesheets are often overlooked because they are associated with visual design rather than functionality. However, they can contain developer comments or embedded data just as HTML can.

3. **JavaScript source** - Client-side scripts are fully readable in the browser and frequently contain logic, comments, or values that developers may not intend to expose.

Working through each of these in order revealed the three parts of the flag, which when combined formed the complete answer.

> **Note:** I also checked the network response headers early in my investigation, as HTTP response headers can sometimes reveal server-side information. While this did not yield a flag fragment in this case, it is a sound habit when approaching web exploitation challenges.

---

## Key Takeaway

Web applications expose far more than what is visible on the rendered page. HTML, CSS, and JavaScript source files are all client-side resources, meaning they are fully accessible to anyone who knows where to look. A methodical review of every resource a browser loads is a fundamental skill in web application security assessment, and the browser's Developer Tools are the first instrument a security professional reaches for.

---

## Reflection

Coming from a networking background, I think of this as analogous to packet inspection: the rendered web page is like the application-layer payload a user sees, but the actual traffic contains headers, metadata, and supporting files that tell a much fuller story. Developer Tools let you see all of it.

---

## References

- [MDN Web Docs: View page source](https://developer.mozilla.org/en-US/docs/Tools/View_source)
- [MDN Web Docs: Browser Developer Tools](https://developer.mozilla.org/en-US/docs/Learn/Common_questions/What_are_browser_developer_tools)