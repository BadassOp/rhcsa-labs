# Lab 03: SELinux Troubleshooting

## Objective

Learn a systematic approach to investigating SELinux denials using audit logs and context-inspection tools.

## Prerequisites

- SELinux enabled.
- `sudo` privileges.
- Audit utilities available.
- A controlled test environment.

## Tasks

### 1. Check the current mode

```bash
getenforce
sestatus
```

For normal enforcement testing, SELinux should be in enforcing mode. Do not change the system mode just to manufacture an error.

### 2. Inspect recent AVC denials

```bash
sudo ausearch -m AVC -ts recent
```

If the output is empty, there may be no matching denials in the selected time range. That is not necessarily a problem.

Inspect audit service status:

```bash
sudo systemctl status auditd
```

The audit service may be managed differently in some environments.

### 3. Inspect application and system logs

```bash
sudo journalctl -p warning -b
sudo journalctl -u httpd -b
```

The second command is useful when Apache HTTP Server is installed. If it is not installed, choose a service available on your VM.

### 4. Inspect process and file labels

For a web-server troubleshooting example, if Apache is installed:

```bash
ps -eZ | grep httpd
ls -Zd /var/www/html
ls -Z /var/www/html
```

Check the labels and compare them with the expected policy for the service.

### 5. Investigate a hypothetical web-content denial

Suppose Apache cannot read a file under `/var/www/html`.

Check:

1. Does the file exist?
2. Do ordinary Unix permissions allow the service to read it?
3. Is the file labeled with an appropriate type, such as `httpd_sys_content_t`?
4. Are there relevant AVC denials?
5. Is there a custom file-context rule that should apply?

If a label is incorrect, restore the expected label:

```bash
sudo restorecon -v /var/www/html/index.html
```

Only use this command for an existing file whose label you have verified should be restored to the policy-defined default.

### 6. Inspect SELinux booleans

```bash
getsebool -a
getsebool httpd_can_network_connect
```

Do not enable a boolean unless the application's intended behavior requires it and you understand the security consequences.

### 7. Review the troubleshooting workflow

```text
Application failure
       |
       v
Check service and ordinary permissions
       |
       v
Inspect SELinux contexts
       |
       v
Review AVC audit records
       |
       v
Identify the specific policy mismatch
       |
       v
Apply the smallest justified correction
       |
       v
Retest the application
```

## Verification

Explain:

- What an AVC denial indicates.
- Why an empty `ausearch` result does not prove SELinux is irrelevant.
- Why disabling SELinux is not a good first troubleshooting step.
- How `restorecon`, `semanage fcontext`, and SELinux booleans serve different purposes.

## Cleanup

This lab does not require changes to SELinux mode or policy. If you made any optional configuration changes, restore the original settings after recording your results.

## Safety

Do not apply automatically generated policy modules or broad permission changes without understanding the denial. Prefer correcting labels or making narrowly scoped policy changes when justified.
