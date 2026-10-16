# Lab 03: DNS, Connectivity, and Troubleshooting

## Objective

Diagnose network problems by checking interfaces, addressing, routes, DNS, listening ports, and packet traffic.

## Tasks

### 1. Test local networking

```bash
ping -c 4 127.0.0.1
ip -br addr
ip route
```

Explain what a successful loopback test proves and what it does not prove.

### 2. Test external IP connectivity

```bash
ping -c 4 8.8.8.8
```

If the test fails, inspect the interface and route. ICMP traffic may be filtered, so a failed ping alone does not prove that the destination is unreachable.

### 3. Test hostname resolution

```bash
getent hosts example.com
cat /etc/resolv.conf
cat /etc/hosts
```

If available:

```bash
dig example.com
```

Compare system name resolution using `getent` with a direct DNS query using `dig`.

### 4. Inspect ports and sockets

```bash
ss -tuln
ss -tn
```

If permitted, inspect process information:

```bash
sudo ss -tulpn
```

Identify a listening TCP port and explain the difference between a listening socket and an established connection.

### 5. Trace the network path

If the relevant tools are installed:

```bash
tracepath example.com
traceroute example.com
```

A route trace may show incomplete results because intermediate routers can filter or deprioritize probe traffic.

### 6. Capture packets

If `tcpdump` is installed:

```bash
sudo tcpdump -i any -nn icmp
```

In a second terminal, generate test traffic:

```bash
ping -c 4 127.0.0.1
```

Stop the capture using `Ctrl+C`.

Explain which packets appeared and why loopback traffic is useful for this exercise.

### 7. Diagnose a simulated problem

Use the following scenarios as troubleshooting exercises. Do not deliberately break your working network.

**Scenario A: No IP address**

- Inspect `ip link` and `ip addr`.
- Check `nmcli device status`.
- Inspect the active connection profile and DHCP settings.

**Scenario B: IP works, hostname does not**

- Check the routing table.
- Inspect `/etc/resolv.conf`.
- Test with `getent hosts` and `dig`.

**Scenario C: Host is reachable but an application is not**

- Inspect listening sockets using `ss`.
- Check the destination port.
- Investigate firewalld and application logs.

Document the commands and findings for each scenario.

## Verification Checklist

- Tested loopback connectivity.
- Investigated external connectivity.
- Tested hostname resolution.
- Inspected listening sockets.
- Examined a route trace.
- Captured ICMP traffic or documented why it was unavailable.
- Completed all three troubleshooting scenarios.

## Expected Outcome

You can isolate network faults methodically instead of assuming every connectivity problem is a DNS or firewall issue.
