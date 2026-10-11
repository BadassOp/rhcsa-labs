# Lab 03 — Password and Account Management

## Objective

Practice password changes, account locking and unlocking, password-aging configuration, and account verification.

## Environment

- OS: RHEL
- Access: Local terminal or SSH
- Privileges: `sudo` access

## Tasks

### Task 1: Create a Dedicated Lab Account

```bash
sudo useradd -m accountlab
sudo passwd accountlab
```

Use a lab-only password that meets your system's password policy.

Inspect password status:

```bash
sudo passwd -S accountlab
sudo chage -l accountlab
```

### Task 2: Change Password-Aging Settings

Set a maximum password age of 90 days:

```bash
sudo chage -M 90 accountlab
```

Set a warning period of 7 days:

```bash
sudo chage -W 7 accountlab
```

Verify:

```bash
sudo chage -l accountlab
```

**Expected result:** The maximum password age is 90 days and the warning period is 7 days.

These settings are for demonstration. Production password policies should follow organizational requirements.

### Task 3: Lock the Account Password

```bash
sudo usermod -L accountlab
sudo passwd -S accountlab
```

The password status should indicate that the password is locked.

Locking a password does not necessarily disable every possible login method. SSH keys or other authentication mechanisms may behave differently depending on system configuration.

### Task 4: Unlock the Account Password

```bash
sudo usermod -U accountlab
sudo passwd -S accountlab
```

Verify that the password is no longer locked.

Unlocking does not remove password-aging settings or guarantee that the account can log in by every method.

### Task 5: Set an Account Expiration Date

Set a temporary expiration date:

```bash
sudo chage -E 2027-01-31 accountlab
```

Inspect the account:

```bash
sudo chage -l accountlab
```

The account expiration date should be displayed.

To remove the expiration date afterward:

```bash
sudo chage -E -1 accountlab
```

### Task 6: Verify and Clean Up

Verify the account:

```bash
id accountlab
sudo passwd -S accountlab
sudo chage -l accountlab
```

Remove the lab account:

```bash
sudo userdel -r accountlab
```

## Verification Checklist

- Created a dedicated lab account.
- Inspected password status and aging.
- Configured password-aging values.
- Locked and unlocked the account password.
- Set and removed an account expiration date.
- Removed the lab account.

## Key Takeaway

Password expiration, account expiration, and password locking are different controls. Always verify the resulting account state rather than assuming a command succeeded.
