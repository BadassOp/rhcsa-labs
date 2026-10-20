# Lab 02: Managing Services and Ports

## Objective

Allow and remove firewall services and ports, and understand runtime versus permanent rules.

## Prerequisites

- Firewalld is running.
- You have `sudo` privileges.
- You can access the VM console if networking is interrupted.

## Tasks

### 1. Check the current HTTP rule

```bash
sudo firewall-cmd --zone=public --query-service=http
```

A `no` response means HTTP is not currently allowed in that zone.

### 2. Add a temporary HTTP rule

```bash
sudo firewall-cmd --zone=public --add-service=http
sudo firewall-cmd --zone=public --query-service=http
```

Inspect the active configuration:

```bash
sudo firewall-cmd --zone=public --list-services
```

### 3. Remove the temporary rule

```bash
sudo firewall-cmd --zone=public --remove-service=http
sudo firewall-cmd --zone=public --query-service=http
```

Confirm the result. If HTTP was already allowed before the lab, restore the original state rather than assuming it should be absent.

### 4. Add a temporary custom port

```bash
sudo firewall-cmd --zone=public --add-port=8080/tcp
sudo firewall-cmd --zone=public --query-port=8080/tcp
sudo firewall-cmd --zone=public --list-ports
```

This allows traffic to TCP port 8080 through the selected zone. It does **not** start an application or create a listening service.

### 5. Remove the custom port

```bash
sudo firewall-cmd --zone=public --remove-port=8080/tcp
sudo firewall-cmd --zone=public --query-port=8080/tcp
```

### 6. Practice a permanent rule

First check whether HTTP is already permanently allowed:

```bash
sudo firewall-cmd --permanent --zone=public --query-service=http
```

If it was absent, add it permanently:

```bash
sudo firewall-cmd --permanent --zone=public --add-service=http
sudo firewall-cmd --permanent --zone=public --query-service=http
```

Apply permanent changes to the runtime configuration:

```bash
sudo firewall-cmd --reload
sudo firewall-cmd --zone=public --query-service=http
```

If you added HTTP only for this exercise, remove it from both configurations:

```bash
sudo firewall-cmd --permanent --zone=public --remove-service=http
sudo firewall-cmd --zone=public --remove-service=http
```

If HTTP was already configured before the lab, preserve that original configuration instead.

## Verification

Explain:

- Why a runtime rule may disappear after a reload.
- Why opening a port does not guarantee an application is reachable.
- Why TCP and UDP must be specified correctly.

## Safety Note

Use temporary rules first. Do not remove SSH access or change firewall rules for the zone carrying your management connection.
