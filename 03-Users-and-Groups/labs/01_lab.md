# Lab 01 — User Management

## Objective

Create, inspect, modify, and delete Linux user accounts.

## Environment

- OS: RHEL
- Access: Local terminal or SSH
- Privileges: `sudo` access

## Tasks

### Task 1: Create a User

```bash
sudo useradd -m labuser
sudo passwd labuser
```

Set a password when prompted.

Verify:

```bash
id labuser
getent passwd labuser
ls -ld /home/labuser
```

**Expected result:** The user exists, has a UID and primary GID, and has a home directory.

### Task 2: Inspect Account Information

```bash
id labuser
getent passwd labuser
```

A typical `/etc/passwd` entry looks like:

```text
labuser:x:1001:1001::/home/labuser:/bin/bash
```

The actual UID, GID, and shell may differ.

The fields identify the username, password placeholder, UID, primary GID, comment, home directory, and login shell.

### Task 3: Modify the User

Set an account comment:

```bash
sudo usermod -c "RHCSA Lab Account" labuser
```

Change the login shell:

```bash
sudo usermod -s /bin/bash labuser
```

Verify:

```bash
getent passwd labuser
```

### Task 4: Delete the Lab User

First inspect the account:

```bash
id labuser
```

Then remove it and its home directory:

```bash
sudo userdel -r labuser
```

Verify:

```bash
getent passwd labuser
ls -ld /home/labuser
```

The user entry and home directory should be absent if deletion completed successfully.

## Verification Checklist

- Created a user with a home directory.
- Set a password.
- Inspected the UID, GID, and account entry.
- Modified the comment and shell.
- Deleted the lab account.

## Cleanup

The lab account is deleted in Task 4.

Only use this procedure for the dedicated `labuser` account created in this exercise. Never delete an existing account simply because it appears in an example.

## Key Takeaway

Linux user administration involves creating accounts, configuring their properties, verifying account information, and removing accounts safely.
