# Lab 02: Package Updates, Removal, and Transaction History

## Objective

Practice updating packages, inspecting DNF transactions, and removing a test package safely.

## Tasks

### 1. Check package information

```bash
dnf info httpd
rpm -q httpd
```

### 2. Check available updates

```bash
sudo dnf check-update
```

Note that this command can return exit status `100` when updates are available. That does not necessarily mean an error occurred.

### 3. Update a specific package

```bash
sudo dnf upgrade httpd
```

Review the transaction before accepting it. If no update is available, record that result instead of forcing an update.

### 4. Review transaction history

```bash
dnf history
```

Identify the transaction corresponding to your installation or update.

Inspect it using its transaction ID:

```bash
dnf history info TRANSACTION_ID
```

Replace `TRANSACTION_ID` with the actual ID.

### 5. Remove the test package

If you installed `httpd` specifically for Lab 1 and do not need it, inspect the proposed removal:

```bash
sudo dnf remove httpd
```

Review all packages DNF proposes to remove before confirming. If it proposes removing something unexpected, cancel the transaction.

### 6. Verify removal

```bash
rpm -q httpd
dnf list installed httpd
```

An absent-package result is expected if removal succeeded.

## Verification Checklist

- Checked package information.
- Checked for available updates.
- Understood the update command's result.
- Inspected DNF transaction history.
- Reviewed dependencies before removal.
- Verified the package's final state.

## Expected Outcome

You can update and remove packages safely and use DNF history to investigate previous transactions.
