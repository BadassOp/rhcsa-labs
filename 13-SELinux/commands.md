# SELinux Commands

## 1. Check SELinux Status

```bash
getenforce
sestatus
```

- `getenforce`: Displays the current mode.
- `sestatus`: Displays detailed SELinux status and policy information.

## 2. Change the Runtime Mode

```bash
sudo setenforce 0
sudo setenforce 1
```

- `0`: Switch to permissive mode.
- `1`: Switch to enforcing mode.

These changes are temporary and do not persist across reboot. Use permissive mode only for controlled troubleshooting.

## 3. Inspect Security Contexts

```bash
ls -Z /var/www/html
ps -eZ
id -Z
```

- `ls -Z`: Displays file and directory contexts.
- `ps -eZ`: Displays process contexts.
- `id -Z`: Displays the current user's security context.

A context commonly has this structure:

```text
user:role:type:level
```

The **type** is particularly important when troubleshooting many common access denials.

## 4. Restore Default File Contexts

```bash
sudo restorecon -v /var/www/html/index.html
sudo restorecon -Rv /var/www/html
```

`restorecon` restores labels based on the system's file-context policy. It does not grant arbitrary permissions.

## 5. Manage Persistent File Context Rules

Inspect existing rules:

```bash
sudo semanage fcontext -l
```

Add a persistent rule for a custom web directory:

```bash
sudo semanage fcontext -a -t httpd_sys_content_t '/webdata(/.*)?'
sudo restorecon -Rv /webdata
```

If the rule already exists, use `-m` to modify it instead of `-a`.

Remove the custom rule when it is no longer needed:

```bash
sudo semanage fcontext -d '/webdata(/.*)?'
```

After removing a rule, inspect the resulting labels and restore them according to the intended policy.

## 6. Inspect SELinux Booleans

```bash
getsebool -a
getsebool httpd_can_network_connect
```

Change a boolean temporarily:

```bash
sudo setsebool httpd_can_network_connect on
```

Make a change persistent:

```bash
sudo setsebool -P httpd_can_network_connect on
```

Only enable a boolean when its behavior is needed and understood.

## 7. Inspect SELinux Port Labels

```bash
sudo semanage port -l
sudo semanage port -l | grep http_port_t
```

Add a custom TCP port label for HTTP services, if appropriate:

```bash
sudo semanage port -a -t http_port_t -p tcp 8081
```

If the port already has a SELinux port mapping, inspect it first. Modifying an existing mapping may require `-m` rather than `-a`.

## 8. Troubleshoot Denials

```bash
sudo ausearch -m AVC -ts recent
sudo journalctl -t setroubleshoot
```

The `setroubleshoot` log may not exist if the relevant service or package is unavailable.

If installed, the `sealert` utility can provide additional analysis:

```bash
sudo sealert -a /var/log/audit/audit.log
```

## 9. Install Common Management Tools

On RHEL, the `semanage` command is commonly provided by `policycoreutils-python-utils`.

```bash
sudo dnf install policycoreutils-python-utils
```

Other troubleshooting utilities may require additional packages.

## Important

Do not automatically apply every suggested fix from diagnostic tools. Understand the denial and the security impact before changing policy.
