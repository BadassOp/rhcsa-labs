# Lab 01 — Linux Permission Basics

## Objective

Understand Linux permission bits and practice changing and verifying file and directory permissions.

## Environment

- OS: RHEL
- Access: Local terminal or SSH
- Privileges: Regular user

## Tasks

### Task 1: Create Test Files

```bash
mkdir -p ~/permissions-lab
cd ~/permissions-lab

touch file.txt
mkdir directory
echo "Permission testing" > file.txt
```

Inspect permissions:

```bash
ls -l
ls -ld directory
```

### Task 2: Set Numeric Permissions

Set permissions to `640`:

```bash
chmod 640 file.txt
ls -l file.txt
```

Expected permissions:

```text
-rw-r-----
```

Interpretation:

- Owner: read and write.
- Group: read.
- Others: no permissions.

Set permissions to `750` on the directory:

```bash
chmod 750 directory
ls -ld directory
```

Expected permissions:

```text
drwxr-x---
```

### Task 3: Use Symbolic Permissions

```bash
chmod u+x file.txt
chmod g-w file.txt
chmod o-r file.txt
ls -l file.txt
```

Inspect the result and explain which permissions changed.

### Task 4: Test Directory Traversal

```bash
touch directory/inside.txt
chmod 600 directory
ls directory
```

A regular user cannot list the directory because it lacks read permission, and cannot access entries within it by pathname because it lacks execute permission.

Restore the directory permissions:

```bash
chmod 700 directory
ls directory
```

### Task 5: Inspect Metadata

```bash
stat file.txt
ls -l file.txt
```

Compare the permission information displayed by both commands.

## Verification Checklist

- Set numeric permissions using `chmod`.
- Modified permissions symbolically.
- Inspected file and directory permissions.
- Explained the difference between file and directory execute permissions.
- Verified metadata with `stat`.

## Cleanup

After checking the contents, remove only the lab directory:

```bash
cd ~
rm -r ~/permissions-lab
```

Ensure the path is correct before running the deletion command.

## Key Takeaway

Permissions control access to files and directories. Directory execute permission is required for pathname traversal.
