# Lab 01: SSH Server and Firewall

## Objective

Install and manage OpenSSH Server, verify that it is listening, and configure firewalld to permit SSH connections.

## Prerequisites

- RHEL VM with sudo access.
- A second VM or SSH client for testing remote access.
- Console access to the server in case of network problems.

## Tasks

### 1. Install OpenSSH Server

```bash
sudo dnf install openssh-server
```

Check the service:

```bash
sudo systemctl status sshd
```

### 2. Enable and start SSH

```bash
sudo systemctl enable --now sshd
sudo systemctl is-enabled sshd
sudo systemctl is-active sshd
```

### 3. Verify the listening port

```bash
sudo ss -tlnp | grep ':22'
```

Confirm that `sshd` is listening on TCP port 22.

### 4. Inspect firewalld

```bash
sudo firewall-cmd --state
sudo firewall-cmd --get-active-zones
sudo firewall-cmd --list-services
```

Identify the zone associated with the server's active network interface.

### 5. Allow SSH through the appropriate zone

If SSH is not already allowed in the relevant zone, add it using a permanent rule. Replace `public` if your active zone is different.

```bash
sudo firewall-cmd --zone=public --add-service=ssh --permanent
sudo firewall-cmd --reload
sudo firewall-cmd --zone=public --query-service=ssh
```

Do not modify unrelated zones unnecessarily.

### 6. Connect from another machine

Find the server IP:

```bash
ip -br address
```

From the client, connect using the server's actual IP:

```bash
ssh username@SERVER_IP
```

Accept the host key only after verifying the server identity through a trusted method.

### 7. Verify the session

After logging in:

```bash
whoami
hostname
who
```

## Verification

Explain:

- Why the service is named `sshd` rather than `ssh`.
- Why the service and firewall must both be configured.
- Why a listening port does not guarantee remote connectivity.

## Cleanup

If you added a firewall rule solely for this lab, remove it after testing if SSH access is no longer required. Do not remove a pre-existing rule needed for administration.

## Safety

Keep console access available. Never remove SSH access from the zone carrying your active management connection without a tested alternative.
