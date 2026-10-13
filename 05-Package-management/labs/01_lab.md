# Lab 01: Package Installation and Inspection

## Objective

Install a package using DNF, inspect its metadata and files, and verify its installation.

## Tasks

### 1. Check repository availability

```bash
sudo dnf repolist
sudo dnf makecache
```

If this fails, investigate repository configuration before proceeding.

### 2. Search for a package

Use `httpd` as the example package:

```bash
dnf search httpd
dnf info httpd
```

Record the package version and repository information.

### 3. Install the package

```bash
sudo dnf install httpd
```

Review the proposed transaction before confirming it.

### 4. Verify installation

```bash
rpm -q httpd
dnf list installed httpd
rpm -qi httpd
```

### 5. Inspect package files

```bash
rpm -ql httpd
rpm -qc httpd
rpm -qd httpd
```

Identify:

- One configuration file.
- One documentation file, if provided.
- The main executable or server binary.

### 6. Find package ownership

Choose a file listed by `rpm -ql httpd` and run:

```bash
rpm -qf /path/to/file
```

Replace the example path with an actual file path from your system.

## Verification Checklist

- Checked repository availability.
- Searched for a package.
- Installed `httpd`.
- Verified the installed version.
- Inspected package files and configuration files.
- Identified which package owns a file.

## Expected Outcome

You can install packages using DNF and inspect their installed contents using RPM.
