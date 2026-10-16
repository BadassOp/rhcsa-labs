# Networking Basics — Mental Model

## 1. How a Linux Host Communicates

A simplified view of communication between two hosts:

```text
Application
    |
    v
Transport Layer (TCP/UDP)
    |
    v
IP Layer (IPv4/IPv6)
    |
    v
Network Interface and Link Layer
    |
    v
Local Network or Router
    |
    v
Destination Host
```

Each layer handles a different responsibility. Applications use ports, TCP and UDP provide transport, IP handles addressing and routing, and the link layer delivers frames on the local network.

## 2. IP Address, Subnet, and Gateway

An IP address identifies an interface within an IP network.

A CIDR prefix specifies how many leading bits represent the network.

Example:

`192.168.1.10/24`

- IP address: `192.168.1.10`
- Prefix length: `24`
- Subnet mask: `255.255.255.0`
- Network address: `192.168.1.0`
- Broadcast address: `192.168.1.255`
- Conventional usable host range: `192.168.1.1` through `192.168.1.254`

A default gateway is the next hop used when no more specific route matches a destination.

## 3. What Happens When You Open a Website?

A typical connection involves several steps:

1. The system resolves the hostname using its configured name-resolution mechanisms.
2. The kernel checks the routing table to select a route and next hop.
3. If the destination is on the local IPv4 network, the host uses ARP to resolve the next-hop MAC address. IPv6 uses Neighbor Discovery instead.
4. If the destination is remote, the packet is sent to the gateway.
5. TCP may establish a connection, or the application may use UDP.
6. The application exchanges data with the remote service.

DNS, routing, and transport connectivity are separate things. A failure at one layer can make an application unavailable even when other layers work.

## 4. NetworkManager vs `ip`

### `ip`

The `ip` command inspects and changes the kernel's current network state.

Examples:

```bash
ip addr
ip route
ip link
```

Changes made directly with `ip` are generally runtime changes and may disappear when networking is reconfigured or the system restarts.

### NetworkManager and `nmcli`

NetworkManager manages network devices and connection profiles. `nmcli` provides a command-line interface to configure those profiles and activate them.

Use NetworkManager for persistent network configuration on RHEL unless the environment specifically requires another supported method.

## 5. DHCP vs Static Addressing

### DHCP

A DHCP server supplies configuration such as an IP address, subnet information, gateway, and DNS servers.

Advantages:

- Centralized configuration
- Less manual administration
- Reduced risk of address conflicts when properly managed

### Static addressing

An administrator configures the address and related settings explicitly.

It requires correct values for:

- IP address and prefix
- Gateway, when needed
- DNS servers
- Address allocation and conflict avoidance

A wrong gateway or prefix can break connectivity even if the IP address looks valid.

## 6. DNS vs `/etc/hosts`

DNS resolves names through configured DNS servers.

The `/etc/hosts` file provides local static name mappings.

The system's name-service configuration determines how different sources are consulted. Use `getent hosts NAME` to test the system's configured resolution path.

## 7. TCP vs UDP

| TCP                                  | UDP                                                       |
| ------------------------------------ | --------------------------------------------------------- |
| Connection-oriented                  | Connectionless                                            |
| Provides ordered, reliable delivery  | Does not itself guarantee delivery or ordering            |
| Used by SSH and many web connections | Used by DNS in many cases, streaming, and other protocols |

UDP-based protocols can implement their own reliability or connection behavior at higher layers.

## 8. Common Troubleshooting Patterns

### Interface is down

Inspect `ip link` and NetworkManager device status.

### Interface has no expected address

Inspect `ip addr`, DHCP status, and the active connection profile.

### Local network works but external destinations fail

Check the default route, gateway reachability, and upstream connectivity.

### IP connectivity works but names fail

Inspect DNS configuration and test using `getent`, `dig`, or `nslookup`.

### Service is unreachable

Inspect the listening socket with `ss`, the firewall, the destination address, and the application logs.

### Ping fails

Check routing and filtering, but remember that ICMP may be blocked even when the application works.

## 9. RHCSA Exam Thinking

For networking tasks:

1. Identify the correct interface and connection profile.
2. Record the original configuration.
3. Determine the required IP, prefix, gateway, and DNS settings.
4. Configure the connection using the appropriate tool.
5. Activate the profile carefully.
6. Verify addresses, routes, DNS, and connectivity.
7. Confirm whether the configuration must persist across reboot.

**Golden rule:** Check the interface, address, route, DNS, and service separately. Do not assume one successful test proves the entire network is working.
