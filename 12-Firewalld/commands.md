# Firewalld — Commands Cheat Sheet

## 1. Service Status

```bash
sudo systemctl status firewalld
sudo firewall-cmd --state
sudo firewall-cmd --version
```

## 2. Inspect Zones

```bash
sudo firewall-cmd --get-zones
sudo firewall-cmd --get-default-zone
sudo firewall-cmd --get-active-zones
sudo firewall-cmd --list-all-zones
```

Inspect a particular zone:

```bash
sudo firewall-cmd --zone=public --list-all
```

Replace `public` with the relevant zone when necessary.

## 3. Manage Services

List services allowed in a zone:

```bash
sudo firewall-cmd --zone=public --list-services
```

Add a service temporarily:

```bash
sudo firewall-cmd --zone=public --add-service=http
```

Add a service permanently:

```bash
sudo firewall-cmd --zone=public --add-service=http --permanent
sudo firewall-cmd --reload
```

Query and remove a service:

```bash
sudo firewall-cmd --zone=public --query-service=http
sudo firewall-cmd --zone=public --remove-service=http
```

For a permanent removal:

```bash
sudo firewall-cmd --zone=public --remove-service=http --permanent
sudo firewall-cmd --reload
```

## 4. Manage Ports

Open TCP port 8080 temporarily:

```bash
sudo firewall-cmd --zone=public --add-port=8080/tcp
```

Open it permanently:

```bash
sudo firewall-cmd --zone=public --add-port=8080/tcp --permanent
sudo firewall-cmd --reload
```

Verify:

```bash
sudo firewall-cmd --zone=public --query-port=8080/tcp
sudo firewall-cmd --zone=public --list-ports
```

Remove a port:

```bash
sudo firewall-cmd --zone=public --remove-port=8080/tcp
```

## 5. Runtime vs Permanent Configuration

```bash
sudo firewall-cmd --runtime-to-permanent
sudo firewall-cmd --reload
```

- Runtime changes apply to the current firewall runtime.
- Permanent changes are saved in configuration and normally require a reload to take effect.
- `--runtime-to-permanent` saves the current runtime configuration as permanent configuration.

Use `--runtime-to-permanent` carefully because it can persist unrelated runtime changes.

## 6. Rich Rules

Example: allow HTTP traffic from one IPv4 source in the public zone.

```bash
sudo firewall-cmd --zone=public \
  --add-rich-rule='rule family="ipv4" source address="192.0.2.10" service name="http" accept'
```

Use `192.0.2.10` only as a documentation example. Replace it with an authorized source address in your test network.

Add a rich rule permanently:

```bash
sudo firewall-cmd --zone=public \
  --add-rich-rule='rule family="ipv4" source address="192.0.2.10" service name="http" accept' \
  --permanent
sudo firewall-cmd --reload
```

List rich rules:

```bash
sudo firewall-cmd --zone=public --list-rich-rules
```

## 7. Masquerading

Inspect current masquerading state:

```bash
sudo firewall-cmd --zone=public --query-masquerade
```

Enable it temporarily:

```bash
sudo firewall-cmd --zone=public --add-masquerade
```

Enable it permanently only when the host is intentionally configured for routing or NAT:

```bash
sudo firewall-cmd --zone=public --add-masquerade --permanent
sudo firewall-cmd --reload
```

Masquerading alone does not configure IP forwarding, routing, or a complete NAT gateway.

## 8. Port Forwarding

Example: forward local TCP port 8080 to port 80 on the same host.

```bash
sudo firewall-cmd --zone=public \
  --add-forward-port=port=8080:proto=tcp:toport=80
```

For permanent configuration:

```bash
sudo firewall-cmd --zone=public \
  --add-forward-port=port=8080:proto=tcp:toport=80 \
  --permanent
sudo firewall-cmd --reload
```

Verify the configuration:

```bash
sudo firewall-cmd --zone=public --list-forward-ports
```

The destination service must be listening and reachable. Forwarding to another host also requires appropriate routing and NAT configuration.

## 9. Troubleshooting

```bash
sudo firewall-cmd --state
sudo firewall-cmd --get-active-zones
sudo firewall-cmd --zone=public --list-all
sudo journalctl -u firewalld -b
sudo ss -tulpn
```

Check the firewall, interface zone, routes, and listening service separately.

## Safety Notes

- Do not flush firewall rules indiscriminately.
- Avoid changing the zone of an interface used for remote administration without a recovery method.
- Open only the required services and ports.
- Remove temporary test rules after completing a lab.
- Inspect runtime and permanent configuration separately.
