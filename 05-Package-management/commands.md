# Package Management — Commands Cheat Sheet

## 1. DNF Basics

```bash
dnf --version
dnf repolist
dnf repolist --all
dnf makecache
```

- `dnf --version`: Display DNF version.
- `dnf repolist`: List enabled repositories.
- `dnf repolist --all`: Show enabled and disabled repositories.
- `dnf makecache`: Download or refresh repository metadata.

## 2. Search and Inspect Packages

```bash
dnf search httpd
dnf info httpd
dnf list available
dnf list installed
dnf list --all httpd
```

## 3. Install, Remove, and Update

```bash
sudo dnf install httpd
sudo dnf remove httpd
sudo dnf update
sudo dnf update httpd
```

Check the proposed transaction before confirming installation or removal.

## 4. RPM Queries

```bash
rpm -q httpd
rpm -qi httpd
rpm -ql httpd
rpm -qc httpd
rpm -qd httpd
rpm -qf /etc/hosts
```

- `rpm -q`: Check whether a package is installed.
- `rpm -qi`: Display package information.
- `rpm -ql`: List files installed by a package.
- `rpm -qc`: List configuration files.
- `rpm -qd`: List documentation files.
- `rpm -qf`: Identify which installed package owns a file.

## 5. Package Verification

```bash
rpm -V httpd
rpm -Va
```

`rpm -V` compares installed files against recorded package metadata. Output can indicate differences; it does not automatically mean a system has been compromised.

Use `rpm -Va` cautiously because it checks many installed packages and may produce substantial output.

## 6. Repository Management

```bash
dnf repolist
dnf repolist --all
dnf config-manager --help
```

The availability of `dnf config-manager` commands depends on the installed DNF version and supporting plugins. Check `--help` before using repository-management options.

Repository configuration files are commonly located in:

```bash
/etc/yum.repos.d/
```

Inspect them:

```bash
ls -l /etc/yum.repos.d/
```

## 7. Transaction History

```bash
dnf history
dnf history info
```

Use `dnf history info` with an appropriate transaction ID to inspect a specific transaction.

## 8. Downloading Packages

If the required plugin is installed:

```bash
dnf download httpd
```

The `download` subcommand may require the `dnf-plugins-core` package, depending on the RHEL version and installed tools.

## Safety Notes

- Avoid removing packages without checking dependencies.
- Review transaction summaries before accepting changes.
- Do not disable repositories or signature verification as a routine troubleshooting shortcut.
- On a production server, plan updates and confirm backup or recovery arrangements first.
