# SSH Commands

## 1. Connect to a Remote System

```bash
ssh user@192.168.1.100
ssh -p 2222 user@192.168.1.100
```

- `user@host`: Specifies the remote username and hostname or IP address.
- `-p`: Specifies a non-default SSH port.

Replace the example address and username with your actual lab system details.

## 2. Check SSH Server Status

On RHEL:

```bash
sudo systemctl status sshd
sudo systemctl enable --now sshd
```

The SSH server package may need to be installed first:

```bash
sudo dnf install openssh-server
```

## 3. Inspect Listening Ports

```bash
sudo ss -tlnp | grep ':22'
```

This checks for a TCP listener on port 22. If the server uses a different port, adjust the command.

## 4. Generate an SSH Key Pair

```bash
ssh-keygen -t ed25519
```

The private key is typically stored at `~/.ssh/id_ed25519`, and the public key at `~/.ssh/id_ed25519.pub`.

Use a passphrase where appropriate. Never share the private key.

## 5. Configure Public-Key Authentication

If available, copy the public key to the remote account:

```bash
ssh-copy-id user@192.168.1.100
```

Alternatively, append the contents of the public key file to the remote user's `~/.ssh/authorized_keys` using a trusted method.

Typical permissions:

```bash
chmod 700 ~/.ssh
chmod 600 ~/.ssh/authorized_keys
chmod 600 ~/.ssh/id_ed25519
chmod 644 ~/.ssh/id_ed25519.pub
```

The remote account must own its `.ssh` directory and `authorized_keys` file, and the server must permit public-key authentication.

## 6. Copy Files with SCP

Copy a local file to a remote host:

```bash
scp file.txt user@192.168.1.100:/tmp/
```

Copy a remote file to the local directory:

```bash
scp user@192.168.1.100:/tmp/file.txt .
```

Use `-P` (uppercase) to specify a non-default port:

```bash
scp -P 2222 file.txt user@192.168.1.100:/tmp/
```

## 7. Use SFTP

```bash
sftp user@192.168.1.100
```

Useful interactive commands:

```text
ls
pwd
lpwd
get remote-file.txt
put local-file.txt
exit
```

## 8. Inspect SSH Configuration

Client configuration:

```bash
cat /etc/ssh/ssh_config
cat ~/.ssh/config
```

Server configuration:

```bash
sudo less /etc/ssh/sshd_config
sudo sshd -t
```

`sshd -t` checks the server configuration for syntax errors.

To inspect effective settings:

```bash
sudo sshd -T
```

## 9. Apply Server Configuration Changes

After editing the server configuration:

```bash
sudo sshd -t
sudo systemctl reload sshd
sudo systemctl status sshd
```

If reload is unsupported or a restart is required for a particular change, use the appropriate service action after validating the configuration.

Keep an existing session or console access available until the new connection is confirmed.

## 10. Troubleshoot SSH

Client-side verbose output:

```bash
ssh -v user@192.168.1.100
```

Server-side logs:

```bash
sudo journalctl -u sshd -b
sudo journalctl -u sshd --since "15 minutes ago"
```

Check firewall configuration:

```bash
sudo firewall-cmd --get-active-zones
sudo firewall-cmd --list-services
```

If SSH is not allowed in the relevant zone and the service is listening on the standard port, you may need to allow it:

```bash
sudo firewall-cmd --permanent --add-service=ssh
sudo firewall-cmd --reload
```

Only make firewall changes appropriate for your active zone and network environment.

## Important

Never disable an authentication method or change the SSH port without confirming that you can still access the server. Always validate configuration before reloading or restarting `sshd`.
