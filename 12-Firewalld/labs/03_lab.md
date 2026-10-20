# Lab 03: Rich Rules and Troubleshooting

## Objective

Practice source-address-based firewall rules, inspect listening services, and troubleshoot firewall configuration.

## Prerequisites

- Firewalld is running.
- You have `sudo` privileges.
- You understand zones, services, and ports.
- Use a VM console or recovery access for configuration troubleshooting.

## Tasks

### 1. Inspect listening sockets

```bash
sudo ss -tulpn
```

Identify any listening TCP and UDP ports. Record which applications own the sockets, where process details are available.

### 2. Add a temporary rich rule

The following example allows HTTP traffic from the documentation-only address `192.0.2.10` through the public zone:

```bash
sudo firewall-cmd --zone=public \
  --add-rich-rule='rule family="ipv4" source address="192.0.2.10" service name="http" accept'
```

Check whether the rule was added:

```bash
sudo firewall-cmd --zone=public --list-rich-rules
```

**Important:** `192.0.2.10` is a documentation address, not your actual client IP. Replace it with a suitable test-client address only in a controlled lab where you know the source address and understand the access implications.

### 3. Remove the rich rule

Use the exact same rule to remove it:

```bash
sudo firewall-cmd --zone=public \
  --remove-rich-rule='rule family="ipv4" source address="192.0.2.10" service name="http" accept'
```

Verify cleanup:

```bash
sudo firewall-cmd --zone=public --list-rich-rules
```

### 4. Inspect firewalld logs

```bash
sudo journalctl -u firewalld -b
sudo journalctl -u firewalld --since "30 minutes ago"
```

Look for service failures, configuration errors, or reload problems.

### 5. Troubleshoot a connectivity problem

Suppose an HTTP server is unreachable. Check each layer:

```bash
sudo systemctl status firewalld
sudo firewall-cmd --get-active-zones
sudo firewall-cmd --zone=public --list-all
sudo ss -tulpn
```

Then verify:

- The application is running.
- The application is listening on the expected address and port.
- The port or service is allowed in the zone associated with the interface.
- The client is connecting to the correct IP and port.
- Other network or virtualization rules are not blocking traffic.

### 6. Optional challenge: permanent rich rule

Only if you have a known test-client IP and a controlled test environment, add a suitable rich rule permanently, verify it, and then remove it from both permanent and runtime configuration.

## Verification

Explain:

- How a rich rule differs from a simple service or port rule.
- Why a firewall rule alone cannot fix an application that is not listening.
- How to use `journalctl` to investigate firewalld issues.

## Cleanup

Confirm that the temporary rich rule is removed and that your original firewall configuration remains intact.

## Safety Note

Do not disable firewalld as a troubleshooting shortcut, and do not experiment with masquerading or port forwarding on a production network. Those features require additional routing and network configuration.
