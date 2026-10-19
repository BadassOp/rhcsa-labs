# Swap Management — Commands Cheat Sheet

## 1. Inspect Memory and Swap

```bash
free -h
swapon --show
cat /proc/swaps
grep -E 'MemTotal|MemAvailable|SwapTotal|SwapFree' /proc/meminfo
```

- `free -h`: Shows memory and swap totals and usage.
- `swapon --show`: Lists active swap areas.
- `/proc/swaps`: Shows currently enabled swap devices and files.

## 2. Create a Swap File

First check available disk space:

```bash
df -h /
```

Create a 512 MiB swap file using a suitable method for your filesystem:

```bash
sudo dd if=/dev/zero of=/swapfile bs=1M count=512 status=progress
sudo chmod 600 /swapfile
sudo mkswap /swapfile
```

`dd` writes data to the specified destination. Confirm the destination is correct and does not already contain data you need.

## 3. Activate and Verify Swap

```bash
sudo swapon /swapfile
swapon --show
free -h
```

## 4. Deactivate Swap

```bash
sudo swapoff /swapfile
swapon --show
```

Do not deactivate essential existing swap areas if the system is under memory pressure. Ensure sufficient memory is available before disabling swap.

## 5. Configure Persistent Swap

Back up `/etc/fstab`:

```bash
sudo cp -a /etc/fstab /etc/fstab.backup
```

Add the following entry if using `/swapfile`:

```fstab
/swapfile none swap defaults 0 0
```

Validate and activate configured swap areas:

```bash
sudo findmnt --verify
sudo swapon -a
swapon --show
```

Do not add a duplicate entry if the swap file is already configured.

## 6. Set Swap Priority

Swap priority can be specified in `/etc/fstab` using the `pri` option.

Example:

```fstab
/swapfile none swap defaults,pri=10 0 0
```

Higher priority swap areas are preferred over lower priority ones. If multiple swap areas have equal priority, Linux may distribute usage among them.

## 7. Troubleshooting

Inspect swap configuration:

```bash
cat /etc/fstab
swapon --show
sudo journalctl -b | grep -i swap
```

Check free space:

```bash
df -h /
```

Check swap file permissions:

```bash
ls -l /swapfile
```

A swap file should be appropriately protected, typically with permissions `600`.

## Safety Notes

- Never overwrite an existing file unintentionally.
- Do not deactivate swap indiscriminately on a memory-constrained system.
- Validate `/etc/fstab` before rebooting.
- Remove only the swap entry you added when cleaning up.
- Do not delete a swap file while it is active.
