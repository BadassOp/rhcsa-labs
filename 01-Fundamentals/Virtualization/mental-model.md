# Virtualization — Mental Model

Think of virtualization as:

Physical Hardware
↓
Hypervisor
↓
Virtual Hardware
↓
Virtual Machine
↓
Guest OS

## Resource Mapping

Physical CPU
↓
vCPU

Physical RAM
↓
vRAM

Physical Disk
↓
Virtual Disk

Physical NIC
↓
Virtual NIC

## Type 1

Hardware
↓
Hypervisor
↓
VMs

## Type 2

Hardware
↓
Host OS
↓
Hypervisor
↓
VMs
