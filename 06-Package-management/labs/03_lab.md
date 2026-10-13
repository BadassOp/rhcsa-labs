# Lab 03: Repositories and Package Troubleshooting

## Objective

Understand repository configuration, diagnose common package management failures, and verify installed package files.

## Tasks

### 1. Inspect repositories

```bash
dnf repolist
dnf repolist --all
ls -l /etc/yum.repos.d/
```

Record the enabled repository IDs.

### 2. Inspect repository configuration

Read the available `.repo` files:

```bash
grep -E '^\[|^name=|^baseurl=|^metalink=|^mirrorlist=|^enabled=' /etc/yum.repos.d/*.repo
```

If there are no matching files, check whether repositories are configured through another supported mechanism.

Do not publish repository credentials or secrets.

### 3. Diagnose a package-not-found error

Try searching for a deliberately nonexistent package:

```bash
dnf search package-that-does-not-exist-12345
```

Answer:

- Does this prove that DNF is broken?
- What would you check if a real package could not be found?
- How do enabled repositories affect package availability?

### 4. Refresh metadata

```bash
sudo dnf clean metadata
sudo dnf makecache
```

If the cache refresh fails, record the exact error and investigate connectivity, repository URLs, subscription status, and metadata availability.

### 5. Verify package files

If `bash` is installed:

```bash
rpm -q bash
rpm -V bash
```

No output from `rpm -V` generally means no differences were detected for the files it checks. Any output should be investigated in context; configuration changes may be intentional.

### 6. Document your troubleshooting

Create a short note with:

- The command that failed.
- The exact error message.
- The likely cause.
- The checks performed.
- The fix, if one was required.

## Verification Checklist

- Listed enabled and disabled repositories.
- Inspected repository configuration.
- Explained why a package may not be found.
- Refreshed repository metadata.
- Verified an installed package.
- Documented a troubleshooting scenario.

## Expected Outcome

You can investigate common DNF and repository issues methodically without disabling security checks or changing repository settings blindly.
