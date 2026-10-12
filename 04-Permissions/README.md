# Linux Permissions and ACLs

Linux permissions control who can read, modify, or execute files and directories. Ownership, permission bits, and Access Control Lists (ACLs) help administrators manage access to system resources.

## Topics Covered

- **File Ownership** — Understand file owners and group ownership.
- **Permission Basics** — Understand read (`r`), write (`w`), and execute (`x`) permissions.
- **Permission Classes** — Understand permissions for the owner, group, and others.
- **Numeric and Symbolic Modes** — Manage permissions using values such as `755` and symbolic expressions such as `u+x`.
- **chmod** — Change file and directory permissions.
- **chown and chgrp** — Change ownership and group ownership.
- **Special Permissions** — Understand SUID, SGID, and the sticky bit.
- **Access Control Lists (ACLs)** — Grant additional permissions to specific users and groups.
- **Default ACLs** — Configure inherited access permissions for newly created items in a directory.
- **Permission Troubleshooting** — Diagnose common access-denied problems.

## Permission Classes

| Class | Meaning      |
| ----- | ------------ |
| `u`   | Owner (user) |
| `g`   | Group        |
| `o`   | Others       |
| `a`   | All classes  |

## Permission Types

| Permission    | Files                           | Directories                                                  |
| ------------- | ------------------------------- | ------------------------------------------------------------ |
| `r` — Read    | Read file contents              | List directory entries                                       |
| `w` — Write   | Modify file contents            | Create, remove, or rename entries, subject to other controls |
| `x` — Execute | Execute the file, if applicable | Traverse or access entries by name                           |

Directory permissions have different practical effects from file permissions. For example, accessing a file by its pathname generally requires execute permission on its parent directories.

## Learning Objectives

- Interpret Linux permission strings and numeric modes.
- Change file permissions and ownership.
- Understand special permission bits.
- Configure and inspect ACLs.
- Diagnose permission-related access failures.
- Verify effective access rather than relying on assumptions.

## Important Commands

`chmod`, `chown`, `chgrp`, `umask`, `ls -l`, `getfacl`, and `setfacl`.

## Scope

This section covers discretionary permissions and ACLs. User and group account creation is covered in the Users and Groups module; SELinux is documented separately.

## Goal

Develop practical access-control skills for Linux system administration and RHCSA preparation.
