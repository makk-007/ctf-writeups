# Vault Door Training

**Platform:** CyLab Security Academy (formerly PicoCTF)
**Category:** Reverse Engineering
**Difficulty:** Easy
**Learning Path:** Beginner's Guide to the Challenge Library (Section 5)
**Note:** This challenge falls under Reverse Engineering, encountered as part of the Beginner's Guide learning path.

---

## Objective

Retrieve a password hardcoded in Java source code by reading and understanding the program's authentication logic.

---

## Concepts Learned

| Concept | Description |
|---|---|
| Java Source Code Analysis | Reading `.java` files to understand program logic and identify security weaknesses |
| Hardcoded Credentials in Java | Passwords or secrets embedded directly as string literals in compiled or source Java code |
| Static Analysis Across Languages | Applying the same source-reading approach used in Python challenges to a Java context |
| Reverse Engineering Fundamentals | Understanding a program's intended behaviour by examining its source, rather than running it |

---

## Approach

The challenge provided the Java source code for a vault door authentication system. The scenario framed it as a training simulation, hinting that the protection level would be minimal.

**Reading the source:**
Opening the file in neovim revealed the password check implementation. The program compared user input against a password value stored directly as a plaintext string literal in the source code. No hashing, no obfuscation, no external lookup. The password was immediately visible on inspection.

This is the Java equivalent of what Level 1 of the PW Crack series demonstrated in Python: hardcoded credentials offer no real protection to anyone with access to the source file.

**Connection to prior challenges:**
Having worked through the PW Crack series, the pattern here was immediately familiar. The language changed from Python to Java, but the vulnerability was identical. Reading the source before running anything remained the correct first step, and it was sufficient to solve the challenge without executing a single line of code.

---

## Key Takeaway

Hardcoded credentials are a critical vulnerability regardless of the programming language. Java source files, like Python scripts, are fully readable text. Even compiled Java `.class` files can be decompiled to recover source code with reasonable fidelity using tools like `javap` or dedicated decompilers. Security cannot rely on keeping source code secret, particularly when the code itself contains the secret.

---

## Reflection

This challenge reinforced that the core skill being developed across the Beginner's Guide is source code literacy: the ability to read code in different languages, identify what it does, and locate security-relevant information. The vault door series appears to be a multi-level progression similar to PW Crack, suggesting the authentication mechanisms will grow more sophisticated in later levels.

---

## References

- [OWASP: Use of Hard-coded Credentials](https://owasp.org/www-community/vulnerabilities/Use_of_hard-coded_password)
- [Oracle Java Docs: String Literals](https://docs.oracle.com/javase/specs/jls/se17/html/jls-3.html#jls-3.10.5)