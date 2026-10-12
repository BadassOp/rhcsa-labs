# Permissions and ACLs — Mental Model

## 1. Basic Permission Structure

```text
-rwxr-xr--
```

In this example:

- Owner: `rwx` — read, write, execute.
- Group: `r-x` — read and execute.
- Others: `r--` — read only.

The first character "-" identifies the file type; it is not a permission bit.

## 2. Numeric Permissions

Each permission has a numeric value:

```text
r = 4
w = 2
x = 1
```

Add the values for each class.

```text
7 = 4 + 2 + 1 = rwx
6 = 4 + 2     = rw-
5 = 4     + 1 = r-x
4 = 4         = r--
```

Example:

```text
chmod 754 file.txt

Owner   = 7 = rwx
Group   = 5 = r-x
Others  = 4 = r--
```

## 3. How Access Is Evaluated

```text
Process Requests Access
          |
          v
Identify Process Credentials
          |
          v
Evaluate Owner / Group / Other
          |
          v
Consider ACL Entries and ACL Mask
          |
          v
Apply Other Access Controls
          |
          v
Access Allowed or Denied
```

This is a conceptual model. Actual access decisions depend on the applicable credentials, filesystem permissions and ACLs, path traversal, and other controls such as SELinux.

## 4. Ownership and Groups

```text
File
 |
 ├── Owner UID
 |
 └── Group GID
          |
          v
      Group Members
```

Ownership identifies the file's owner and owning group. Permission bits and any ACL entries then influence which operations are allowed.

## 5. How ACLs Extend Permissions

Traditional permissions provide three classes:

```text
Owner | Group | Others
```

ACLs can provide additional entries:

```text
Owner
Named User: alice
Owning Group
Named Group: developers
Others
```

The ACL mask limits the effective permissions of named users, named groups, and the owning group entry. Always inspect `getfacl` output to distinguish configured permissions from effective permissions.

## 6. Special Permission Bits

- **SUID:** On executable files, causes execution with the file owner's effective UID where supported.
- **SGID:** On executable files, affects the effective group during execution. On directories, it commonly causes new entries to inherit the directory's group.
- **Sticky bit:** On directories, restricts deletion or renaming of entries, commonly to the entry owner, directory owner, or a privileged process.

## Key Idea

Ownership identifies the owner and group. Permission bits establish basic access, ACLs provide finer-grained rules, and other security controls may further restrict access. When access fails, inspect all relevant layers instead of blindly changing permissions.
