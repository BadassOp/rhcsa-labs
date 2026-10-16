# Networking Basics

## Overview

Networking is a core Linux system administration skill. Administrators must configure network interfaces, assign IP addresses, manage routes, troubleshoot connectivity, and understand how DNS resolves hostnames.

This module combines networking fundamentals with practical RHEL networking tasks using NetworkManager and command-line tools.

## Learning Objectives

- Understand the OSI and TCP/IP models.
- Understand IPv4, IPv6, private and public IP addresses.
- Calculate CIDR ranges and practice subnetting.
- Understand MAC addresses, ARP, TCP, and UDP.
- Inspect network interfaces and IP configuration.
- Configure networking using `ip` and `nmcli`.
- Understand DHCP, static IP configuration, gateways, and routing.
- Troubleshoot DNS and connectivity problems.
- Inspect listening ports and active network connections.
- Understand hostname resolution and basic packet analysis.

## Key Topics

### 1. Networking Fundamentals

- OSI and TCP/IP models
- LAN, WAN, and VLAN concepts
- MAC addresses and ARP
- TCP vs UDP
- Ports and sockets

### 2. IP Addressing

- IPv4 and IPv6
- Public and private addresses
- Subnet masks and CIDR
- Network, host, and broadcast addresses
- Default gateways
- Subnetting practice

### 3. Network Configuration in RHEL

- Network interfaces
- `ip` commands
- NetworkManager
- `nmcli`
- DHCP and static addressing
- DNS configuration
- Hostname configuration

### 4. Routing and Name Resolution

- Routing tables
- Default routes
- `/etc/hosts`
- DNS resolvers
- `getent`, `dig`, and `nslookup`

### 5. Network Troubleshooting

- `ping`
- `tracepath` and `traceroute`
- `ip route`
- `ss`
- `tcpdump`
- NetworkManager connection profiles
- Common connectivity failures

## Essential Commands

```bash
ip addr
ip route
ip link
nmcli device status
nmcli connection show
hostnamectl
getent hosts example.com
ss -tulpn
ping -c 4 127.0.0.1
```

## Learning Outcome

By completing this module, you should be able to inspect and configure RHEL networking, understand addressing and routing, diagnose DNS and connectivity issues, and explain how Linux communicates across networks.

## Lab Environment

- RHEL 10 virtual machine
- VirtualBox network adapter
- Root access or appropriate sudo privileges
- A recovery method such as the VM console before changing network settings
