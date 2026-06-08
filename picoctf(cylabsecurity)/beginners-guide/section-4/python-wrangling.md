# Python Wrangling

**Platform:** CyLab Security Academy (formerly PicoCTF)  
**Category:** General Skills  
**Difficulty:** Medium  
**Learning Path:** Beginner's Guide to the Challenge Library (Section 4)

---

## Objective

Execute a provided Python script from the terminal to decrypt an encrypted file using a supplied password, demonstrating comfort with running scripts and interpreting command-line arguments.

---

## Concepts Learned

| Concept | Description |
|---|---|
| Running Python Scripts | Invoking a `.py` file from the terminal using the `python3` interpreter |
| Command-Line Arguments / Flags | Parameters passed to a script at runtime that alter its behaviour (e.g., `-e` for encrypt, `-d` for decrypt) |
| File Encryption and Decryption | The process of transforming data into an unreadable format (encryption) and reversing that process with a key (decryption) |
| Reading Source Code Before Executing | Inspecting an unfamiliar script to understand its expected inputs and behaviour before running it |

---

## Approach

The challenge provided three files: a Python script (`ende.py`), a password file (`password.txt`), and an encrypted file (`flag.txt.en`). The task was to recover the original flag by running the decryption script correctly.

**Step 1: Read the script before running it.**  
Before executing any unfamiliar script, I read through `ende.py` to understand what it does. This is a fundamental security habit: you should never run code you have not inspected. Reading the script revealed that it performs either encryption or decryption on a file depending on which flag is passed at the command line. The `-e` flag encrypts and the `-d` flag decrypts. The script also prompts for or accepts a password to perform the operation.

This step was important not just for practical reasons, but because understanding the script's logic made the subsequent steps deliberate rather than trial-and-error.

**Step 2: Retrieve the password.**  
With the script's behaviour understood, the next step was to read `password.txt` to obtain the decryption key.

**Step 3: Run the script with the correct arguments.**  
Using the `-d` flag and supplying the encrypted file as the target, the script decrypted `flag.txt.en` and returned the flag.

---

## Commands Used

| Command | Purpose |
|---|---|
| `cat ende.py` | Read the script source to understand its logic and expected arguments |
| `cat password.txt` | Retrieve the decryption password |
| `python3 ende.py -d flag.txt.en` | Execute the script in decrypt mode against the encrypted file |

---

## Key Takeaway

Python scripts are tools, and like any tool, understanding how they work before using them is essential. The ability to read a script, identify its input parameters, and invoke it correctly from the terminal is a practical skill used constantly in security work, whether you are running custom exploit scripts, automation tools, or analysis utilities. The `-e`/`-d` flag pattern here is also a clean introduction to how command-line argument parsing works in Python, a pattern you will encounter repeatedly.

---

## Reflection

The habit of reading source code before execution is one worth internalising early. In a real-world context, running an unreviewed script, especially one that handles file encryption, could have significant consequences. Even in a controlled CTF environment, taking sixty seconds to read the script first was the right approach and directly informed each subsequent step.

---

## References

- [Python Docs: Command Line and Environment](https://docs.python.org/3/using/cmdline.html)
- [Python Docs: argparse - Parser for command-line options](https://docs.python.org/3/library/argparse.html)