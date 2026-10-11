# Lab 02 — Group Management

## Objective

Create groups, assign users to groups, inspect group membership, and remove lab resources.

## Environment

- OS: RHEL
- Access: Local terminal or SSH
- Privileges: `sudo` access

## Tasks

### Task 1: Create Users and a Group

```bash
sudo groupadd labteam
sudo useradd -m labuser1
sudo useradd -m labuser2
```

Verify the group:

```bash
getent group labteam
```

### Task 2: Add Users to the Group

```bash
sudo usermod -aG labteam labuser1
sudo usermod -aG labteam labuser2
```

The `-aG` options append the supplementary group without replacing the user's existing supplementary group memberships.

### Task 3: Verify Group Membership

```bash
id labuser1
id labuser2
groups labuser1
getent group labteam
```

**Expected result:** Both users appear as members of `labteam`.

### Task 4: Change a User's Primary Group

Create another group:

```bash
sudo groupadd labprimary
```

Change `labuser1`'s primary group:

```bash
sudo usermod -g labprimary labuser1
```

Verify:

```bash
id labuser1
```

The primary GID should now correspond to `labprimary`. The supplementary membership in `labteam` should remain.

### Task 5: Remove a Supplementary Group Membership

```bash
sudo gpasswd -d labuser2 labteam
```

Verify:

```bash
id labuser2
getent group labteam
```

`labuser2` should no longer be a member of `labteam`.

## Verification Checklist

- Created a group and two users.
- Added both users as supplementary members.
- Verified membership using `id` and `getent`.
- Changed one user's primary group.
- Removed a supplementary group membership.

## Cleanup

Remove the lab users and groups after verifying the results:

```bash
sudo userdel -r labuser1
sudo userdel -r labuser2
sudo groupdel labteam
sudo groupdel labprimary
```

If a group cannot be deleted because it is still the primary group of another account, inspect that account before proceeding.

These commands are intended only for the dedicated accounts and groups created in this lab.

## Key Takeaway

A user's primary group and supplementary groups serve different purposes. Use `id` to verify the effective account and group configuration.
