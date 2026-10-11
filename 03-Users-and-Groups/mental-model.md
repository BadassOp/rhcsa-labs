# Users and Groups — Mental Model

## 1. How Linux Identifies Users

Linux uses numeric IDs internally to identify accounts.

```text
Username
   |
   v
UID (User ID)
   |
   v
Linux identifies the user
```

Example:

```text
username: student
UID:      1001
```

The numeric ID is the identity used by the system; the username is its human-readable representation.

## 2. How Groups Work

Groups allow multiple users to share a common group identity.

```text
                 Group: developers
                  GID: 2001
                      |
             ┌────────┼────────┐
             |        |        |
           User A   User B   User C
```

A user has one primary group and may belong to multiple supplementary groups.

## 3. Account Information Flow

```text
/etc/passwd
    |
    └── Username, UID, GID,
        home directory, shell

/etc/shadow
    |
    └── Password hash and
        password-aging information

/etc/group
    |
    └── Group names, GIDs,
        supplementary memberships

/etc/gshadow
    |
    └── Protected group information
```

These files provide local account information. Systems using centralized identity services may obtain some account information from other sources.

## 4. User Management Lifecycle

```text
Create User
    |
    v
Set Password
    |
    v
Configure Groups
    |
    v
Verify Account
    |
    v
Modify or Lock Account
    |
    v
Delete Account (when appropriate)
```

## 5. Connection to File Permissions

Linux file permissions use numeric user and group identities to determine access.

```text
User Identity (UID)
        |
        v
File Ownership

Group Identity (GID)
        |
        v
Group Ownership
        |
        v
Permission Evaluation
```

Creating a user or adding them to a group does not automatically grant unrestricted access. The relevant file permissions and other access controls still apply.

## Key Idea

Users identify accounts, groups organize membership, and permissions determine what those identities can access.
