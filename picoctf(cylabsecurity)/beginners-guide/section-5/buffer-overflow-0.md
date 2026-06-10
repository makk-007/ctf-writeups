# Buffer Overflow 0

**Platform:** CyLab Security Academy (formerly PicoCTF)
**Category:** Binary Exploitation
**Difficulty:** Medium
**Learning Path:** Beginner's Guide to the Challenge Library (Section 5)
**Note:** This challenge falls under Binary Exploitation, encountered as part of the Beginner's Guide learning path.

---

## Objective

Trigger a buffer overflow in a vulnerable C program to cause a segmentation fault, which in turn activates a signal handler that prints the flag.

---

## Concepts Learned

| Concept | Description |
|---|---|
| Buffer Overflow | Writing more data into a fixed-size memory buffer than it was allocated to hold, corrupting adjacent memory |
| Stack Memory Layout | Understanding how local variables, buffers, and return addresses are arranged on the call stack |
| `gets()` Vulnerability | A C standard library function that reads unlimited input into a buffer with no bounds checking, making it inherently unsafe |
| `strcpy()` Vulnerability | A string copy function that does not check whether the destination buffer is large enough to hold the source data |
| Segmentation Fault (SIGSEGV) | A fault triggered when a program attempts to access memory outside its permitted region |
| Signal Handlers | Functions registered to execute automatically when the operating system sends a specific signal to a process |
| Netcat for Remote Exploitation | Using `nc` to send crafted input to a remotely running vulnerable service |

---

## Approach

The challenge provided C source code for a vulnerable program running on a remote server. Reading the source was the essential first step.

**Analysing the vulnerable code:**
Opening `vuln.c` in neovim revealed the overflow chain clearly. The program defined two buffers: `buf1` with 100 bytes allocated and `buf2` with only 16 bytes. The vulnerability arose from how input moved through the program:

```
gets()      →  reads unlimited input into buf1[100]
vuln(buf1)  →  strcpy copies buf1 into buf2[16]
buf2[16]    →  overflows when input exceeds 16 bytes
overflow    →  corrupts memory beyond buf2
segfault    →  triggers the SIGSEGV signal handler
handler     →  prints the flag
```

Two unsafe functions compounded the vulnerability. `gets()` performs no bounds checking whatsoever, reading input until a newline regardless of the destination buffer size. `strcpy()` then copies that unchecked input into a much smaller buffer, guaranteeing an overflow whenever the input exceeds 16 bytes.

**The exploitation goal:**
Unlike typical buffer overflow exploitation which aims to control the instruction pointer and redirect execution, this challenge required only triggering a segmentation fault. The program already had a SIGSEGV signal handler registered, which would print the flag automatically when the fault occurred. The task was simply to provide enough input to overflow `buf2` and corrupt surrounding memory.

**Crafting and sending the payload:**
Any input longer than 16 bytes would cause the overflow. I generated a 200-byte string of repeated characters and piped it directly to the remote service via netcat:

```
python3 -c "print('A' * 200)" | nc saturn.picoctf.net 54653
```

The overflow corrupted memory as expected, the SIGSEGV signal fired, the handler executed, and the flag was printed.

---

## Key Takeaway

`gets()` is so fundamentally unsafe that it has been removed from the C standard library in C11 and its use generates compiler warnings. Any program using `gets()` has an exploitable buffer overflow by design. The same applies to `strcpy()` when the destination buffer is smaller than the source. These are not subtle bugs: they are well-documented, decades-old vulnerabilities that persist in legacy code. Recognising unsafe C functions on sight is a foundational skill in binary exploitation and secure code review.

---

## Reflection

This was a conceptually significant challenge because it introduced memory corruption as an exploitation primitive, something qualitatively different from all previous challenges in the learning path. Understanding the stack layout and the overflow chain required thinking about how the program behaves at the memory level, not just at the source code level. The fact that the solution required only triggering a fault rather than redirecting execution made this an ideal introduction: the mechanics of the overflow were clear without the added complexity of shellcode or return-oriented programming.

---

## References

- [OWASP: Buffer Overflow](https://owasp.org/www-community/vulnerabilities/Buffer_Overflow)
- [Linux man page: gets (deprecated)](https://linux.die.net/man/3/gets)
- [Linux man page: signal](https://linux.die.net/man/7/signal)
- [Smashing the Stack for Fun and Profit (Aleph One)](http://phrack.org/issues/49/14.html)