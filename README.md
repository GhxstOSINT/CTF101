# 🗡️ CTF-101: The Ultimate Hacker's Arsenal

> A highly curated, battle-tested collection of the most effective tools for Capture The Flag (CTF) competitions, bridging the gap between raw command-line power and rapid web utilities.

---

## 📑 Table of Contents
1. [Introduction & Purpose](#-introduction--purpose)
2. [Tool Rankings: At a Glance](#-tool-rankings-at-a-glance)
3. [Part 1: Network Enumeration & Web Security](#-part-1-network-enumeration--web-security)
4. [Part 2: Cryptography & Esoteric Languages](#-part-2-cryptography-password-cracking--esoteric-languages)
5. [Part 3: Steganography & Forensics](#-part-3-steganography--forensics)
6. [Part 4: Reverse Engineering & Binary Exploitation](#-part-4-reverse-engineering--binary-exploitation-pwn)
7. [Contributing](#-contributing-to-the-arsenal)

---

## 🏕️ Introduction & Purpose

Welcome to the ultimate hacker's arsenal. If you've ever found yourself staring at a terminal at 2 AM during a grueling Capture The Flag (CTF) competition, wondering which of the hundred different scanning tools you are supposed to use, this repository is for you.

### The Origin of This Arsenal
This project stands as a testament to relentless building—born out of a drive to push through a massive 100-project coding gauntlet to master different facets of computer science and cybersecurity. 

During intense, themed CTF events (especially those driven by heavy OWASP standards), time is your most valuable resource. The cybersecurity community has incredible resources, but they are often fragmented. You have hardcore, command-line "survival guides" which are brilliant for living off the land but can be intimidating to parse quickly. On the flip side, you have highly accessible, GUI-focused dashboards. 

**The purpose of this repository is to bridge that gap.** It merges the raw exploitation power of the command line with the rapid-response speed of online web utilities, creating a single, master cheat sheet. 

### The Core Mindset
To effectively use this repository and succeed in competitions, you must adopt three core principles:
* **Low-Hanging Fruit First:** Never start by trying to reverse-engineer a complex binary if you haven't checked for hidden web directories (`robots.txt`), anonymous network logins, or basic image metadata.
* **Live Off the Land:** If a target machine doesn't have the software you need, you must know how to upload your own static binaries.
* **Automate the Boring Stuff:** If an online tool can run five steganography checks simultaneously in the background, use it. Save your brainpower for the actual puzzle.

---

## 🏆 Tool Rankings: At a Glance

We rank tools based on **Ease of Use (1-10)** and **Power/Expansiveness (1-10)**. 

| Tool | Category | Ease | Power | Best For |
| :--- | :--- | :--- | :--- | :--- |
| **CyberChef** | General/Crypto | 10 | 9 | Rapid data manipulation and decoding. |
| **Aperi'Solve** | Steganography | 10 | 8 | "Lazy" automated image analysis. |
| **Stegseek** | Steganography | 7 | 9 | Blazing fast brute-forcing of hidden files. |
| **CrackStation** | Cryptography | 10 | 9 | Instant hash cracking for standard algorithms. |
| **Binwalk** | Forensics | 7 | 10 | Ripping hidden files out of other files. |
| **enum4linux** | Networking | 6 | 9 | Dumping Windows/Samba users and policies. |
| **JADX** | Reverse Engineering| 9 | 9 | Turning Android APKs back into readable Java. |
| **DogBolt** | Reverse Engineering| 10| 9 | Cloud-based multi-decompilation of binaries. |

---

## 🌐 Part 1: Network Enumeration & Web Security

Before you can hack a target, you need to know exactly what it is running. This section covers the "low-hanging fruit" of port scanning, Samba shares, and web application vulnerabilities. 

### 📡 Network & Port Enumeration Tools

#### 1. smbmap
**Ease of Use:** 7/10 | **Power:** 8/10
* **What it does & How it works:** Think of this as a scout that knocks on all the doors of a Windows network to see which ones are unlocked. It talks to the Server Message Block (SMB) protocol to list all shared network folders and checks your read/write permissions.
* **The Scenario:** Imagine an office where everyone shares files on a central drive. `smbmap` finds the "HR_Documents" folder that someone accidentally left open to the public without requiring a password.
* **Usage & Commands:** `smbmap -H <TARGET_IP> -u anonymous`

#### 2. enum4linux
**Ease of Use:** 6/10 | **Power:** 9/10
* **What it does & How it works:** It’s a gossip column for Windows networks. It aggressively queries the server using built-in Windows protocols to dump a massive list of all registered users, groups, and password rules.
* **The Scenario:** Before you can brute-force a login, you need usernames. You run this tool, it asks the server, "Hey, who works here?" and hands you a list revealing the usernames `admin`, `guest`, and `akshay.dev`.
* **Usage & Commands:** `enum4linux -a <TARGET_IP>`

#### 3. smbclient
**Ease of Use:** 5/10 | **Power:** 7/10
* **What it does & How it works:** It is basically an FTP app (like FileZilla) but specifically for Windows shares. It gives you a terminal interface to browse folders, download (`get`), or upload (`put`) files.
* **The Scenario:** `smbmap` told you the server door is open. `smbclient` is you actually walking through the door. You connect, type `ls` to look around, and grab the `secret_passwords.txt` file.
* **Usage & Commands:** `smbclient -m SMB2 -N //10.10.10.125/Reports`

#### 4. impacket (mssqlclient.py)
**Ease of Use:** 4/10 | **Power:** 10/10
* **What it does & How it works:** A Python script that acts like a legit Microsoft SQL database client. If you have the credentials, it logs you in and lets you type SQL queries. More importantly, it can activate hidden developer features to run raw terminal commands.
* **The Scenario:** You find database credentials in some leaked source code. You use this tool to log in, but instead of just stealing tables, you tell the database to run `whoami` on the host machine. Boom, remote code execution.
* **Usage & Commands:** `mssqlclient.py username@<TARGET_IP> -windows-auth` then type `SQL> enable_xp_cmdshell`

### 🕸️ Web Security Tools

#### 5. EditThisCookie
**Ease of Use:** 10/10 | **Power:** 6/10
* **What it does & How it works:** A browser extension that lets you peek at and rewrite the "sticky notes" (session cookies) a website leaves in your browser to remember who you are.
* **The Scenario:** You log into a site normally, and the site gives you a cookie saying `role=guest`. You pop open the extension, literally backspace "guest", type "admin", and hit refresh. Suddenly you have the keys to the castle.

#### 6. GitDumper.sh / Git-Dumper
**Ease of Use:** 8/10 | **Power:** 9/10
* **What it does & How it works:** If a developer accidentally uploads their hidden `.git` folder (the version control history) to the live web server, this tool downloads the entire thing so you can rebuild their codebase locally.
* **The Scenario:** A dev hardcoded an API key, realized their mistake, and deleted it in the next update. Since the `.git` folder was left online, you download it, check the "commit history", and pull the deleted key right out of the past.
* **Usage & Commands:** `git-dumper http://target-website.com/.git/ output_directory`

#### 7. CSP Evaluator
**Ease of Use:** 9/10 | **Power:** 7/10
* **What it does & How it works:** It acts like a building inspector for a website's Content Security Policy. It reads the rules the site uses to block malicious scripts and tells you exactly where the loopholes are.
* **The Scenario:** You're trying to inject a malicious popup (XSS) into a webpage, but it's being blocked. You put the URL into this tool, and it tells you, "Hey, this site trusts scripts from Google." You disguise your payload to look like it's coming from Google, and it bypasses the filter.

---

## 🔒 Part 2: Cryptography, Password Cracking & Esoteric Languages

### 🔑 Hash Cracking & Identification

#### 1. Hash Identifier & CrackStation
**Ease of Use:** 10/10 | **Power:** 9/10
* **What it does & How it works:** Hashes are one-way math scrambles of passwords. `hash-identifier` guesses the math formula used (like MD5 or SHA-256). `CrackStation` is a giant, 15-terabyte database of pre-scrambled dictionary words to instantly reverse it.
* **The Scenario:** You steal a database and the admin password is `5d41402abc4b2a76b9719d911017c592`. You feed it to CrackStation, which essentially says, "I've seen that exact scramble before! The original word was 'hello'."

#### 2. Crunch
**Ease of Use:** 6/10 | **Power:** 8/10
* **What it does & How it works:** A brute-force wordlist generator. You give it rules (like length and characters), and it spits out a text file with absolutely every possible combination.
* **The Scenario:** You know the target's server PIN is exactly 4 digits. You tell Crunch to make a list from 0000 to 9999, which you then feed into your password cracker to automate the guessing.
* **Usage & Commands:** `crunch 4 4 0123456789 -o pin_list.txt`

### 🧮 Classical & Modern Cryptography

#### 3. dCode.fr & QuipQuip
**Ease of Use:** 10/10 | **Power:** 10/10
* **What it does & How it works:** The ultimate decoders for classic cryptography. They use frequency analysis (knowing 'E' is the most common letter in English) to automatically guess and break substitution ciphers.
* **The Scenario:** You find a message that looks like gibberish (`khoor zruog`). You drop it into dCode's Caesar Cipher tool, and it instantly shifts the letters back to read `hello world`.

#### 4. RsaCtfTool & FactorDB

**Ease of Use:** 5/10 | **Power:** 10/10
* **What it does & How it works:** RSA encryption relies on multiplying giant prime numbers. If the numbers are too small or generated poorly, this Python script uses heavy math to factor them and crack the private key.
* **The Scenario:** A challenge gives you a locked file and a public key. You run this tool, it realizes the CTF creator used weak math, factors the primes, and decrypts the file for you automatically.
* **Usage & Commands:** `python3 RsaCtfTool.py -n <modulus> -e <exponent> --uncipher <ciphertext>`

### 👽 Esoteric & Joke Languages

#### 5. Brainfuck, Ook! & Rockstar
* **What it does & How it works:** Joke programming languages designed to look like gibberish. They compile and run like real code but use weird syntax. Brainfuck uses only punctuation (`> < + - . , [ ]`). Rockstar disguises code as 1980s hair metal lyrics.
* **The Scenario:** You find a text file filled with 80s rock lyrics. You run it through the Rockstar web interpreter, and it turns out the lyrics are actually a functioning script that prints out the flag.

---

## 🎨 Part 3: Steganography & Forensics

### 🖼️ Steganography (Images & Audio)

#### 1. Aperi'Solve
**Ease of Use:** 10/10 | **Power:** 9/10
* **What it does & How it works:** An automated steganography web dashboard. Steganography is hiding secret data inside the pixels or metadata of an image. This tool runs five different forensic checks at once in the cloud.
* **The Scenario:** You are given a picture of a cat. You upload it to Aperi'Solve, and it finds a secret hidden zip file buried inside the image's hex code that you couldn't see with the naked eye.

#### 2. Stegsolve.jar

**Ease of Use:** 8/10 | **Power:** 8/10
* **What it does & How it works:** A tool that acts like X-ray glasses for images. It lets you strip away specific color layers (Red, Green, Blue) to see what's hidden underneath.
* **The Scenario:** An image looks entirely black. You load it up, filter out the Red and Green layers, and in the isolated Blue layer, a hidden QR code appears.

#### 3. Stegseek
**Ease of Use:** 7/10 | **Power:** 9/10
* **What it does & How it works:** A blazing-fast password cracker specifically designed for files hidden inside images or audio tracks using `steghide`. 
* **The Scenario:** You know there's a secret file inside `sunset.jpg` but it requires a passphrase. You point Stegseek at it with a dictionary file, and in two seconds, it guesses the password and spits out the hidden file.
* **Usage & Commands:** `stegseek target_image.jpg rockyou.txt`

#### 4. Sonic Visualizer

**Ease of Use:** 8/10 | **Power:** 7/10
* **What it does & How it works:** Software that turns sound waves into visual graphs (spectrograms), allowing you to "see" the frequencies of audio.
* **The Scenario:** You download a `.wav` file that sounds like old dial-up internet screeching. You look at the spectrogram, and the frequencies literally draw the words "FLAG FOUND" across the screen.

### 🕵️‍♂️ Digital Forensics & File Carving

#### 5. Binwalk & Hachoir-Subfile
**Ease of Use:** 7/10 | **Power:** 10/10
* **What it does & How it works:** Scanners that look for "magic bytes"—the unique hex signatures that identify file types (like how every PDF starts with `%PDF`). They carve out files that have been maliciously glued together.
* **The Scenario:** Someone hid a `.zip` archive inside a `.png` image. When you double-click the image, it just shows a picture. But `binwalk` sees the hidden `.zip` signature in the raw data and rips it out for you.
* **Usage & Commands:** `binwalk -e target_file`

#### 6. Autopsy

**Ease of Use:** 6/10 | **Power:** 10/10
* **What it does & How it works:** Professional digital forensics software. It reads raw hard drive dumps and bypasses the operating system to find files that were "deleted" but not fully overwritten on the disk.
* **The Scenario:** The challenge gives you a USB drive image. The suspect "deleted" their chat logs. You load the image into Autopsy, click the "Deleted Files" tab, and recover the chat logs instantly.

---

## ⚙️ Part 4: Reverse Engineering & Binary Exploitation (Pwn)

### 📱 Mobile & Java Reversing

#### 1. JADX-GUI & MobSF

**Ease of Use:** 9/10 | **Power:** 10/10
* **What it does & How it works:** Decompilers for Android. They take the compiled, unreadable app (`.apk`) and reverse-engineer it back into human-readable Java code. 
* **The Scenario:** You need a premium key for an app. You load the APK into JADX, search the code for the word "password", and find the developer accidentally left the admin login hardcoded in plain text.

### 💻 .NET, Python & C/C++ Reversing

#### 2. dnSpy
**Ease of Use:** 9/10 | **Power:** 10/10
* **What it does & How it works:** A decompiler specifically for Windows apps made with .NET/C#. It lets you read the source code, change the logic, and re-save the working app.
* **The Scenario:** A game won't let you pass level 1 without paying. You decompile it, find the `is_paid_user` variable, change it from `false` to `true`, recompile the `.exe`, and play the whole game for free.

#### 3. DogBolt
**Ease of Use:** 10/10 | **Power:** 9/10
* **What it does & How it works:** An online platform that runs multiple heavy-duty decompilers (like Ghidra and Binary Ninja) at the same time in the cloud.
* **The Scenario:** You have a compiled Linux binary (`.elf`). You upload it, and DogBolt translates the raw machine code back into C++ so you can see exactly how the program handles memory.

#### 4. Buffer Overflow Basics (readelf & dmesg)

**Ease of Use:** 4/10 | **Power:** 10/10
* **What it does & How it works:** Exploiting bad memory management. If a program asks for your name and only expects 10 letters, giving it 100 letters might overflow into critical system memory. You can use this overflow to overwrite the "Instruction Pointer" and take control of the program.
* **The Scenario:** You type a massive string of "A"s into an input box. The program crashes. You use `dmesg` to see exactly where it crashed, and `readelf` to find the address of a hidden "admin" function. You then craft an input that perfectly overwrites the memory to trigger that admin function and print the flag.

---

## 📚 OSS Documentation & Contribution Guide

CTF-101 is an actively maintained Open Source project. The cybersecurity landscape shifts rapidly, and a tool that wins a CTF today might be obsolete tomorrow. 

We welcome contributions from fellow CTF players, security researchers, and developers. To keep this cheat sheet clean, readable, and highly practical during timed competitions, please follow the documentation standards below.

### 🏗️ Repository Structure
The arsenal is strictly categorized by CTF domains:
1. **Part 1:** Network Enumeration & Web Security
2. **Part 2:** Cryptography, Password Cracking & Esoteric Languages
3. **Part 3:** Steganography & Forensics
4. **Part 4:** Reverse Engineering & Binary Exploitation (Pwn)

If your tool doesn't fit into these, open an issue first to discuss adding a new category.

### 📝 The Standard Tool Template
If you are submitting a Pull Request (PR) to add a new tool, **you must use this exact Markdown format**. We do not want 5-page essays. Keep it punchy, practical, and focused on real-world usage.

Copy this template for your PR:

#### [Tool Name]
**Ease of Use:** [1-10] | **Power:** [1-10]
* **What it does & How it works:** [1-2 sentences explaining the tool in plain tech English. No fluff.]
* **The Scenario:** [A relatable, real-world CTF scenario where you would actually use this tool.]
* **Usage & Commands:** `[Insert the exact terminal command or web link here]`

### 🛠️ How to Contribute
1. **Fork the repository** to your local GitHub account.
2. **Create a feature branch:** `git checkout -b add/your-tool-name`
3. **Add your tool** using the template above in the correct category section.
4. **Commit your changes** with a clear message: 
   `git commit -m "Add [Tool Name] to [Category]"`
5. **Push to your branch:** `git push origin add/your-tool-name`
6. **Open a Pull Request (PR)** against the `main` branch of this repository. In your PR description, briefly explain why this tool is a game-changer.

### ⚖️ License
This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details. You are free to use, modify, and distribute this cheat sheet.

---
*Maintained with ❤️ by the open-source CTF community.*
