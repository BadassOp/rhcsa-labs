# Users and Groups — Useful Commands

## User Information

| Command                  | Purpose                                         |
| ------------------------ | ----------------------------------------------- |
| `whoami`                 | Display the current username                    |
| `id`                     | Display the current user's UID, GID, and groups |
| `id username`            | Display information about a specific user       |
| `who`                    | Display logged-in users                         |
| `w`                      | Show logged-in users and their activity         |
| `getent passwd username` | Retrieve a user's account entry                 |
| `getent group groupname` | Retrieve a group's entry                        |

## Create and Manage Users

| Command                                | Purpose                                             |
| -------------------------------------- | --------------------------------------------------- |
| `sudo useradd username`                | Create a user account                               |
| `sudo useradd -m username`             | Create a user with a home directory                 |
| `sudo passwd username`                 | Set or change a user's password                     |
| `sudo usermod -c "Full Name" username` | Modify the account comment                          |
| `sudo usermod -s /bin/bash username`   | Change the login shell                              |
| `sudo usermod -aG groupname username`  | Add a user to a supplementary group                 |
| `sudo usermod -L username`             | Lock a user's password                              |
| `sudo usermod -U username`             | Unlock a user's password                            |
| `sudo userdel username`                | Delete a user account                               |
| `sudo userdel -r username`             | Delete a user and its home directory and mail spool |

**Important:** Use `-aG` when adding supplementary group membership. Using `-G` without `-a` can replace existing supplementary group memberships.

## Create and Manage Groups

| Command                            | Purpose                                        |
| ---------------------------------- | ---------------------------------------------- |
| `sudo groupadd groupname`          | Create a group                                 |
| `sudo groupmod -n newname oldname` | Rename a group                                 |
| `sudo groupdel groupname`          | Delete a group                                 |
| `groups username`                  | Display a user's group memberships             |
| `newgrp groupname`                 | Start a shell with a different effective group |

## Password and Account Aging

| Command                             | Purpose                             |
| ----------------------------------- | ----------------------------------- |
| `sudo passwd username`              | Set or change a password            |
| `passwd -S username`                | Display password status             |
| `sudo chage -l username`            | Display password-aging information  |
| `sudo chage -M 90 username`         | Set maximum password age to 90 days |
| `sudo chage -E YYYY-MM-DD username` | Set account expiration date         |

Account expiration and password expiration are different. `chage -E` sets an account expiration date; `chage -M` sets the maximum password age.

## Configuration Files

| Command                         | Purpose                                  |
| ------------------------------- | ---------------------------------------- |
| `cat /etc/passwd`               | Inspect basic account entries            |
| `sudo cat /etc/shadow`          | Inspect protected password-aging entries |
| `cat /etc/group`                | Inspect group entries                    |
| `sudo cat /etc/gshadow`         | Inspect protected group information      |
| `grep '^username:' /etc/passwd` | Find a specific user entry               |

Avoid sharing the contents of `/etc/shadow` or `/etc/gshadow`; these files contain sensitive authentication information.

## Practical Notes

- Use `sudo` when administrative privileges are required.
- Verify changes with `id username`, `getent passwd username`, and `getent group groupname`.
- Existing login sessions may not immediately reflect changed group membership.
- Be cautious when deleting accounts, especially if they own files or run services.
- On RHEL, `useradd` defaults and account creation behavior can depend on system configuration.
