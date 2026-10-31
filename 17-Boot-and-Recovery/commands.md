# Boot Troubleshooting and Recovery Commands

## 1. Inspect the Default Target

```bash
systemctl get-default
systemctl list-units --type=target
```

Common targets include:

- `multi-user.target`: Non-graphical multi-user environment.
- `graphical.target`: Multi-user environment with graphical login services.
- `rescue.target`: Single-user-style recovery environment.
- `emergency.target`: Minimal recovery environment.

## 2. Inspect Failed Services

```bash
systemctl --failed
systemctl status SERVICE_NAME
journalctl -b -p err
```

Replace `SERVICE_NAME` with the affected service.

Inspect logs from the previous boot, if retained:

```bash
journalctl -b -1
```

## 3. Inspect Kernel and Boot Information

```bash
uname -r
cat /proc/cmdline
sudo grubby --default-kernel
sudo grubby --info=ALL
```

- `uname -r`: Shows the running kernel version.
- `/proc/cmdline`: Shows parameters used by the currently running kernel.
- `grubby`: Inspects and manages kernel boot entries on supported RHEL systems.

## 4. Validate Filesystem Mount Configuration

```bash
findmnt --verify
cat /etc/fstab
lsblk -f
```

Check UUIDs, filesystem types, mount points, and options before rebooting.

Back up the file before editing it:

```bash
sudo cp -a /etc/fstab /etc/fstab.bak
```

After editing, validate again:

```bash
sudo findmnt --verify --verbose
```

A successful validation does not guarantee that every filesystem will mount successfully at boot. Confirm that referenced devices and filesystems actually exist.

## 5. Inspect the Boot Journal

```bash
journalctl -b
journalctl -b -p warning
journalctl -b -p err
journalctl --list-boots
```

Use the boot ID and timestamps to correlate service failures with startup events.

## 6. Inspect GRUB Configuration

```bash
sudo grubby --default-kernel
sudo grubby --info=ALL
```

RHEL commonly uses GRUB2 with BLS boot entries. Avoid manually editing generated GRUB configuration files unless the documented procedure for your installation requires it.

## 7. Understand Emergency and Rescue Targets

Inspect the target definitions:

```bash
systemctl cat rescue.target
systemctl cat emergency.target
```

These commands inspect the target units. They do not enter recovery mode.

Switching to a recovery target changes the system's operating state and can interrupt services. Perform such exercises only from a VM console with a recovery plan.

## 8. Rebuild initramfs — Advanced Recovery

The `dracut` utility manages initramfs images on RHEL.

Inspect available images:

```bash
ls -lh /boot/initramfs-*.img
```

A rebuild may be required if an initramfs image is damaged or missing necessary components. The exact recovery command depends on the installed kernel and system state.

Do not rebuild or overwrite boot images as a routine troubleshooting step.

## Important

Always confirm the boot entry, kernel version, filesystem configuration, and recovery access before making boot-related changes.
