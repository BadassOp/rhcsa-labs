# Lab 02 — Ownership and Special Permissions

## Objective

Practice changing file ownership and group ownership, and understand SUID, SGID, and the sticky bit.

## Environment

- OS: RHEL
- Access: Local terminal or SSH
- Privileges: Regular user and `sudo` access

## Tasks

### Task 1: Create a Test User and Group

```bash
sudo groupadd permgroup
sudo useradd -m permuser

mkdir -p ~/ownership-lab
touch ~/ownership-lab/file.txt
```

### Task 2: Change Ownership

```bash
sudo chown permuser:permgroup ~/ownership-lab/file.txt
ls -l ~/ownership-lab/file.txt
```

Verify ownership:

```bash
stat ~/ownership-lab/file.txt
```

The owner should be `permuser`, and the group should be `permgroup`.

### Task 3: Set SGID on a Directory

```bash
sudo mkdir /opt/perm-shared
sudo chown root:permgroup /opt/perm-shared
sudo chmod 2775 /opt/perm-shared
ls -ld /opt/perm-shared
```

Create a file as a member of the group, or use a temporary test account that belongs to it. Verify that the file inherits the directory's group.

**Expected result:** The directory displays an `s` in the group-execute position. SGID on a directory commonly causes new entries to inherit its group.

### Task 4: Demonstrate the Sticky Bit

```bash
sudo mkdir /opt/perm-sticky
sudo chmod 1777 /opt/perm-sticky
ls -ld /opt/perm-sticky
```

Expected permission pattern:

```text
drwxrwxrwt
```

The sticky bit restricts deletion or renaming of entries in a shared writable directory to the entry owner, directory owner, or a privileged process.

### Task 5: Inspect SUID

Inspect a known executable:

```bash
ls -l /usr/bin/passwd
```

On typical RHEL installations, `/usr/bin/passwd` has SUID set. The exact output can vary by configuration.

**Do not set SUID on arbitrary files.** It can grant processes additional privileges and create security risks.

## Verification Checklist

- Created a test user and group.
- Changed file ownership.
- Inspected ownership using `stat`.
- Set and verified SGID on a directory.
- Set and verified the sticky bit.
- Inspected an executable with SUID.

## Cleanup

Remove the test directories and files:

```bash
rm -r ~/ownership-lab
sudo rm -r /opt/perm-shared /opt/perm-sticky
```

Remove the test account and group:

```bash
sudo userdel -r permuser
sudo groupdel permgroup
```

Run these commands only for the resources created in this lab. If you created additional test accounts or files, inspect and remove them separately.

## Key Takeaway

Ownership determines the owner and group associated with an object. Special permission bits modify how certain access and execution behaviors work.
