# Lab 02: Persistent Swap Configuration

## Objective

Configure swap to activate automatically using `/etc/fstab` and verify the configuration.

## Tasks

### 1. Verify the swap file

```bash
swapon --show
sudo blkid /swapfile
```

A swap file is not a normal filesystem mount, so you do not need a filesystem UUID to configure it in `/etc/fstab`.

### 2. Back up `/etc/fstab`

```bash
sudo cp -a /etc/fstab /etc/fstab.backup
```

### 3. Inspect existing configuration

```bash
cat /etc/fstab
```

Check whether an entry for `/swapfile` already exists. Do not add a duplicate entry.

### 4. Add the persistent entry

Edit the file:

```bash
sudo vi /etc/fstab
```

Add this line if it does not already exist:

```fstab
/swapfile none swap defaults 0 0
```

### 5. Validate configuration

```bash
sudo findmnt --verify
```

Read the output carefully. Resolve any relevant errors before continuing.

### 6. Test activation through configuration

Deactivate only the swap file created for this lab:

```bash
sudo swapoff /swapfile
```

Then activate swap areas listed in `/etc/fstab`:

```bash
sudo swapon -a
swapon --show
free -h
```

Confirm that `/swapfile` is active again.

**Safety:** Do not disable other swap areas. Ensure sufficient RAM is available before running `swapoff`.

### 7. Document the result

Record:

- The persistent entry you added.
- The validation output.
- The result of `swapon -a`.
- How you confirmed the swap file was active.

## Verification Checklist

- Backed up `/etc/fstab`.
- Added the correct swap entry.
- Checked for duplicate entries.
- Validated the configuration.
- Reactivated the swap file using `swapon -a`.
- Verified the active swap area.

## Expected Outcome

You can configure swap to activate automatically through `/etc/fstab`.
