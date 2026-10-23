# Lab 02: SSH Key-Based Authentication

## Objective

Generate an SSH key pair, configure public-key authentication, and verify access to a remote RHEL account.

## Prerequisites

- SSH server running on the destination.
- A client and server account that you control.
- Working password-based access or another tested login method.
- The `ssh-keygen` utility.

## Tasks

### 1. Generate an Ed25519 key pair

On the client:

```bash
ssh-keygen -t ed25519
```

Use the default file location for this lab, unless you already have a key there. Set a passphrase when appropriate.

Inspect the files:

```bash
ls -l ~/.ssh/id_ed25519*
```

Never share or copy the private key (`id_ed25519`) to the server.

### 2. Install the public key

If `ssh-copy-id` is available:

```bash
ssh-copy-id username@SERVER_IP
```

Alternatively, securely append the contents of `~/.ssh/id_ed25519.pub` to the remote user's `~/.ssh/authorized_keys` file.

### 3. Check permissions on the server

Log in to the remote account and check:

```bash
ls -ld ~/.ssh
ls -l ~/.ssh/authorized_keys
```

Typical permissions are:

```bash
chmod 700 ~/.ssh
chmod 600 ~/.ssh/authorized_keys
```

The directory and file must also have appropriate ownership by the remote user.

### 4. Test key authentication

From the client:

```bash
ssh -o PreferredAuthentications=publickey \
    -o PasswordAuthentication=no \
    username@SERVER_IP
```

This tests public-key authentication without falling back to password authentication for this connection.

If it fails, continue using your original working login method while troubleshooting.

### 5. Inspect authentication details

```bash
ssh -v username@SERVER_IP
```

Look for messages indicating which keys were offered and whether authentication succeeded.

### 6. Understand the key files

- `id_ed25519`: private key; keep it secret.
- `id_ed25519.pub`: public key; may be installed on the server.
- `authorized_keys`: public keys accepted for that account.

## Verification

Explain:

- Why the private key must remain on the client.
- How public-key authentication differs from password authentication.
- Why correct file permissions and ownership matter.

## Cleanup

Remove only the public key you added for this lab if you no longer need it. Keep any existing keys and access methods intact.

## Safety

Do **not** disable password authentication globally as part of this lab. First confirm that key-based login works in a separate session and that you have console access.
