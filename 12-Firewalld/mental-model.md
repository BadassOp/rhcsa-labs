# Firewalld — Mental Model

## 1. What Is Firewalld?

Firewalld is a management layer for host firewall rules. It provides a way to define which traffic is allowed or rejected without manually managing every low-level packet-filtering rule.

The `firewall-cmd` utility communicates with the firewalld service.

## 2. Understand Zones

A zone represents a firewall policy associated with network interfaces or source addresses.

Common zones include:

- `public`: For networks where other hosts are not trusted.
- `home`: For relatively trusted home networks.
- `internal`: For internal networks.
- `trusted`: Accepts network traffic with minimal filtering; use carefully.
- `drop`: Drops incoming packets without normal responses.
- `block`: Rejects incoming connections with responses.

The correct zone depends on the trust level and intended use of the network.

Inspect the current assignment:

```bash
sudo firewall-cmd --get-active-zones
```

A rule added to the wrong zone may not affect the interface you intended to protect.

## 3. Service vs Port

A service definition groups the ports and protocols required by an application.

For example, the `http` service represents standard HTTP traffic, while `8080/tcp` represents a specific TCP port.

Prefer a predefined service when it accurately represents the application. Use an explicit port rule when you need to allow a particular port.

## 4. Runtime vs Permanent Rules

Firewalld maintains runtime and permanent configurations.

### Runtime

Changes take effect in the current runtime but are not necessarily saved across reloads or restarts.

### Permanent

Changes are stored in configuration. They generally become active after a reload or another appropriate activation step.

A reload replaces runtime configuration with the permanent configuration. Unsaved runtime-only changes can be lost.

## 5. Rich Rules

Rich rules express more detailed policies than simple service or port rules. They can match conditions such as source addresses, destination addresses, services, ports, logging, and actions.

Use the narrowest rule that satisfies the requirement. Avoid broad allow rules that expose services unnecessarily.

## 6. Masquerading and Port Forwarding

### Masquerading

Masquerading performs source network address translation, commonly used when internal hosts share an external IP address.

A functional gateway also needs appropriate routing, IP forwarding, and correctly configured interfaces.

### Port forwarding

Port forwarding redirects traffic arriving on one port to another port or destination.

It does not guarantee application availability. The destination must be reachable, the service must be listening, and routing and firewall rules must permit the connection.

## 7. Troubleshooting Workflow

When a connection fails:

1. Check whether firewalld is running.
2. Identify the active zone and interface assignment.
3. Inspect runtime and permanent rules.
4. Confirm the application is listening with `ss`.
5. Check IP addressing and routes.
6. Inspect logs and test the connection again.

Do not assume the firewall is the cause until you verify the service and network path.

## 8. RHCSA Exam Thinking

For a firewall task:

1. Identify the relevant zone.
2. Determine whether a service or port rule is required.
3. Apply the correct runtime or permanent change.
4. Reload if needed.
5. Query the resulting configuration.
6. Verify that the intended traffic can reach the service.
7. Remove test rules when the exercise is complete.

**Golden rule:** Correct zone, minimum required access, persistent configuration when requested, and verification.
