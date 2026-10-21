# SELinux — Mental Model

## 1. Two Layers of Access Control

Linux access checks often involve two separate layers:

1. **Discretionary Access Control (DAC):** Traditional owner, group, and permission-bit checks.
2. **Mandatory Access Control (MAC):** SELinux policy determines whether an action is permitted.

An operation may pass DAC checks and still be denied by SELinux.

Conceptually:

```text
Application requests access
          |
          v
Traditional Linux permissions
          |
          v
SELinux policy checks
          |
          v
Access allowed or denied
```

This is a simplified model; actual access checks depend on the operation and kernel security hooks.

## 2. SELinux Modes

| Mode       | Behavior                                                                                     |
| ---------- | -------------------------------------------------------------------------------------------- |
| Enforcing  | Applies policy and denies prohibited actions                                                 |
| Permissive | Logs policy violations but does not deny them through SELinux                                |
| Disabled   | SELinux is not enforcing policy; changing to this state is not an ordinary runtime operation |

Permissive mode is useful for controlled diagnosis, but it should not be treated as a permanent solution.

## 3. Security Contexts

A context commonly looks like:

```text
system_u:object_r:httpd_sys_content_t:s0
```

| Field                 | Meaning        |
| --------------------- | -------------- |
| `system_u`            | SELinux user   |
| `object_r`            | SELinux role   |
| `httpd_sys_content_t` | SELinux type   |
| `s0`                  | Security level |

For many common service-access problems, identifying the correct **type** is a key troubleshooting step.

## 4. Types and Policy

Consider a web server attempting to read a file.

- The file has a SELinux type.
- The web server process runs in a SELinux domain.
- Policy rules determine whether that domain can perform the requested operation on that type.

Changing ordinary file permissions does not automatically change the SELinux policy decision.

## 5. File Labels vs. File-Context Rules

These are related but different:

- **Current label:** The context actually attached to a file, inspected using `ls -Z`.
- **Expected label:** The context defined by the system's file-context policy.
- **`restorecon`:** Reapplies the expected label.
- **`semanage fcontext`:** Adds or modifies persistent file-context rules.

When a file has the wrong label, prefer correcting the label over disabling SELinux.

## 6. Booleans

SELinux booleans control selected policy behaviors.

For example, a boolean may determine whether a service is permitted to make certain network connections.

Use:

```bash
getsebool -a
sudo setsebool BOOLEAN_NAME on
sudo setsebool -P BOOLEAN_NAME on
```

The `-P` option makes the boolean change persistent.

## 7. Port Labels

A network service may listen on a TCP or UDP port, but SELinux can also restrict which port types a confined service is permitted to use.

Therefore, check both:

- Firewall rules controlling network traffic.
- SELinux port policy controlling service access to port types.

Opening a firewall port alone does not resolve every SELinux port restriction.

## 8. Troubleshooting Sequence

When an application fails unexpectedly:

1. Confirm the application is running.
2. Check standard Linux permissions.
3. Inspect file and process contexts with `ls -Z` and `ps -eZ`.
4. Review AVC denials with `ausearch`.
5. Check file-context rules, booleans, and port labels as relevant.
6. Apply the smallest justified correction.
7. Retest the application.

## Core Principle

**Diagnose the policy decision; do not disable the security mechanism to hide the problem.**
