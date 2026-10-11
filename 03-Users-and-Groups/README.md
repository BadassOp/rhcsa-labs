# Linux Users and Groups

Linux uses users and groups to identify accounts, organize access, and control which resources users can access.

## Topics Covered

- **User Accounts** — Understand regular users, the root user, and system accounts.
- **Group Management** — Organize users into groups to simplify access management.
- **UID and GID** — Understand User IDs and Group IDs.
- **User Management** — Create, modify, and delete user accounts.
- **Password Management** — Set passwords, manage password expiration, and inspect account status.
- **Account Configuration** — Understand `/etc/passwd`, `/etc/shadow`, and `/etc/group`.
- **Primary and Supplementary Groups** — Understand the difference between a user's primary group and additional group memberships.
- **User Environment** — Understand home directories, login shells, and default account settings.

## Important Configuration Files

| File              | Purpose                                                      |
| ----------------- | ------------------------------------------------------------ |
| `/etc/passwd`     | Stores basic user account information                        |
| `/etc/shadow`     | Stores password hashes and password-aging information        |
| `/etc/group`      | Stores group information and supplementary group memberships |
| `/etc/gshadow`    | Stores protected group information                           |
| `/etc/login.defs` | Defines default settings for user account management         |

## Learning Objectives

- Create and manage Linux user accounts.
- Create groups and manage group membership.
- Understand UID, GID, and account configuration.
- Manage passwords and account expiration.
- Inspect user and group information using command-line tools.
- Understand how users and groups relate to Linux permissions.

## Scope

This section covers user and group administration. File permissions, ownership, ACLs, and special permissions are covered separately.

## Goal

Develop the account-management skills required for practical Linux system administration and RHCSA preparation.
