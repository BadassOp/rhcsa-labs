# Lab 03: SSH Troubleshooting

## Objective

Diagnose SSH connection failures by checking network connectivity, listening ports, service status, firewall rules, authentication, and logs.

## Prerequisites

- RHEL VM with SSH configured.
- Client access to attempt a connection.
- Sudo privileges on the server.

## Tasks

### 1. Check the SSH service

On the server:

```bash
sudo systemctl status sshd
sudo journalctl -u sshd -b
```

If the service is stopped and you intend to use SSH:

```bash
sudo systemctl start sshd
```

### 2. Check the listening socket

```bash
sudo ss -tlnp | grep ':22'
```

If nothing appears, check the service status and SSH server configuration. The server may be configured to use a different port.

### 3. Test the network path

From the client:

```bash
ping SERVER_IP
```

A failed ping does not necessarily mean the host is unreachable because ICMP traffic may be blocked.

Test the SSH port if `nc` is installed:

```bash
nc -vz SERVER_IP 22
```

### 4. Check the firewall

On the server:

```bash
sudo firewall-cmd --get-active-zones
sudo firewall-cmd --zone=public --list-all
```

Use the zone actually assigned to the server's network interface. Confirm that the SSH service or configured SSH port is allowed.

### 5. Validate SSH configuration

```bash
sudo sshd -t
```

No output usually means no syntax errors were detected.

Inspect effective settings:

```bash
sudo sshd -T | grep -E '^(port|permitrootlogin|pubkeyauthentication|passwordauthentication)'
```

Effective settings may also depend on configuration snippets, match rules, and the client account.

### 6. Diagnose authentication failures

On the client:

```bash
ssh -v username@SERVER_IP
```

On the server:

```bash
sudo journalctl -u sshd --since "15 minutes ago"
```

Check:

- Username and destination IP.
- Whether the correct private key is being offered.
- Whether the corresponding public key is in the correct account's `authorized_keys`.
- Ownership and permissions of the account's SSH files.
- Whether the server permits the selected authentication method.

### 7. Interpret common errors

| Error                           | Possible causes                                                        |
| ------------------------------- | ---------------------------------------------------------------------- |
| `Connection refused`            | No listener on the target port, or the connection is actively rejected |
| `Connection timed out`          | Network path, firewall, routing, or host availability problem          |
| `Permission denied (publickey)` | Key mismatch, incorrect account, file permissions, or server policy    |
| `Host key verification failed`  | Server identity mismatch or changed host key                           |
| `Could not resolve hostname`    | Incorrect hostname or DNS/name-resolution issue                        |

These are starting points, not definitive diagnoses.

## Verification

Choose one simulated failure, such as using an incorrect username or port, and identify the resulting error. Explain which layer failed and which command helped diagnose it.

Do not deliberately break the server's working SSH configuration.

## Cleanup

Restore any temporary test settings and confirm that your original SSH login still works.

## Safety

Never blindly delete entries from `~/.ssh/known_hosts` to suppress a host-key warning. Verify the server's identity first. Keep an active working session or console access while troubleshooting.
