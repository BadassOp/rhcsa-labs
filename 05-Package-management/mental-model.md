# Package Management — Mental Model

## 1. Understand the Package Management Stack

Think of package management as three connected components:

```text
Repositories
     |
     v
    DNF
     |
     v
    RPM
     |
     v
Installed files and package database
```

### Repositories

Repositories provide package files and metadata. Metadata tells DNF which packages are available and what dependencies they require.

### DNF

DNF is the high-level package manager. It searches repositories, resolves dependencies, and coordinates package installation, removal, and updates.

### RPM

RPM is the underlying package format and package management tool. It can query installed package information, inspect files, verify package contents, and install or remove individual packages.

## 2. DNF vs RPM

| Task                                                | Preferred tool |
| --------------------------------------------------- | -------------- |
| Install a package from repositories                 | DNF            |
| Resolve dependencies automatically                  | DNF            |
| Search available repository packages                | DNF            |
| Query an installed package                          | RPM or DNF     |
| List files belonging to an installed package        | RPM            |
| Verify installed package files                      | RPM            |
| Install a local RPM file with dependency resolution | DNF            |

**Rule of thumb:** Use DNF for normal software management. Use RPM when you need low-level package inspection or verification.

## 3. What Happens During Installation?

When you run:

```bash
sudo dnf install httpd
```

DNF generally:

1. Reads enabled repository metadata.
2. Locates the requested package.
3. Resolves dependencies.
4. Displays the proposed transaction.
5. Downloads required packages.
6. Verifies package signatures and integrity according to configuration.
7. Installs packages and updates the RPM database.

The exact steps depend on repository configuration, cache state, and the transaction.

## 4. Package Names and Dependencies

A package can depend on other packages or libraries. DNF attempts to satisfy those dependencies before completing a transaction.

Removing a package can also affect dependent software. Review the proposed transaction rather than assuming only one package will change.

## 5. Troubleshooting by Layer

### Package not found

- Check spelling.
- Search using `dnf search`.
- Check enabled repositories.
- Confirm the package is available for your RHEL version.

### Repository errors

- Check network connectivity.
- Inspect enabled repositories.
- Check repository URLs and configuration.
- Review the exact error before changing repository settings.

### Dependency errors

- Read the dependency conflict details.
- Check enabled repositories and package versions.
- Avoid forcing installation without understanding the conflict.

### Installed package behaves unexpectedly

- Check package ownership of relevant files.
- Inspect package metadata.
- Use `rpm -V PACKAGE` to look for file differences.

## 6. RHCSA Exam Thinking

For package management tasks:

1. Identify the package and desired state.
2. Check repository availability.
3. Use DNF for installation or removal.
4. Verify the result with `rpm -q` or `dnf list installed`.
5. Troubleshoot based on the exact error.

A successful command is not enough; always verify that the system reached the required state.
