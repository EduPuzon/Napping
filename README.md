
# Napping — Tryhackme Write-up

## Overview

Napping is a tryhackme machine that involves web application enumeration, initial access, lateral movement between users, and Linux privilege escalation.

The objective is to gain access to the machine and escalate privileges to root.

## 1. Initial Reconnaissance

During my initial enumeration, I inspected the website's HTML and noticed the following attribute:

```html
target="_blank"
```

I researched potential vulnerabilities associated with this attribute and discovered a vulnerability that could expose authentication information.


## 2. Initial Access — Daniel

While inspecting the website's HTML, I discovered a link using `target="_blank"` without appropriate protection against reverse tabnabbing.

I researched the vulnerability and learned that a malicious page opened in a new tab could potentially redirect the original tab to a fake login page through `window.opener`.

I hosted a malicious page and used the website's functionality to get the victim to open it. This allowed me to capture Daniel's authentication information through HTTP traffic.

Using the recovered credentials, I established an SSH connection as Daniel.

## 3. Lateral Movement — Daniel to Adrian

After gaining access as Daniel, I checked the available files and permissions.

I discovered that Daniel belonged to the `administrators` group and had write access to Adrian's `query.py` script.

The script ran periodically with Adrian's privileges. I modified it to execute a reverse shell, allowing me to obtain access as Adrian.

## 4. Privilege Escalation — Adrian to Root

While checking Adrian's sudo permissions, I discovered that Vim could be executed as root without a password.

I used the following command to spawn a shell:

```bash
sudo /usr/bin/vim -c ':!/bin/sh'
```

Since Vim was running with root privileges, the shell inherited those privileges.

I successfully obtained root access and completed the machine.

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
