# SSH (Secure Shell)

## Overview

SSH is a network protocol used to securely access and administer remote systems. It encrypts communication between the client and server and supports remote shells, file transfers, and key-based authentication.

On RHEL, OpenSSH commonly provides the SSH server (`sshd`) and client (`ssh`).

## Core Concepts

- **SSH client:** Initiates a connection to a remote system.
- **SSH server:** Accepts connections through the `sshd` service.
- **Password authentication:** Authenticates users using passwords, when enabled.
- **Public-key authentication:** Uses a key pair to authenticate without sending the private key to the server.
- **SSH configuration:** Client settings are commonly stored in `/etc/ssh/ssh_config` and `~/.ssh/config`.
- **Server configuration:** Managed through `/etc/ssh/sshd_config` and applicable configuration snippets.
- **Host keys:** Identify the SSH server to clients.
- **Authorized keys:** Public keys permitted to authenticate as a particular user.
- **SSH security:** Includes access restrictions, authentication controls, and secure key management.

## Important Commands

```bash
ssh user@server
ssh-keygen
ssh-copy-id user@server
scp file.txt user@server:/tmp/
sftp user@server
sudo systemctl status sshd
sudo sshd -t
sudo systemctl reload sshd
```

## Learning Objectives

- Connect to a remote Linux system.
- Install and manage the OpenSSH server.
- Understand SSH client and server configuration.
- Generate and use SSH key pairs.
- Configure public-key authentication.
- Transfer files securely with `scp` and `sftp`.
- Troubleshoot connection and authentication failures.
- Apply safe SSH configuration practices.

## Best Practices

- Use SSH keys where appropriate.
- Protect private keys with suitable file permissions and a passphrase.
- Verify server host-key fingerprints when connecting to unfamiliar systems.
- Validate server configuration using `sshd -t` before applying changes.
- Keep an existing session or console access available when changing SSH settings.
- Do not disable password authentication until key-based access has been tested.
- Restrict SSH access according to the server's operational requirements.
