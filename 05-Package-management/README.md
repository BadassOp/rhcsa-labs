# Package Management

## Overview

Package management in Red Hat Enterprise Linux (RHEL) involves installing, updating, querying, and removing software packages while managing their dependencies.

RHEL primarily uses **DNF** as its high-level package manager and **RPM** for working directly with individual RPM packages.

## Learning Objectives

- Understand RPM packages and package repositories.
- Install, update, and remove software using DNF.
- Query installed packages using DNF and RPM.
- Search repositories for available packages.
- Inspect package files and metadata.
- Understand repository configuration and dependency resolution.
- Troubleshoot common package management issues.

## Key Topics

### 1. RPM Basics

- RPM package format
- Package names and versions
- Querying installed packages
- Inspecting package metadata
- Verifying installed packages

### 2. DNF Package Management

- Installing packages
- Removing packages
- Updating packages
- Searching for packages
- Listing installed and available packages
- Checking package information
- Understanding dependencies

### 3. Repositories

- Repository concepts
- Listing enabled repositories
- Repository configuration
- Enabling and disabling repositories
- Refreshing repository metadata

### 4. Package Troubleshooting

- Repository connectivity issues
- Missing packages
- Dependency errors
- Metadata problems
- Understanding package installation logs

## Essential Commands

```bash
dnf search PACKAGE
dnf info PACKAGE
dnf list installed
dnf install PACKAGE
dnf remove PACKAGE
dnf update
dnf repolist
rpm -q PACKAGE
rpm -qi PACKAGE
rpm -ql PACKAGE
rpm -V PACKAGE
```

Replace `PACKAGE` with an actual package name.

## Learning Outcome

By completing this module, you should be able to manage software packages, inspect installed software, work with repositories, and troubleshoot common package management problems in RHEL.

## Lab Environment

- RHEL virtual machine
- Root access or a user with appropriate sudo privileges
- Working package repositories and network connectivity, where required
