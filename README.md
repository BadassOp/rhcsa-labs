# RHCSA Labs — Linux System Administration

A structured, hands-on learning repository for **Red Hat Enterprise Linux (RHEL)** and the **Red Hat Certified System Administrator (RHCSA)** certification.

This repository documents concepts, practical labs, mental models, useful commands, troubleshooting scenarios, and lessons learned while building Linux administration skills.

## Objectives

- Prepare for the RHCSA certification.
- Develop practical Linux system administration skills.
- Understand concepts instead of memorizing commands.
- Practice real-world administration and troubleshooting scenarios.
- Maintain organized, reusable technical documentation.

## Repository Structure

| Directory                | Contents                                                      |
| ------------------------ | ------------------------------------------------------------- |
| `01-fundamentals/`       | Virtualization, Linux architecture, and foundational concepts |
| `02-file-management/`    | File operations, paths, links, and file discovery             |
| `03-users-and-groups/`   | User, group, and password management                          |
| `04-permissions/`        | Permissions, ownership, ACLs, and special permissions         |
| `05-process-management/` | Process monitoring and management                             |
| `06-package-management/` | RPM, DNF, and software installation                           |
| `07-systemd/`            | Services, targets, and systemd                                |
| `08-networking/`         | Network configuration and troubleshooting                     |
| `09-storage/`            | Partitions, filesystems, mounting, and `/etc/fstab`           |
| `10-lvm/`                | Logical Volume Manager                                        |
| `11-swap/`               | Swap space management                                         |
| `12-firewalld/`          | Firewall configuration and troubleshooting                    |
| `13-selinux/`            | SELinux contexts, policies, and troubleshooting               |
| `14-ssh/`                | SSH configuration and remote administration                   |
| `15-scheduling/`         | Cron and scheduled tasks                                      |
| `16-logs/`               | System logs and journal analysis                              |
| `17-boot-and-recovery/`  | Boot process and system recovery                              |
| `18-troubleshooting/`    | Practical Linux troubleshooting scenarios                     |
| `cheat-sheets/`          | Quick command references                                      |
| `resources/`             | References and additional learning resources                  |

_Directories and topics will be added as the repository develops._

## Learning Materials

Each topic may contain the following resources:

- **Concepts:** What the technology is and why it matters.
- **Mental Models:** Visual explanations of how things work.
- **Useful Commands:** Commands with concise explanations and examples.
- **Hands-on Labs:** Step-by-step practical exercises.
- **Troubleshooting:** Common errors, diagnosis, and solutions.
- **Verification:** Commands and checks to confirm the expected result.

## Start Here

### Fundamentals

- [Virtualization](./01-Fundamentals/Virtualization/README.md) — Bare-metal and hosted virtualization, hypervisors, and virtual machines.

More topics will be linked here as they are documented.

## Lab Environment

The lab environment uses virtualization to run RHEL systems for practical Linux administration exercises.

Typical components include:

- **Host OS:** Windows OR Anything
- **Hypervisor:** Oracle VirtualBox
- **Guest OS:** Red Hat Enterprise Linux
- **Lab resources:** Virtual CPUs, memory, virtual disks, and virtual network interfaces

Exact configurations may vary between labs.

## Lab Documentation Standard

Each practical lab should document:

1. **Objective** — What the lab teaches.
2. **Prerequisites** — Required knowledge and resources.
3. **Environment** — System and configuration details.
4. **Procedure** — Commands and configuration steps.
5. **Expected Results** — What successful execution looks like.
6. **Verification** — How to confirm the result.
7. **Troubleshooting** — Common errors and fixes.
8. **Cleanup** — How to safely revert temporary changes, where applicable.

## Important Notes

- Commands are tested where possible in the documented environment.
- Outputs may differ depending on RHEL version and system configuration.
- Destructive operations involving disks, partitions, filesystems, or boot settings should be performed only in an appropriate lab environment.
- This repository is an independent learning project and is not an official Red Hat publication.

## Learning Progress

Topics are documented progressively as concepts are studied and labs are completed. The repository is intended to evolve alongside practical experience.

## Author

Maintained as a personal learning and portfolio project focused on Linux system administration, RHCSA preparation, and hands-on troubleshooting.
