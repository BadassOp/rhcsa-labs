# SELinux

## Overview

SELinux (Security-Enhanced Linux) is a Linux security mechanism that enforces mandatory access control (MAC) through security policies.

Unlike traditional Linux permissions, SELinux can restrict what processes are allowed to access, even when standard file permissions appear to permit access.

## Core Concepts

- **SELinux modes:** Enforcing, Permissive, and Disabled.
- **Security contexts:** Labels attached to files, processes, ports, and other resources.
- **SELinux types:** Labels used by policy rules to control access.
- **Policy enforcement:** Rules determine which actions processes are allowed to perform.
- **Booleans:** Switches that enable or disable specific policy behaviors.
- **File context management:** Ensures files have appropriate SELinux labels.
- **Port labeling:** Associates network ports with SELinux port types.
- **Troubleshooting:** Uses audit logs and policy tools to investigate denials.

## Important Commands

```bash
getenforce
sestatus
ls -Z
ps -eZ
id -Z
restorecon -Rv /path
semanage fcontext -l
semanage port -l
getsebool -a
ausearch -m AVC -ts recent
```

## Learning Objectives

- Understand the purpose of SELinux and mandatory access control.
- Identify SELinux modes and security contexts.
- Inspect file and process labels.
- Restore default file contexts.
- Manage persistent file context rules.
- Inspect SELinux booleans and port labels.
- Investigate access denials without disabling SELinux.

## Best Practices

- Keep SELinux in enforcing mode where supported by the environment and policy.
- Diagnose denials before changing policy.
- Prefer `restorecon` when correcting incorrect labels.
- Use `semanage fcontext` for persistent custom file-context rules.
- Avoid using `setenforce 0` as a permanent workaround.
- Do not disable SELinux simply because an application fails.