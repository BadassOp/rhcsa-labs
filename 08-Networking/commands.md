# Networking Basics — Commands Cheat Sheet

## 1. Inspect Network Interfaces

```bash
ip link
ip addr
ip -br addr
ip -s link
nmcli device status
```

- `ip link`: Lists interfaces and link states.
- `ip addr`: Displays interface addresses.
- `ip -br addr`: Displays a compact address summary.
- `ip -s link`: Displays interface statistics.
- `nmcli device status`: Shows NetworkManager device status.

## 2. Inspect Network Connections

```bash
nmcli connection show
nmcli connection show --active
nmcli device show
```

Show details for a connection profile:

```bash
nmcli connection show "CONNECTION_NAME"
```

Replace `CONNECTION_NAME` with an actual connection profile name.

## 3. IP Address and Link Operations

```bash
ip addr show
ip route show
ip neigh show
```

Temporary address example:

```bash
sudo ip addr add 192.0.2.10/24 dev enp0s3
```

This uses a documentation-only IP range. Use it only in an isolated practice environment after verifying the interface. An address added with `ip addr` is not a persistent NetworkManager configuration.

Remove the example address if you added it:

```bash
sudo ip addr del 192.0.2.10/24 dev enp0s3
```

## 4. Configure Networking with NetworkManager

Inspect connection profiles first:

```bash
nmcli connection show
```

Set a static IPv4 address on an existing profile:

```bash
sudo nmcli connection modify "CONNECTION_NAME" \
  ipv4.method manual \
  ipv4.addresses 192.0.2.10/24 \
  ipv4.gateway 192.0.2.1 \
  ipv4.dns "192.0.2.53"
```

These are documentation addresses, not real gateway or DNS settings. Do not apply them to your working VM connection unless you have configured an isolated test network that uses those addresses.

For DHCP:

```bash
sudo nmcli connection modify "CONNECTION_NAME" ipv4.method auto
```

Inspect the profile before activating it. Activating a modified connection can interrupt SSH access.

## 5. Activate a Connection

```bash
nmcli connection show
sudo nmcli connection up "CONNECTION_NAME"
```

To deactivate a connection:

```bash
sudo nmcli connection down "CONNECTION_NAME"
```

Do not deactivate the connection you depend on for remote access unless you have a recovery path through the VM console.

## 6. Hostname Configuration

```bash
hostname
hostnamectl
hostnamectl status
```

Set a hostname:

```bash
sudo hostnamectl set-hostname rhel-lab
```

Use a suitable hostname for your environment.

## 7. Routing

```bash
ip route
ip -6 route
ip route get 8.8.8.8
```

These commands inspect routes and the kernel's route selection. The route lookup does not prove that the destination is reachable.

## 8. DNS and Name Resolution

```bash
cat /etc/resolv.conf
cat /etc/hosts
getent hosts example.com
```

If installed:

```bash
dig example.com
nslookup example.com
```

`getent hosts` uses the system's configured name-resolution mechanisms, while `dig` queries DNS directly.

## 9. Connectivity Troubleshooting

```bash
ping -c 4 127.0.0.1
ping -c 4 8.8.8.8
ping -c 4 example.com
```

A failed ping does not always mean a host is down; ICMP traffic may be filtered.

Inspect the path:

```bash
tracepath example.com
traceroute example.com
```

One or both tools may need to be installed. Network filtering can prevent them from showing every hop.

## 10. Ports and Connections

```bash
ss -tuln
ss -tulpn
ss -tn
```

- `-t`: TCP
- `-u`: UDP
- `-l`: Listening sockets
- `-n`: Numeric addresses and ports
- `-p`: Process information, where permitted

## 11. Packet Capture

If `tcpdump` is installed:

```bash
sudo tcpdump -i any -nn
```

Limit the capture to ICMP:

```bash
sudo tcpdump -i any -nn icmp
```

Stop with `Ctrl+C`. Capture only traffic you are authorized to inspect.

## 12. Troubleshooting Workflow

1. Check the interface state using `ip link`.
2. Inspect the IP address using `ip addr`.
3. Check routes using `ip route`.
4. Test local connectivity.
5. Test external IP connectivity.
6. Test hostname resolution.
7. Inspect listening sockets and logs.
8. Investigate NetworkManager profiles and packet captures as needed.

Always record the original network settings before changing them.
