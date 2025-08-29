# Kernel

The kernel is the fundamental component of any operating system.  
As this core part of the OS' it always has the complete control over everything in the system.
The extend of the kernels tasks is very dependent on the type of kernel and the developers decisions, but in general it manages communication between hardware and software, providing controlled access to system resources such as CPU, memory, and I/O devices. 

Different operating systems implement different types of kernels.  
This documentation focuses primarily on the monolithic kernel(used by Linux), but many concepts also apply to other types, such as the hybrid kernel(used by Windows NT).

Check out the following diagram regarding the linux kernel mapping: 
https://upload.wikimedia.org/wikipedia/commons/thumb/5/5b/Linux_kernel_map.png/960px-Linux_kernel_map.png

## Kernel Types
There are multiple types of kernels. Linux for example has a kernel of the "monolithic" type, Windows NT on the other hand a "hybrid" one.
Every type has its advantages and disadvantages that fit their specific usecase.

Important to know, that there is a term "OS Architecture" which is often used interchangably with the kernel types. 
Altough they are connected to each other(e.g a OS with a monolithic kernel, will also have a monolithic OS architecture), they dont exactly stand for the same thing.
The OS architecture stands for the entire OS structure - while the kernel type for the kernel specifically.

If you are interested in a overview of operating systems and the kernel they use, check this out:
https://en.wikipedia.org/wiki/Comparison_of_operating_system_kernels

Additionally the following diagram gives a visualization of the 3 main types of kernels:
https://en.wikipedia.org/wiki/Monolithic_kernel#/media/File:OS-structure2.svg


### Monolithic 
In a monolithic kernel, all core system services and functions run within a single, protected memory region called kernel space. 
This memory is reserved and inaccessible to user-mode processes, ensuring system stability and security. 
Within kernel space, a virtual interface to the hardware is established. This interface abstracts hardware-specific details — such as memory layouts, I/O registers, and device behavior—through kernel-resident drivers. 
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
The goal of the microkernel is, to provide a core system that ensures vital functionalities and keep all non-vital components as modular as possible. While it still offers a similar virtual interface to the hardware
Therefore only the most important system services make it into the kernel space, such as the inter-process communication(IPC), memory management and CPU-scheduling. This can vary depending on the type of system.
All non-critical services are installed in the user space as isolated processes called "servers". This approach allows the user to directly interact with them and install, update or replace indiviual services without affecting the entire system.
 
This design approach enables the system to be more stable and less prone to crashes, as a crashing/faulty server will not affect any other processes.

The servers communicate with each other via inter-process communication(IPC), specifically message passing. 
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
This is, like the name suggests, the combination of both monolithic and microkernel. Its goal is to combine the efficiency and performance of the monolithic type with the microkernels modularity and stability.
While this approach has many advantages, it also has compromises: 
 - Security is not as high as in the microkernel, as most of the system services run in the kernel space. 
 - Performance is not as good as a monolithic kernel, as there are still important services running in user space.

Unlike monolithic and microkernels, there is no strict definition regarding which services run in kernel space versus user space. 
The hybrid kernel is more like a spectrum in between monolithic and microkernel. This allows developers to customize their kernel to the point where it perfectly fits their needs.

#### Advantages
 - As there is no strict definition, the hybrid kernel allows for full customization by the developer to fit their system requirements
 - Offers a middle ground between the two extremes


#### Disadvantages
 - While it combines the advantages of monolithic and microkernels, it does so at reduced effectiveness
 - Implementing advantages from either type of kernel inevitably brings corresponding disadvantages
 - Hybrid kernels are very complex in their design and implementation, and therefore require more knowledge to develop and maintain them

### Nanokernel
Today, the microkernel and nanokernel are often used interchangeably, since both aim to minimize what runs in the kernel space.
Historically, however, the nanokernel referred to a even more extreme version of the microkernel, keeping only the most critical services(like interrupt handling and context switching) in the kernel space. 

### Exokernel
The exokernel pushes the boundaries of minimalism, allowing applications direct access to the hardware resources such as CPU, memory and I/O devices. 
https://upload.wikimedia.org/wikipedia/commons/f/f2/Exokernel_revised%28english%29.png

Exokernels are tiny in size, since functionality is reduced to allocating and multiplexing hardware resources - managing when and how an application accesses CPU, memory and I/O devices.
The kernel only ensures that the requested resource is free, and the application is allowed to access it.

The exokernel forces a minimal amount of abstractions on the application developer.
The kernel-provided abstractions are called library operating systems (LibOS). They request specific memory addresses, disk blocks, etc.
Unlike traditional kernel types, which enforce strict abstraction interfaces(Like System Call Interface), the exokernels library operating system allows application-specific custom abstractions. 
This approach makes exokernels very flexible and modular, while keeping performance up. It allows applications to handle these resources with greater precision/efficiency and requires less work from the kernel. 
On the contrary it also shifts responsibility(especially security) and complexity to the developer. 
If this responsibility is not taken seriously, it turns the exposure of hardware resources into an inherent security risk. 
This is due to the reduced amount of abstractions forced onto the applications and less isolation layers to go through, making it easier for bugs and malicious behavior to cause damage to the system. 




## User Mode vs Kernel Mode
To fully understand the aforementioned advantages and disadvantages of the different kernel types, it is important to know the two key states in a operating system. 
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

### User Mode / User Space
User mode is an OS state with restricted access to system resources and hardware. 
It is less privileged than kernel mode and cannot execute operations that could compromise system stability or security.
The term "user space" refers to the reserved area of system memory, where non-kernel programs run. Programs in user space are executed with user mode privileges.

When an application starts in user mode, the kernel creates an isolated process with its own virtual address space (VAS) in the user space. 
This ensures the process can only access its allocated memory and cannot interfere with other user-mode processes. 
The kernel retains full access to every process’s memory, but under normal circumstances it will not modify it directly.

This separation ensures stability and security: if a process crashes, the failure is contained within that process and does not impact the rest of the system. 
When a process needs to perform privileged operations (e.g., access hardware or perform I/O), it must invoke a system call via the system call interface. 
The kernel then temporarily elevates execution to kernel mode, performs the task safely, and returns control to user mode.

#### Advantages
- Containing crashes to the application, as the process is isolated from others
- Applications cannot read or write other processes memory
- No direct access to system resources to block unauthorizes use
- System calls need to be issued to execute privileged operations, further enhancing security measures

#### Disadvantages
- Mode switching is time-consuming and resource-intensive
- Low-level tasks (like direct I/O) cannot be done
- Application depends on the kernel to perform essential functions


### Kernel Mode / Kernel Space
Kernel mode is the privileged and unrestricted mode. It has direct access to hardware, including CPU, memory, and I/O devices.
In the kernel mode memory isolation does not exist - all processes run in the same memory space. This can cause problems in case of a service failing, as it will affect the entire system.
For this reason and additional security concerns, exclusively OS relevant programs and services run in the kernel mode.
Kernel space is the kernel equivalent of the user space, referring to the area of system memory, where all the kernel programs run. Logically, programs in kernel space are executed with kernel mode privileges.

A user-space application can only temporarily enter kernel mode via a system call. System calls are validated and authorized by the kernels system call interface, and they exclusively allow the execution of the requested operation. 
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


## System Calls
System calls are the mechanism by which a user space program requests actions from the kernel. They are a way for software to temporarely switch to privilege ring 0 and execute operations, they are otherwise cant(such as memory management or process control).
There are a lot of system calls for different tasks and operations. Each one is restricted to its own task, only allowing specific arguments and parameters. 
https://media.geeksforgeeks.org/wp-content/uploads/20231017212555/Types-of-System-Calls-(3)-(2).png

These system calls are invoked by a special instruction "syscall" known to the kernel. Which system call a program wants to invoke, is defined by a argument (the system calls number) in the syscall instruction. This instruction is the entry point into the kernel and is also executed there. 
System calls are not functions, but architecture and kernel specific assembly instructions. 
Their tasks are:
 - setup information to identify the system call and its paramenter
 - trigger a kernel mode switch
 - retrieve the result of the system call 
 - return the process to user mode

System calls are located in the kernel itself, more specifically in their respective file. Additionally there is a table called "system call table", this is used to correlate numbers to a specific identified by numbers.
During runtime, the system call table resides in the kernel memory and is used by the system call dispatcher to resolve system call numbers. 
The system calls numbers are made available to programs running in user space via the system call interface. 
Looks like this:
https://github.com/torvalds/linux/blob/16f73eb02d7e1765ccab3d2018e0bd98eb93d973/arch/x86/entry/syscalls/syscall_64.tbl

While system calls can be invoked directly, it is rarely done. Usually system libraries are used to call the system calls (also called library call). This is because of the following reasons:
 - It makes it easier for developer to communicate with the kernel and request operations, by providing an interface with unified requests
 - It indirectly reduces the work, the kernel has to do, because the functions already validates the args before making a system call 

A somewhat simplified write system call would look like this:
 - User program calls a function fwrite() → library wrapper in user space prepares args and number
 - Library executes fwrite(), a function that calls the generic syscall instruction.
    - Executes the syscall instruction, which signals the CPU core to switch to kernel mode
    - Supplies system call number(in our case 1 for write), arguments and parameters and pre-validates them
 - CPU acts
    - Switches mode to kernel by moving stack pointer of acting core to kernel memory
    - Saves process state needed to return after the system call
 - System call dispatcher
    - Validates arguments
    - Translates system call number to sys_write() (or whatever is assigned to provided number) with the help of the sys_call_table
    - Calls sys_write() (the handler)
 - Handler executes - performs write
 - Return to user space
    - Takes return value from handler
    - Restores saved process state
    - CPU core switches back to user mode and resumes execution

A few takeaways regarding my example: 
 - When talking about mode switching, it is always a single core, not the entire CPU. 
 - A program always returns to user mode after execution. There are no exceptions to this.
 - System calls are only executed AFTER their arguments are validated. This is done to ensure security and block malicious actions.
 - Libraries are not necessary, but required by most modern programs.

If you are interested in a more indepth documentation, please refer to:
https://0xax.gitbooks.io/linux-insides/content/
This documentation was invaluable for my writeup!

## Resource Management

### Memory Management



## I/O Devices

## Inter-Process Communication(IPC)
Inter-process communication, from now on IPC describes a mechanism that allows seperate processes to communicate with each other by sending messages.
Shared memory, such as the kernel space, is also considered a inter-process communication mechanism, but the abbreviation IPC usually refers to "message passing" only. 
https://media.geeksforgeeks.org/wp-content/uploads/1-76.png

The message passing part of IPC is especially important, when talking about microkernels. 
IPC is the mechanism that allows the system to be built from a number of smaller programs called servers, which are used by other programs on the system, invoked via IPC. 
Usually most or even all support for peripheral devices is provided this way - with servers for device drivers, network protocol stacks, file systems etc.

There are two types of IPC: synchronous and asynchronous. Asynchronous IPC is comparable to network communication: The sender dispatches a message and continues working as normal. The receiver checks for the messages, or is alerted by some mechanism.
For asynchronous IPC to function, it requires that the kernel maintain buffers and queues messages. The kernel is additionally responsible for dealing with buffer overflows and to copy messages(first copy from sender to kernel buffer, second from kernel buffer to receiver)
In synchronous IPC, the first party(sender or receiver) blocks/waits until the other party is ready to perform the IPC. It requires neither buffering nor copying of messages. 
While the following diagram was created in the context of asynchronous/synchronous programming, the same logic applies here:
https://refine.ams3.cdn.digitaloceanspaces.com/blog/2024-02-16-async-vs-sync/diagram.png

The advantage of synchronous IPC lies in its lower overhead, since it avoids buffering and extra message copies, whereas asynchronous IPC is more efficient in practice, as the sender can continue execution without waiting for the receiver.


#### Layers

https://www.informit.com/content/images/chap3_9780136820154/elementLinks/03fig01.jpg
<!-- 
Author: cturpn
File: kernel.md
Purpose: Documentation of the basic linux architecture to further understand the different components
Created: 2025-08-22
Edited: 2025-08-22
-->
