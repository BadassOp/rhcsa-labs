# Lab 01: Network Interfaces, IP Addressing, and Routes

## Objective

Inspect network interfaces, identify IP addresses, understand routing, and examine local neighbor information.

## Tasks

### 1. Inspect network interfaces

```bash
ip link
ip addr
ip -br addr
nmcli device status
```

Record:

- Interface names.
- Interface state.
- IPv4 and IPv6 addresses.
- Which interface is managed by NetworkManager.

### 2. Inspect IP configuration

```bash
ip -4 addr
ip -6 addr
ip -s link
```

Identify the IPv4 prefix length and any configured IPv6 addresses.

### 3. Inspect routing tables

```bash
ip route
ip -6 route
ip route get 8.8.8.8
```

Answer:

- What is the default route?
- Which interface would the kernel use to reach `8.8.8.8`?
- What is the difference between a connected route and a default route?

A route lookup does not prove that the destination is reachable.

### 4. Inspect neighbor information

```bash
ip neigh show
```

Generate some local traffic, if appropriate, and run the command again.

Explain what a neighbor entry represents and why the table may initially be empty.

### 5. Inspect the active NetworkManager profile

```bash
nmcli connection show --active
nmcli device show
```

Identify the active connection profile associated with your primary network interface.

### 6. Practice CIDR calculations

Calculate the following on paper or in your notes:

| Address            | Network/prefix           | Usable IPv4 hosts |
| ------------------ | ------------------------ | ----------------: |
| `192.168.10.25/24` | Find the network address |               254 |
| `10.20.30.40/16`   | Find the network address |            65,534 |
| `172.16.5.10/30`   | Find the network address |                 2 |

For each example, identify the network address and broadcast address.

## Verification Checklist

- Identified interfaces and their states.
- Inspected IPv4 and IPv6 addresses.
- Identified the default route.
- Inspected neighbor entries.
- Matched an interface to its NetworkManager profile.
- Completed the CIDR exercises.

## Expected Outcome

You can inspect the current network configuration and explain how IP addresses, routes, and neighbor discovery work.
