# Lab 01 — Filesystem Navigation and Listing

## Objective

Practice navigating the Linux filesystem and inspecting directory contents.

## Environment

- Operating system: RHEL
- Access: Local terminal or SSH session
- Privileges: Regular user

## Tasks

### Task 1: Identify Your Location

```bash
pwd
whoami
hostname
```

**Expected result:** Identify the current directory, username, and hostname.

### Task 2: Inspect the Home Directory

```bash
cd ~
pwd
ls
ls -la
```

**Expected result:** Navigate to your home directory and display its contents, including hidden entries.

### Task 3: Explore System Directories

```bash
cd /etc
pwd
ls
```

Inspect another directory:

```bash
cd /var/log
pwd
ls
```

**Expected result:** Navigate to system configuration and log directories.

### Task 4: Practice Relative Paths

```bash
cd ~
mkdir -p practice/docs
cd practice/docs
pwd
cd ..
pwd
cd ../..
pwd
```

**Expected result:** Understand how relative paths and `..` change your location.

### Task 5: Use Command Help

```bash
man ls
```

Press `q` to exit the manual.

You can also use:

```bash
ls --help
```

## Verification Checklist

- Identified the current directory using `pwd`.
- Listed normal and hidden files.
- Navigated between system directories.
- Created nested directories.
- Used relative paths successfully.
- Read command documentation.

## Cleanup

Remove the practice directories created in this lab:

```bash
rmdir ~/practice/docs
rmdir ~/practice
```

These commands only remove the empty directories created during this exercise. If they contain files, inspect and remove those files separately.

## Key Takeaway

Always understand your current directory and target path before executing file management commands.
