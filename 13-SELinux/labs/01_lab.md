# Lab 01: SELinux Contexts and restorecon

## Objective

Inspect SELinux security contexts, understand file labels, and restore default labels safely.

## Prerequisites

- RHEL VM with SELinux enabled.
- `sudo` privileges.
- SELinux utilities installed.

## Tasks

### 1. Check SELinux status

```bash
getenforce
sestatus
```

Confirm whether SELinux is enforcing, permissive, or disabled.

If SELinux is disabled, do not attempt to enable it as part of this lab. Complete the inspection tasks that are available and investigate the system configuration separately.

### 2. Inspect file contexts

```bash
ls -Z /etc/passwd
ls -Zd /etc
ps -eZ | head
id -Z
```

Record the context of a file, a directory, a process, and your current session.

### 3. Create a test directory

```bash
sudo mkdir -p /srv/selinux-lab
sudo touch /srv/selinux-lab/test.txt
ls -Zd /srv/selinux-lab
ls -Z /srv/selinux-lab/test.txt
```

Observe the assigned labels.

### 4. Inspect the expected file context

```bash
sudo matchpathcon /srv/selinux-lab
sudo matchpathcon /srv/selinux-lab/test.txt
```

Compare the expected labels with the labels shown by `ls -Z`.

### 5. Restore default labels

```bash
sudo restorecon -v /srv/selinux-lab/test.txt
sudo restorecon -Rv /srv/selinux-lab
```

Inspect the labels again:

```bash
ls -Zd /srv/selinux-lab
ls -Z /srv/selinux-lab/test.txt
```

`restorecon` restores labels according to the applicable file-context policy. It does not invent a special label for a new application.

## Verification

Explain:

- What information `ls -Z` displays.
- The difference between a current context and an expected context.
- Why changing standard Linux permissions does not necessarily resolve an SELinux denial.

## Cleanup

```bash
sudo rm -rf /srv/selinux-lab
```

Only run the cleanup command if you created this lab directory and it contains no files you need.

## Safety

Do not experiment by changing labels on system files such as `/etc/passwd`, SSH configuration, or files belonging to installed applications.
