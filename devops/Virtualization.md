- Virtualization is the ability to run multiple operating systems on a single physical system and share the underlying hardware resources
- It is the process by which one computer hosts the appearance of many computers
- A Virtual Machine (VM) is an isolated runtime environment (guest OS and applications)
- Multiple virtual systems (VMs) can run on a single physical system.

## Hypervisor
- A hypervisor, a.k.a a virtual machine manager/monitor (VMM), or virtualization manager, is a program that allows multiple operating systems to share a single hardware host.
- Each guest operating system appears to have the host's processor, memory, and other resources all to itself. However, the hypervisor is actually controlling the host processor and resources, allocating what is needed to each operating system in turn and making sure that the guest operating systems (called virtual machine) cannot disrupt each other.

## Benefits of Virtualization
- Sharing of resources helps cost reduction
- Isolation: Virtual machines are isolated from each other
- Encapsulation: Virtual machines encapsulate a complete computing environment

## Virtualization in Cloud Computing
- You don't need to own the hardware
- Resources are rented as needed from cloud 

## Trusted Computing Base (TCB)