# Systemd and Services — Commands Cheat Sheet

## 1. Check Systemd

```bash
systemctl --version
systemctl is-system-running
systemctl list-units
systemctl list-unit-files
```

## 2. Inspect Services

```bash
systemctl status sshd
systemctl is-active sshd
systemctl is-enabled sshd
systemctl --failed
systemctl list-units --type=service
systemctl list-unit-files --type=service
```

- `status`: Shows service state and recent log messages.
- `is-active`: Checks whether a unit is active.
- `is-enabled`: Checks whether a unit is enabled at boot.
- `--failed`: Lists failed units.

## 3. Control Services

```bash
sudo systemctl start sshd
sudo systemctl stop sshd
sudo systemctl restart sshd
sudo systemctl reload sshd
sudo systemctl try-restart sshd
```

`reload` requests that a service reload its configuration, if supported. `restart` stops and starts the service. `try-restart` restarts it only if it is already running.

## 4. Configure Boot-Time Behavior

```bash
sudo systemctl enable sshd
sudo systemctl disable sshd
sudo systemctl enable --now sshd
sudo systemctl disable --now sshd
```

- `enable`: Configures automatic startup; it does not necessarily start the service immediately.
- `disable`: Removes automatic-start configuration; it does not necessarily stop a running service.
- `--now`: Also starts or stops the service immediately.

**Caution:** Do not disable or stop `sshd` if you depend on it for remote access unless you have console access or another recovery method.

## 5. Mask and Unmask Services

```bash
sudo systemctl mask SERVICE
sudo systemctl unmask SERVICE
```

Masking prevents a unit from being started through normal systemd activation. Masking a critical service can disrupt the system; practice with a nonessential service.

## 6. Inspect Unit Files and Dependencies

```bash
systemctl cat sshd
systemctl show sshd
systemctl list-dependencies sshd
systemctl list-dependencies --reverse sshd
```

To locate unit files:

```bash
systemctl show -p FragmentPath sshd
```

Common unit types include:

- `.service`: Services
- `.socket`: Socket activation
- `.target`: Groups of units
- `.mount`: Mount points
- `.timer`: Scheduled activation

## 7. Reload Systemd Configuration

After adding or modifying unit files:

```bash
sudo systemctl daemon-reload
```

This makes systemd reread unit definitions. It does not automatically restart affected services.

## 8. Troubleshoot Failed Services

```bash
systemctl --failed
systemctl status SERVICE
journalctl -u SERVICE
journalctl -u SERVICE -b
journalctl -xeu SERVICE
```

Replace `SERVICE` with the actual unit name.

## 9. Inspect Default Target

```bash
systemctl get-default
systemctl list-units --type=target
```

To change the default target, use `systemctl set-default TARGET` only when you understand the effect on the system's normal boot mode.
