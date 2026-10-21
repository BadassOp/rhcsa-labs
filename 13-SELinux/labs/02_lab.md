# Lab 02: Persistent SELinux File Contexts

## Objective

Create a custom file-context rule with `semanage fcontext`, apply it using `restorecon`, and remove the rule cleanly.

## Prerequisites

- SELinux enabled.
- `sudo` privileges.
- The `semanage` command available.

If needed, install the common SELinux management utilities:

```bash
sudo dnf install policycoreutils-python-utils
```

## Tasks

### 1. Create a custom directory

```bash
sudo mkdir -p /webdata
sudo touch /webdata/index.html
```

Inspect the current and expected contexts:

```bash
ls -Zd /webdata
ls -Z /webdata/index.html
sudo matchpathcon /webdata/index.html
```

### 2. Add a persistent file-context rule

Assign the standard read-only web-content type to the lab path:

```bash
sudo semanage fcontext -a -t httpd_sys_content_t '/webdata(/.*)?'
```

If the exact rule already exists, inspect it and use `-m` to modify it instead of adding a duplicate.

### 3. Apply the rule

```bash
sudo restorecon -Rv /webdata
```

Verify the labels:

```bash
ls -Zd /webdata
ls -Z /webdata/index.html
sudo matchpathcon /webdata/index.html
```

The expected type should now be `httpd_sys_content_t`.

### 4. Confirm the rule persists in policy configuration

```bash
sudo semanage fcontext -l | grep '/webdata'
```

The rule is stored in SELinux file-context configuration. The actual labels are applied to the filesystem by `restorecon`.

### 5. Remove the custom rule

```bash
sudo semanage fcontext -d '/webdata(/.*)?'
```

Check the expected context after removing the custom rule:

```bash
sudo matchpathcon /webdata/index.html
```

Then remove the lab directory:

```bash
sudo rm -rf /webdata
```

## Verification

Explain:

- Why `chcon` is not a substitute for a persistent `semanage fcontext` rule.
- Why `restorecon` is needed after adding the rule.
- How the `(/.*)?` expression matches the directory and its contents.

## Optional Challenge

Create a second lab directory under `/srv/`, assign a suitable SELinux type for a specific service, and verify the result. Research the intended service policy before selecting a type.

## Safety

Use only the dedicated `/webdata` path created for this lab. Do not modify labels or file-context rules for existing production directories.
