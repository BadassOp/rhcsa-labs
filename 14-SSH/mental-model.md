# SSH — Mental Model

## 1. How SSH Works

SSH establishes an encrypted connection between a client and a server.

```text
SSH Client
    |
    |  Network connection
    v
SSH Server (sshd)
    |
    |  Authentication
    v
Remote User Session
```

The client verifies the server's host identity, negotiates encryption, and authenticates the user before granting access.

## 2. Client vs. Server

| Component     | Purpose                                    |
| ------------- | ------------------------------------------ |
| `ssh`         | Connects to a remote host                  |
| `sshd`        | Accepts incoming SSH connections           |
| `ssh-keygen`  | Generates SSH key pairs                    |
| `ssh-copy-id` | Installs a public key for a remote account |
| `scp`         | Copies files over SSH                      |
| `sftp`        | Transfers files over SSH                   |

A system can run an SSH client without running an SSH server.

## 3. Three Different Keys or Identities

### User key pair

- **Private key:** Kept secret by the user.
- **Public key:** Installed on the remote account when using public-key authentication.

The server uses the authorized public key to verify that the client possesses the corresponding private key.

### Server host key

The server's host key helps the client identify the server. The client records or otherwise verifies the server's identity to detect unexpected changes.

A host-key warning should be investigated, not blindly bypassed.

### Password

Password authentication is a separate authentication method. SSH encrypts the connection, but password authentication and public-key authentication work differently.

## 4. Authentication Flow

With public-key authentication:

1. The client connects to the SSH server.
2. The client verifies the server's host identity.
3. The server checks whether the account permits the requested authentication method.
4. The client proves possession of the corresponding private key.
5. The server verifies the proof using the authorized public key.
6. If authentication and access checks succeed, a session is opened.

The private key is not sent to the server.

## 5. Important Files and Directories

| Path                     | Purpose                              |
| ------------------------ | ------------------------------------ |
| `/etc/ssh/sshd_config`   | SSH server configuration             |
| `/etc/ssh/ssh_config`    | System-wide SSH client configuration |
| `~/.ssh/config`          | Per-user client configuration        |
| `~/.ssh/id_ed25519`      | Example private key                  |
| `~/.ssh/id_ed25519.pub`  | Corresponding public key             |
| `~/.ssh/authorized_keys` | Public keys allowed for an account   |
| `/etc/ssh/ssh_host_*`    | SSH server host-key files            |

Exact filenames and enabled configuration options may vary by system.

## 6. SSH and Firewalld

Three conditions commonly need to be satisfied for remote SSH access:

1. `sshd` is running and listening on the intended address and port.
2. The network path and firewall allow the connection.
3. The user can authenticate and is permitted to log in.

Opening a firewall port does not start `sshd`, and running `sshd` does not guarantee that the network permits connections.

## 7. Troubleshooting Model

When SSH fails, investigate in layers:

1. **Network:** Is the destination reachable?
2. **Port:** Is the server listening on the expected port?
3. **Firewall:** Is traffic permitted through the relevant zone and network?
4. **Service:** Is `sshd` running without configuration errors?
5. **Host identity:** Is the server host key expected?
6. **Authentication:** Are the credentials or key pair correct?
7. **Authorization:** Is the account allowed to log in?

Useful tools include `ping`, `ss`, `firewall-cmd`, `systemctl`, `ssh -v`, and `journalctl`.

## Core Principle

SSH access depends on the network path, server configuration, host identity, authentication, and account authorization. Troubleshoot each layer separately instead of changing multiple settings at once.
