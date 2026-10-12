# Lab 03 — Access Control Lists (ACLs)

## Objective

Grant specific users and groups access to files and directories using ACLs, inspect effective permissions, and configure default ACLs.

## Environment

- OS: RHEL
- Access: Local terminal or SSH
- Privileges: Regular user and `sudo` access

## Tasks

### Task 1: Create Test Users and Files

```bash
sudo useradd -m acluser
sudo useradd -m acluser2

mkdir -p ~/acl-lab
echo "Confidential lab data" > ~/acl-lab/report.txt

chmod 640 ~/acl-lab/report.txt
```

### Task 2: Grant a Named User Access

Grant read permission to `acluser`:

```bash
setfacl -m u:acluser:r-- ~/acl-lab/report.txt
```

Inspect the ACL:

```bash
getfacl ~/acl-lab/report.txt
```

The ACL should contain an entry for `acluser`.

### Task 3: Test Access

Check that the named user can read the file:

```bash
sudo -u acluser cat ~/acl-lab/report.txt
```

If access is denied, inspect the permissions on the parent directories as well as the file ACL. The user needs execute permission on the directories in the path to reach the file.

### Task 4: Modify and Remove an ACL Entry

Grant read and write permissions:

```bash
setfacl -m u:acluser:rw- ~/acl-lab/report.txt
getfacl ~/acl-lab/report.txt
```

Remove the named user's ACL entry:

```bash
setfacl -x u:acluser ~/acl-lab/report.txt
getfacl ~/acl-lab/report.txt
```

### Task 5: Configure a Default ACL

Create a shared directory:

```bash
mkdir ~/acl-lab/shared
```

Set a default ACL:

```bash
setfacl -m d:u:acluser:rwx ~/acl-lab/shared
```

Create a file and a subdirectory:

```bash
touch ~/acl-lab/shared/newfile.txt
mkdir ~/acl-lab/shared/newdir
```

Inspect their ACLs:

```bash
getfacl ~/acl-lab/shared
getfacl ~/acl-lab/shared/newfile.txt
getfacl ~/acl-lab/shared/newdir
```

**Expected result:** The new file and directory inherit ACL information from the parent directory, subject to the permissions requested at creation.

A default ACL does not automatically grant execute permission on regular files created without it, because file-creation permissions normally exclude execution.

### Task 6: Remove Extended ACL Entries

Remove extended ACL entries from the test file:

```bash
setfacl -b ~/acl-lab/report.txt
getfacl ~/acl-lab/report.txt
```

Remove default ACLs from the shared directory:

```bash
setfacl -k ~/acl-lab/shared
getfacl ~/acl-lab/shared
```

## Verification Checklist

- Granted a named user read access.
- Inspected ACL entries with `getfacl`.
- Modified and removed an ACL entry.
- Configured a default ACL.
- Verified ACL inheritance on newly created objects.
- Removed extended and default ACLs.

## Cleanup

Inspect the test directory:

```bash
find ~/acl-lab -maxdepth 3 -print
```

Then remove it:

```bash
rm -r ~/acl-lab
```

Remove the test users:

```bash
sudo userdel -r acluser
sudo userdel -r acluser2
```

Only remove the dedicated test resources created by this lab.

## Key Takeaway

ACLs provide finer-grained access control than the traditional owner/group/others permission model. Always inspect effective permissions and parent-directory traversal permissions when troubleshooting access failures.
