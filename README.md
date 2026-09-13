
# Napping — Hack The Box Write-up

## Overview

Napping is a Hack The Box machine that involves web application enumeration, initial access, lateral movement between users, and Linux privilege escalation.

The objective is to gain access to the machine and escalate privileges to root.

## 1. Initial Reconnaissance

During my initial enumeration, I inspected the website's HTML and noticed the following attribute:

```html
target="_blank"
```

I researched potential vulnerabilities associated with this attribute and discovered a vulnerability that could expose authentication information.

## 2. Initial Access — Daniel

I exploited the vulnerability by uploading a file, which allowed me to obtain Daniel's authentication information.

Using the information obtained, I was able to establish an SSH connection as Daniel.

## 3. Lateral Movement — Daniel to Adrian

After gaining access to Daniel's account, I explored the available files and discovered a Python script named `query.py` associated with Adrian.

I found that I could modify the script. I edited it using `nano` and replaced its contents with a reverse shell payload.

This allowed me to obtain SSH access as Adrian.

## 4. Privilege Escalation — Adrian to Root

While enumerating Adrian's privileges, I discovered that Vim could be executed using `sudo` without requiring a password.

I used the following command to spawn a shell with elevated privileges:

```bash
sudo /usr/bin/vim -c ':!/bin/sh'
```

Vim allowed me to execute a shell command, resulting in a root shell.

With root access, I successfully completed the machine.

## 5. Summary

The attack chain involved:

1. Discovering a potential web application vulnerability.
2. Obtaining Daniel's authentication information.
3. Gaining SSH access as Daniel.
4. Modifying `query.py` to obtain Adrian's SSH access.
5. Exploiting passwordless sudo access to Vim to escalate privileges to root.

## 6. Key Takeaways

- Inspecting HTML can reveal clues about potential web application vulnerabilities.
- Enumerating files and scripts can help identify opportunities for lateral movement.
- Reviewing sudo permissions is an important part of Linux privilege-escalation enumeration.
- Allowing unrestricted applications to run with elevated privileges can create serious security risks.

---

**Note:** This write-up is based on my personal notes from completing the machine. Some commands and intermediate steps were not recorded.
