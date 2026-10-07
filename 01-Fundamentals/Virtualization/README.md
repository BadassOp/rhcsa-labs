# Virtualization

## What is Virtualization?

Virtualization is the process of creating virtual versions of computing
resources such as servers, operating systems, storage, and networks.

A physical machine can be divided into multiple virtual machines (VMs),
with each VM operating as an independent system.

---

## Why is Virtualization Used?

Virtualization allows multiple workloads to run on the same physical
hardware.

Common benefits include:

- Better hardware utilization
- Isolation between workloads
- Easy testing and development
- Snapshots and cloning
- Faster provisioning
- Reduced hardware requirements
- Useful environments for training and labs

---

## What is a Hypervisor?

A hypervisor is software or firmware that creates and manages virtual
machines.

It allocates physical resources such as:

- CPU
- RAM
- Storage
- Network interfaces

to virtual machines.

There are two major categories:

1. Bare-metal hypervisor
2. Hosted hypervisor

---

## Bare-Metal Virtualization

A bare-metal hypervisor runs directly on the physical hardware.

Architecture:

Physical Hardware
↓
Hypervisor
↓
Virtual Machines

Examples include:

- VMware ESXi
- Microsoft Hyper-V
- KVM-based virtualization

---

## Hosted Virtualization

A hosted hypervisor runs as an application on top of a host operating
system.

Architecture:

Physical Hardware
↓
Host Operating System
↓
Hosted Hypervisor
↓
Virtual Machines

Examples include:

- Oracle VirtualBox
- VMware Workstation

---

## Bare-Metal vs Hosted

| Feature     | Bare-Metal             | Hosted          |
| ----------- | ---------------------- | --------------- |
| Runs on     | Physical hardware      | Host OS         |
| Common use  | Servers / data centers | Desktop / labs  |
| Performance | Generally higher       | Generally lower |
| Management  | Enterprise focused     | User friendly   |
| Example     | ESXi                   | VirtualBox      |

---

## Host vs Guest

### Host

The host is the physical or primary system providing resources to
virtual machines.

### Guest

A guest is the operating system running inside a virtual machine.

Example:

Host:

- Linux Mint

Hypervisor:

- VirtualBox

Guest:

- RHEL

---

## Virtual Machine Resources

A VM receives virtualized resources such as:

- vCPU
- vRAM
- Virtual Disk
- Virtual NIC

These resources are backed by the physical host.

---

## RHCSA Relevance

Virtualization is important for RHCSA practice because it allows us to
build isolated RHEL environments without requiring separate physical
machines.

A typical RHCSA practice environment may contain:

- RHEL Server VM
- Additional Linux VM for networking
- Virtual storage devices
- Virtual network interfaces

---

## Key Takeaways

- Virtualization allows multiple VMs to share physical hardware.
- A hypervisor manages virtual machines.
- Bare-metal hypervisors run directly on hardware.
- Hosted hypervisors run on top of a host operating system.
- The operating system inside a VM is called the guest OS.
- The physical/primary system providing resources is the host.
