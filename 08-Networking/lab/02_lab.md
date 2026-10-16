# Lab 02: NetworkManager and Persistent Configuration

## Objective

Inspect NetworkManager profiles, understand DHCP and static configuration, and practice a safe temporary configuration change.

## Tasks

### 1. Inspect connection profiles

```bash
nmcli connection show
nmcli connection show --active
nmcli device status
```

Record the name of your active connection profile and its interface.

### 2. Inspect IPv4 settings

Replace `CONNECTION_NAME` with your actual profile name:

```bash
nmcli connection show "CONNECTION_NAME"
```

Inspect the following properties:

```bash
nmcli -g ipv4.method connection show "CONNECTION_NAME"
nmcli -g ipv4.addresses connection show "CONNECTION_NAME"
nmcli -g ipv4.gateway connection show "CONNECTION_NAME"
nmcli -g ipv4.dns connection show "CONNECTION_NAME"
```

Determine whether the profile uses DHCP (`auto`) or manual addressing (`manual`).

### 3. Understand static configuration

A persistent static configuration can be set with:

```bash
sudo nmcli connection modify "CONNECTION_NAME" \
  ipv4.method manual \
  ipv4.addresses "192.0.2.10/24" \
  ipv4.gateway "192.0.2.1" \
  ipv4.dns "192.0.2.53"
```

These are documentation-only IP addresses. **Do not apply this example to your working VM connection.** It could remove your connectivity.

Instead, document the command and explain which values must match the actual network.

### 4. Inspect DHCP configuration

For a DHCP-managed profile, the general configuration is:

```bash
sudo nmcli connection modify "CONNECTION_NAME" ipv4.method auto
```

Do not apply it to your active profile unless you intend to change that profile and have recorded the original settings.

### 5. Practice a safe temporary IP change

Use only an isolated, disposable lab interface that is not carrying your SSH connection or default route.

Inspect the interface first:

```bash
ip -br addr
ip route
```

If you have verified an unused test interface named `enp0s8`, you can temporarily add a documentation address:

```bash
sudo ip addr add 192.0.2.10/24 dev enp0s8
ip addr show dev enp0s8
```

Remove the test address afterward:

```bash
sudo ip addr del 192.0.2.10/24 dev enp0s8
```

Replace `enp0s8` with your actual isolated test interface. If no safe test interface exists, skip this step and document the expected result.

### 6. Verify persistent settings without applying risky changes

```bash
nmcli connection show
nmcli device status
ip -br addr
ip route
```

Explain the difference between a temporary change made using `ip addr` and a persistent connection profile managed by NetworkManager.

## Verification Checklist

- Inspected NetworkManager profiles.
- Identified the IPv4 method.
- Inspected address, gateway, and DNS properties.
- Explained static versus DHCP configuration.
- Practiced a safe temporary change or documented why it was skipped.
- Verified the original working connection remained intact.

## Expected Outcome

You understand how RHEL manages network profiles and how to plan persistent configuration changes safely.
