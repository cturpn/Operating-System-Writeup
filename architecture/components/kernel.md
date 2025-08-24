# Kernel

The kernel is the fundamental component of any operating system.  
As this core part of the OS' it always has the complete control over everything in the system.
It manages communication between hardware and software, providing controlled access to system resources such as CPU, memory, and I/O devices.

Different operating systems implement different types of kernels.  
This documentation focuses primarily on the monolithic kernel(used by Linux), but many concepts also apply to other types, such as the hybrid kernel(used by Windows NT).

Check out the following diagram regarding the linux kernel mapping: 
https://upload.wikimedia.org/wikipedia/commons/thumb/5/5b/Linux_kernel_map.png/960px-Linux_kernel_map.png

## Kernel Types
There are multiple types of kernels. Linux for example has a kernel of the "monolithic" type, Windows NT on the other hand a "hybrid" one.
Every type has its advantages and disadvantages that fit their specific usecase.

Important to know, that there is a term "OS Architecture" which is often used interchangably with the kernel types. 
Altough they are connected to each other(e.g a OS with a monolithic kernel, will also have a monolithic OS architecture), they dont exactly stand for the same thing.
The OS architecture stands for the entire OS structure, while the kernel type for the kernel specifically.

If you are interested in a overview of operating systems and the kernel they use, check this out:
https://en.wikipedia.org/wiki/Comparison_of_operating_system_kernels

Additionally the following diagram gives a visualization of the 3 main types of kernels:
https://en.wikipedia.org/wiki/Monolithic_kernel#/media/File:OS-structure2.svg


### Monolithic 
In a monolithic kernel, all core system services and functions run within a single, protected memory region called kernel space. 
This memory is reserved and inaccessible to user-mode processes, ensuring system stability and security. 
Within kernel space, a virtual interface to the hardware is established. This interface abstracts hardware-specific details—such as memory layouts, I/O registers, and device behavior—through kernel-resident drivers. 
User-mode applications and processes interact with this interface via system calls, allowing them to perform operations on the hardware without accessing it directly. 

Although the monolithic kernel is entirely running in the kernel space, it is designed in layers. 
These are arranged in a logical order, starting with the most fundamental operations, like process management to the interfaces that are exposed to user applications such as the system calls interface (SCI)

#### Advantaged
 - Generally faster than other types of kernels, because the system services dont need to switch mode for every system call.
 - More efficient communication between system services, as they all coexist in the kernel space
 - Less latency as the system calls and interrupts can be handled by the kernel directly
 - Due to the unified structure simpler to design, implement and debug


#### Disadvantages
 - The size of the kernel itself is bigger than other types of kernel
 - A single system service crashing causes the entire to crash
 - All system services running in kernel mode causes the system to be more prone to vulnerabilities
 - Changes on a single service can affect the entire system

### Microkernel
The microkernel is a type of kernel that contains only the bare minimum of system services in its kernel space.
Therefore only the most important system services make it into the kernel space, such as the inter-process communication(IPC), memory management and CPU-scheduling. This can vary depending on the type of system.
All the other system services run in user-mode, which allows the user to directly interact with them and install, update or replace indiviual components without affecting the entire system.
The core concept of the microkernel is to provide a core system that ensures vital functionalities and therefore keep all non-vital components as modular as possible.
This design approach enables the system to be more stable and less prone to crashes, as a faulty or crashing system service will not crash any other services and

The services,that are not part of the kernel space, communicate with each other via inter-process communication(IPC) and are in this context also called "servers"
Microkernel servers are essentially daemon programs like any others, except that the kernel grants some of them privileges to interact with parts of physical memory that are otherwise off limits to most programs. This allows some servers, particularly device drivers, to interact directly with hardware. 


#### Advantages
 - More secure / less vulnerable, because only the most important system services run in the kernel space
 - More stable and less prone to full system crashes
 - The kernel size, is like the name suggest, a lot smaller than a monolithic kernel for example
 - The microkernel is very modular due to the services/servers running separated from others and in user-mode
 - Development is way easier and faster compared to a monolithic kernel


#### Disadvantages
 - Performance is lower due to the many system calls and IPC being done by the various user-mode servers/services
 - Including all services the microkernel architecture requires more code than a monolithic kernel

### Hybrid 


### Exokernel

## User Mode vs Kernel Mode
To fully understand the aforementioned advantages and disadvantages of the different kernel types it is important to know the two key states in a operating system. 
These two states, or also user mode and kernel mode have very different usecases due to how they affect security, stability and performance.
In short: the user mode is a restricted mode that limits access to system resources, while kernel mode is a privileged mode that allows direct access to system resources.
Here a quick comparison between the two states

| Characteristics       | User mode                                     | Kernel mode                                      |
|----------------------|-----------------------------------------------|-------------------------------------------------|
| Definition           | Restricted OS mode for running application code | Privileged mode for core OS functions          |
| Resource access      | Limited access to system resources and hardware | Full access to system resources and hardware  |
| Memory access        | Cannot access kernel memory directly; code is isolated | Unrestricted access to user and kernel memory; code is not isolated |
| Privilege level      | Lower privilege level                         | Higher privilege level                          |
| Purpose              | Runs nonsystem software, like applications   | Manages system resources and enforces restrictions |
| Security and stability | Less critical for operations and less consequence for errors | Critical for system operations but larger consequence for errors |

These two modes are implemented on processors using so called privilege rings (or protection rings)
It looks like this: 
https://www.techgeekbuzz.com/media/post_images/uploads/2021/10/How-the-Kernel-Architecture-is-related-to-Operating-System.png

A x86 processor supports 4 privilege rings, numbered from 0-3. Typically only 2-3 are actually used; user mode, kernel mode and if present the hypervisor.
Kernel is placed on privilege ring 0, user on 3. The hypervisor' placement depends on the CPU, if it supports x86 virtualization its on 0(it allows the guest os to run on it, without affecting other guests and the host), otherwise on 1 

### User Mode
User mode is an OS state with restricted access to system resources and hardware. 
It is less privileged than kernel mode and cannot execute operations that could compromise system stability or security.

When an application starts in user mode, the kernel creates an isolated process with its own virtual address space (VAS). 
This ensures the process can only access its allocated memory and cannot interfere with other user-mode processes. 
The kernel retains full access to every process’s memory, but under normal circumstances it will not modify it directly.

This separation ensures stability and security: if a process crashes, the failure is contained within that process and does not impact the rest of the system. 
When a process needs to perform privileged operations (e.g., access hardware or perform I/O), it must invoke a system call via the system call interface (SCI). 
The kernel then temporarily elevates execution to kernel mode, performs the task safely, and returns control to user mode.

#### Advantages
- Containing crashes to the application, as the process is isolated from others
- Applications cannot read or write other processes memory
- No direct access to system resources to block unauthorizes use
- System calls need to be issued to execute priviledged operations, further enhancing security measures

#### Disadvantages
- Mode switching is time-consuming and resource-intensive
- Low-level tasks (like direct I/O) cannot be done
- Application depends on the kernel to perform essential functions


### Kernel Mode
Kernel mode is the privileged and unrestricted mode. It has direct access to hardware, including CPU, memory, and I/O devices.
In the kernel mode memory isolation does not exist - all processes run in the same memory space. This can cause 

A user-space application can only enter kernel mode via a system call. System calls are validated and authorized by the kernel’s system call interface (SCI), and they exclusively allow the execution of the requested operation. 
Even though kernel mode itself has unrestricted access, this mechanism ensures that arbitrary or malicious code from user-space cannot be executed directly in kernel mode.
Once a system call is validated, the kernel temporarily switches the CPU to kernel mode to execute the requested operation. 
After the operation completes, control always returns to user mode. Only once back in user mode, new system calls can be issued

User-space applications cannot be permanently run in kernel mode, they will always return to user mode after a sys call. 
While the kernel mode is intended to be temporary and controlled; vulnerabilities or exploits can allow malicious code to run with full privileges.

#### Advantages 
 - Full access to resources and devices
 - Can directly manage CPU, memory and I/O devices

#### Disadvantages
 - No process isolation, all processes share the same memory space, bugs or crashes can corrupt the entire system
 - Vulnerabilities in a kernel driver can allow the entire system to be comprimised
 - Developing on this level is requires in-depth knowledge and does not allow for mistakes


## System Calls and Mode Transition



## Resource Management

## Memory Allocation / Management

#### Layers

https://www.informit.com/content/images/chap3_9780136820154/elementLinks/03fig01.jpg
<!-- 
Author: cturpn
File: kernel.md
Purpose: Documentation of the basic linux architecture to further understand the different components
Created: 2025-08-22
Edited: 2025-08-22
-->
