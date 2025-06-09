---
layout: post
title: How Linux System Stores User Passwords
subtitle: Hashing, Salting, Fast and Low Hashing Algorithm
tags: [Security, Hashing, Salting, Operating System]
comments: false
mathjax: false
author: Longfei Jiao
last-updated: 2025-06-09
---

## /etc/passwd

The /etc/passwd file is a plain-text database that contains a list of the system's user accounts. It's the primary source of user information for the operating system.

The core purpose of this file is to map essential pieces of information to each user account, including:

* Username: The name you use to log in.
* User ID (UID): A unique number the system uses internally to identify the user.
* Group ID (GID): The unique number of the user's primary group.
* Home Directory: The user's personal directory (e.g., /home/username).
* Login Shell: The program that runs when the user logs in (e.g., /bin/bash).

The file is structured as a series of lines, where each line represents one user account. Each line is made up of seven colon-separated (:) fields.

**Example of a User Account**  
    jdoe:x:1001:1001:John Doe:/home/jdoe:/bin/bash

1. username (jdoe)
The name used for logging in. It must be unique.
2. password (x)
This is the most misunderstood field. In modern systems, this field does not contain the password.
**The x is a placeholder that tells the system to look for the actual encrypted password in a separate, more secure file: /etc/shadow.**
Historical Context: Decades ago, the encrypted password hash was stored directly in this field. This was a security risk because /etc/passwd needs to be readable by all users on the system. Anyone could copy the file and try to crack the passwords offline. The "shadow password" system was created to fix this.
3. UID (User ID) (1001)
A unique integer that the kernel uses to identify a user. Permissions on files and processes are tracked by UID, not by username.
By convention:
0: The root user (the superuser).
1-999: System accounts (for services like sshd, www-data, nobody).
1000+: Regular user accounts.
4. GID (Group ID) (1001)
The ID of the user's primary group. Groups are used to manage permissions for multiple users at once. Group information is stored in the /etc/group file.
5. GECOS or Comment Field (John Doe)
A field for miscellaneous user information. It's most commonly used for the user's full name. It can also store other details like office number or phone number, separated by commas. The name "GECOS" is a historical artifact from an old mainframe system (General Electric Comprehensive Operating Supervisor).
6. home_directory (/home/jdoe)
The absolute path to the user's home directory. This is where a user is placed after logging in and where their personal files and configuration files (dotfiles) are stored.
7. shell (/bin/bash)
The absolute path to the user's default login shell (command-line interpreter).
For regular users, this is typically /bin/bash, /bin/zsh, or /bin/sh.
For system accounts or users who are not allowed to log in, this is often set to /sbin/nologin or /bin/false. This effectively disables shell access for that account.


**Example of a System Account**  
    sshd:x:111:65534::/run/sshd:/usr/sbin/nologin

* User: sshd (for the Secure Shell daemon)
* UID: 111 (a low number, indicating a system account)
* Home Directory: /run/sshd (a temporary, non-standard home)
* Shell: /usr/sbin/nologin (this account cannot be used to log in interactively)

## /etc/shadow

Passwords are saved in the /etc/shadow file using a secure, one-way process involving hashing and salting.

/etc/passwd is world-readable, meaning any user on the system can see its contents. Storing password information there, even encrypted, is a major security risk. The /etc/shadow file was created to solve this. It is only readable by the root user and members of the shadow group. This prevents regular users from accessing the encrypted password data.

In addition, A unique, random salt for every user is added after the user's password before going through the hashing algorithm. Now, even if two users have the same password, their stored hashes will be completely different because their salts are different. This makes rainbow table attacks ineffective.

**Example of a user's password in /etc/shadow**

    jlf:$y$j9T$eF2.Wimpc6fzSdrdzvHge.$95sMQszTzgx37mKyQWJghVKcrqSo45m92HRcEEBDDH6:20247:0:99999:7:::

Here is the meaning of each colon-separated field:

|Field# | Value | Meaning |
|------|-------|---------|
|1| jlf | Username: This entry belongs to the user account named jlf. |
|2| $y$j9T$eF2.Wi<br>mpc6fzSdrdzvH<br>ge.$95sMQszTz<br>gx37mKyQWJgh<br>VKcrqSo45m92<br>HRcEEBDDH6 | Password Hash: This is the encrypted password information. This particular format indicates it's using the yescrypt algorithm, which is a modern, memory-hard hashing function designed to be resistant to GPU and ASIC-based attacks. |
|3| 20247 | Last Password Change: The date the password was last changed, expressed as the number of days since the Unix Epoch (January 1, 1970). The value 20247 corresponds to June 8, 2025. |
|4| 0 | Minimum Password Age: The minimum number of days that must pass before the user is allowed to change their password again. A value of 0 means they can change it at any time. |
|5| 99999 | Maximum Password Age: The maximum number of days the password is valid. The value 99999 is a convention that means the password never expires. |
|6| 7 | Warning Period: The number of days before password expiration that the user will be warned. Since the password is set to never expire (99999), this field is currently inactive. |
|7| (empty) | Inactivity Period: The number of days after a password expires that the account is disabled. An empty field here means this feature is turned off. |
|8| (empty) | Account Expiration Date: An absolute date (in days since the epoch) when the user account will be disabled. An empty field means the account itself never expires. |
|9| (empty) | Reserved Field: This field is unused and reserved for future use. |

**A Deeper Dive of Password Hash Field** (Field 2)

    $y$j9T$eF2.Wimpc6fzSdrdzvHge.$95sMQszTzgx37mKyQWJghVKcrqSo45m92HRcEEBDDH6

It follows the yescrypt format: $id$params$salt$hash

1. $y$ - Algorithm ID
This identifier tells the system that the hash was created using the yescrypt algorithm. This is a step up in security from the older $6$ (SHA-512) because yescrypt is intentionally designed to be "memory-hard," making it much more expensive for attackers to try and crack with specialized hardware.
2. j9T - Cost Parameters
Unlike simpler algorithms, yescrypt includes tunable cost parameters. This value (j9T) defines how much CPU time and memory are required to compute the hash. This allows administrators to make hashing more "expensive" as computers get faster, maintaining a strong defense against brute-force attacks over time.
3. eF2.Wimpc6fzSdrdzvHge. - The Salt
This is the randomly generated salt that was combined with the user's plain-text password before hashing. It is stored here in plain text so the system can reuse it to verify a login attempt.
4. 95sMQszTzgx37mKyQWJghVKcrqSo45m92HRcEEBDDH6 - The Final Hash
This is the actual hash value. It is the result of running the user's password and the salt through the yescrypt algorithm with the specified cost parameters.

## Why use salt?

Without salting, multiple users with the same passwords will have the same hash for their passwords. A hacker could pre-calculate the hash of 10 billion most common passwords (called a rainbow table) and match the hash (if they somehow get the password hash) with a specific password. 

Adding a salt for every user does not consume more computing power for the user, but it makes the rainbow table completely useless. 

If the hacker wants the crack the password by brute force, they need to calculate each password + unique salt for every user, which makes their attack much less effective. 

## Why use yescrypt instead of sha256 algorithm?

The difference of sha256 and yescrypt is the difference of fast and slow algorithms. 

**Fast Algorithms (MD5, SHA-256, SHA-512)**: These were designed for speed. Their goal is to quickly generate a unique signature for a file to check for corruption. On a modern graphics card (GPU), an attacker can calculate billions of these hashes per second.

**Slow, Purpose-Built Algorithms (bcrypt, scrypt, Argon2, and yescrypt in this example)**: These were designed specifically for password hashing. They have a "secret weapon": a configurable "work factor" or "cost" parameter.

The **Work Factor** makes the calculation of the algorithm artificially slow. They force the CPU/GPU to perform thousands or millions of internal computations for just one password hash. 

A modern GPU or GPU may calculate 1 billion hashes per second. Even with a salt, the hacker can try 1 billion possible passwords for a user per second (using a fast hashing algorithm). 

However, if we adjust the work factor in a slow hashing algorithm so that one hash costs 0.25 second for a single core CPU, the hacker with a modern GPU (let'say, it has 2000 times more computing power than a single CPU). It can only try 2000*4 = 8000 passwords per second. In order to try 1 billion passwords, it will take 1B/8kps = 125000 seconds = 34.7 hours! This is the computing time for a single user with no guarantee that a match will be found. 

With an unnoticeable 0.25 second delay each time the user logs in, the hacker's computing time surged from 1 second to 34.7 hours. 