# Kernel

The kernel is the fundamental component of any operating system.  
As this core part of the OS' it always has the complete control over everything in the system.
The extend of the kernels tasks is very dependent on the type of kernel and the developers decisions, but in general it manages communication between hardware and software, providing controlled access to system resources such as CPU, memory, and I/O devices. 

Different operating systems implement different types of kernels.  
This documentation focuses primarily on the monolithic kernel(used by Linux), but many concepts also apply to other types, such as the hybrid kernel(used by Windows NT).

Check out the following diagram regarding the linux kernel mapping: 
https://upload.wikimedia.org/wikipedia/commons/thumb/5/5b/Linux_kernel_map.png/960px-Linux_kernel_map.png

## Core Kernel Architecture 

### Kernel Types
There are multiple types of kernels. Linux for example has a kernel of the "monolithic" type, Windows NT on the other hand a "hybrid" one.
Every type has its advantages and disadvantages that fit their specific usecase.

Important to know, that there is a term "OS Architecture" which is often used interchangably with the kernel types. 
Altough they are connected to each other(e.g a OS with a monolithic kernel, will also have a monolithic OS architecture), they dont exactly stand for the same thing.
The OS architecture stands for the entire OS structure - while the kernel type for the kernel specifically.

If you are interested in a overview of operating systems and the kernel they use, check this out:
https://en.wikipedia.org/wiki/Comparison_of_operating_system_kernels

Additionally the following diagram gives a visualization of the 3 main types of kernels:
https://en.wikipedia.org/wiki/Monolithic_kernel#/media/File:OS-structure2.svg


#### Monolithic 
In a monolithic kernel, all core system services and functions run within a single, protected memory region called kernel space. 
This memory is reserved and inaccessible to user-mode processes, ensuring system stability and security. 
Within kernel space, a virtual interface to the hardware is established. This interface abstracts hardware-specific details — such as memory layouts, I/O registers, and device behavior—through kernel-resident drivers. 
User-mode applications and processes interact with this interface via system calls, allowing them to perform operations on the hardware without accessing it directly. 

Although the monolithic kernel is entirely running in the kernel space, it is designed in layers. 
These are arranged in a logical order, starting with the most fundamental operations, like process management to the interfaces that are exposed to user applications such as the system calls interface (SCI)

##### Advantages
 - Generally faster than other types of kernels, because the system services dont need to switch mode for every system call.
 - More efficient communication between system services, as they all coexist in the kernel space
 - Less latency as the system calls and interrupts can be handled by the kernel directly
 - Due to the unified structure simpler to design, implement and debug


##### Disadvantages
 - The size of the kernel itself is bigger than other types of kernel
 - A single system service crashing causes the entire to crash
 - All system services running in kernel mode causes the system to be more prone to vulnerabilities
 - Changes on a single service can affect the entire system


#### Microkernel
The microkernel is a type of kernel that contains only the bare minimum of system services in its kernel space. 
The goal of the microkernel is, to provide a core system that ensures vital functionalities and keep all non-vital components as modular as possible. While it still offers a similar virtual interface to the hardware
Therefore only the most important system services make it into the kernel space, such as the inter-process communication(IPC), memory management and CPU-scheduling. This can vary depending on the type of system.
All non-critical services are installed in the user space as isolated processes called "servers". This approach allows the user to directly interact with them and install, update or replace indiviual services without affecting the entire system.
 
This design approach enables the system to be more stable and less prone to crashes, as a crashing/faulty server will not affect any other processes.

The servers communicate with each other via inter-process communication(IPC), specifically message passing. 
Microkernel servers are essentially daemon programs like any others, except that the kernel grants some of them privileges to interact with parts of physical memory that are otherwise off limits to most programs. This allows some servers, particularly device drivers, to interact directly with hardware. 


##### Advantages
 - More secure / less vulnerable, because only the most important system services run in the kernel space
 - More stable and less prone to full system crashes
 - The kernel size, is like the name suggest, a lot smaller than a monolithic kernel for example
 - The microkernel is very modular due to the services/servers running separated from others and in user-mode
 - Development is way easier and faster compared to a monolithic kernel


##### Disadvantages
 - Performance is lower due to the many system calls and IPC being done by the various user-mode servers/services
 - Including all services the microkernel architecture requires more code than a monolithic kernel


#### Hybrid 
This is, like the name suggests, the combination of both monolithic and microkernel. Its goal is to combine the efficiency and performance of the monolithic type with the microkernels modularity and stability.
While this approach has many advantages, it also has compromises: 
 - Security is not as high as in the microkernel, as most of the system services run in the kernel space. 
 - Performance is not as good as a monolithic kernel, as there are still important services running in user space.

Unlike monolithic and microkernels, there is no strict definition regarding which services run in kernel space versus user space. 
The hybrid kernel is more like a spectrum in between monolithic and microkernel. This allows developers to customize their kernel to the point where it perfectly fits their needs.

##### Advantages
 - As there is no strict definition, the hybrid kernel allows for full customization by the developer to fit their system requirements
 - Offers a middle ground between the two extremes


##### Disadvantages
 - While it combines the advantages of monolithic and microkernels, it does so at reduced effectiveness
 - Implementing advantages from either type of kernel inevitably brings corresponding disadvantages
 - Hybrid kernels are very complex in their design and implementation, and therefore require more knowledge to develop and maintain them


#### Nanokernel
Today, the microkernel and nanokernel are often used interchangeably, since both aim to minimize what runs in the kernel space.
Historically, however, the nanokernel referred to a even more extreme version of the microkernel, keeping only the most critical services(like interrupt handling and context switching) in the kernel space. 

#### Exokernel
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

### Kernel Modularity
Many kernels, such as Windows and Linux, support loadable kernel modules(LKM). Kernel modules are executable libraries used to extend a kernels capabilities. They can be used to add support for additional hardware and filesystems or system calls. If a LKMs functionality is no longer required, they can simply be unloaded from the system to free up the memory space. 

While loadable kernel module is a common name, it might change depending on the OS. Windows for example, calls them kernel-mode drivers.

### User Mode vs Kernel Mode
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
Kernel is placed on privilege ring 0, user on 3. The hypervisor' placement depends on the CPU, if it supports x86 virtualization its on 0(it allows the guest os to run on it, without affecting other guests and the host), otherwise on 1.

#### User Mode / User Space
User mode is an OS state with restricted access to system resources and hardware. 
It is less privileged than kernel mode and cannot execute operations that could compromise system stability or security.
The term "user space" refers to the reserved area of system memory, where non-kernel programs run. Programs in user space are executed with user mode privileges.

When an application starts in user mode, the kernel creates an isolated process with its own virtual address space (VAS) in the user space. 
This ensures the process can only access its allocated memory and cannot interfere with other user-mode processes. 
The kernel retains full access to every process’s memory, but under normal circumstances it will not modify it directly.

This separation ensures stability and security: if a process crashes, the failure is contained within that process and does not impact the rest of the system. 
When a process needs to perform privileged operations (e.g., access hardware or perform I/O), it must invoke a system call via the system call interface. 
The kernel then temporarily elevates execution to kernel mode, performs the task safely, and returns control to user mode.

##### Advantages
- Containing crashes to the application, as the process is isolated from others
- Applications cannot read or write other processes memory
- No direct access to system resources to block unauthorizes use
- System calls need to be issued to execute privileged operations, further enhancing security measures

##### Disadvantages
- Mode switching is time-consuming and resource-intensive
- Low-level tasks (like direct I/O) cannot be done
- Application depends on the kernel to perform essential functions


#### Kernel Mode / Kernel Space
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

##### Advantages 
 - Full access to resources and devices
 - Can directly manage CPU, memory and I/O devices

##### Disadvantages
 - No process isolation, all processes share the same memory space, bugs or crashes can corrupt the entire system
 - Vulnerabilities in a kernel driver can allow the entire system to be comprimised
 - Developing on this level is requires in-depth knowledge and does not allow for mistakes


### System Calls
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

## System Resource Management

## Process Management
A process is a running instance of a program. The OS must allocate resources to processes and enable them to share and exchange information, while protecting the allocated resources from other processes. Additionally, the OS is responsible for secure synchronization among processes.
The OS maintains a data structure called the process control block(PCB) for each process, containing:
 - its state 
 - ownership 
 - assigned resources
 - scheduling priority
 - Parent process pointer
 - Process ID (PID)
 - Privileges

This data structure is the heart of process management. 
Process management is responsible for:
 - Creation & termination of processes
 - Scheduling, prioritizing & CPU allocation
 - Context switching
 - Resource tracking
 - Inter Process communication - explained in a seperate part, as it is quite a big topic


### Multiprogramming 
Most modern operating systems support multiprogramming(therefore ill focus on multiprogramming OS, unless mentioned). At any given point in time, a single CPU core can only execute instructions from one process/thread, but several of them over a period of time by assigning each process or thread to the processor at intervals. The other processes temporarily become inactive(state waiting or ready). 
The execution of multiple processes/threads over a period of time is known as concurrent execution. 
Multiprogramming is defined as "keeping several jobs in memory so the CPU always has one to execute". In practice, the term is often used interchangeably with concurrent execution, although strictly speaking, multiprogramming focuses on maximizing CPU utilization, while concurrent execution refers to the interval execution of multiple processes/threads.

A multiprogramming OS supports concurrent execution and therefore allows for many processes/threads to be executed concurrently. This requires the kernel (the part responsible for process management) to allocate and deallocate CPU cores at an appropriate moment. 
If the CPU core has to be de-allocated during execution of a process, it must be done in a way, that allows it to resume efficiently later on. 
There are 3 ways for this to happen: 
 - A system call, by the process currently being executed
 - A hardware interrupt, such as keyboard or mouse interrupts (e.g., Alt+F4)
 - A timer interrupt (preemption enforced by the scheduler)

The stopping, starting and restarting of another process is called a context switch (or context change). 
Typically, these switches happen every few ms, but can vary heavily by OS and workload.

### Process Scheduler & Dispatcher
Process scheduling is the act of removing and assigning processes from and to the CPU, as well as deciding the order in which processes are executed
The scheduler is essential for scheduling the processes CPU time according to priority and importance and ensuring, that the CPU time is spilt appropriately. Additionally, the scheduler provides security measures against malicious acts such as CPU DoS, by providing maximum time slices for processes. 

There are 3 types of process schedulers, these types are as follows: 
 - Long term or job scheduler - Job queue -> memory
 - Medium term scheduler - ready -> suspended
 - Short term or CPU scheduler - ready queue -> CPU via dispatcher

The long term scheduler determines which processes get admitted to the system for processing. It selects processes from the job queue and loads them into memory for execution. The main objective of the long term scheduler is to provide a equal amount of I/O bound and CPU bound processes. It also controls the level of multiprogramming, ensuring, that the amount of processes being created is the same as the ones leaving.
This scheduler, just like the job queue is absent in most modern OS's and its tasks are handled by the short term scheduler or the memory management respectively.

The medium term scheduler is part of swapping and therefore optional. Its job is to swap suspended / blocked processes to disk and it is responsible for managing the swapped processes.
A process gets switched when it requests a I/O operation, such as write(). This happens because the CPU is way faster than the I/O devices and doesnt wait for said operation. Therefore the process is moved to suspended / blocked and CPU executes a different process. If your system has enough RAM, this is not necessary, but on systems with low memory it can reduce the memory load and increase performance. 

The short term scheduler(also CPU scheduler) is responsible for actually scheduling CPU time and enforcing kernel process management policies(such as max time slices for processes).
It selects a process among the "ready for execution" processes and allocates CPU to it. Which one of the processes gets CPU time is defined by the scheduling algorithm and process priority. 

The process scheduler has, depending on the algorithm used, 2 types of scheduling:
 - Non-preemptive
 - Preemptive

Preemptive scheduling means that processes currently being executed in CPU can be switched out for a different process before finishing its execution. When this happens, the preemptively switched process gets put back into the ready queue and waits there. This can be caused by priority rules or time sharing policies.

The non-preemptive scheduling is the opposite. With this approach it is not possible to switch a process until it finishes execution. While this is simpler to implement, it is also quite easy to abuse by a malicious actor. Therefore it is rarely used in modern OS.


The dispatcher is directly connected to the short term scheduler and does the actual moving of processes(called context switch) selected by the scheduler. It moves the finished or stopped process back to the correct location or queue, saves information from the process that is about to be executed and moves it to CPU
Example: Short term scheduler selects process X for execution, the dispatcher then acts and moves it into CPU. Once the execution ends or the scheduler interrupts the execution for some reason, the dispatcher once again moves the process.


### Process Queues
This list is not exhaustive, it contains the most important ones. 

Job Queue
 - Definition: Contains all processes waiting to be admitted into main memory.
 - Where processes come from: Newly created jobs (user programs, batch jobs, system services).
 - Purpose: Ensures the system doesn’t overload memory — the long-term scheduler pulls processes from this queue.
 - Example: Opening a terminal places the process in the job queue before it is loaded into RAM.
 - Rare in modern OS, as processes are created dynamically and directly admitted into the ready queue.

Ready Queue
 - Definition: Holds all processes in main memory that are ready to execute on the CPU.
 - Purpose: Short-term scheduler picks processes from this queue to assign CPU time.
 - Characteristics: 
   - All processes here are waiting only for CPU availability.
   - Can grow if CPU is busy or if many processes are in memory.

Device / Block Queue
 - Definition: Holds processes waiting for I/O operations (disk read/write, network, user input).
 - Purpose: Ensures CPU doesn’t waste cycles on processes blocked by I/O.
 - Characteristics: There can be a device queue for each I/O device available.
 - Example: A text editor waiting for disk save or a download waiting for network packets.

### Scheduling Algorithms
There are multiple scheduling disciplines or algorithms. They define how and in which order processes are scheduled, its basically the ruleset used by the scheduler to schedule the processes. You mightve heard of these algorithms before, as they are not exclusive to process scheduling, but rather widely used for a range of different tasks.
The main purpose of a scheduling algorithm is avoiding resource starvation and ensuring that CPU time shared in a fair manner. 
Once again - these are only a few of these algorithms and therefore not exhaustive.

#### First Come, First Serve(FCFS) - First In, First Out(FIFO)
The simplest scheduling algorithm of the bunch. FIFO takes processes from the queue in the order they arrived in the ready queue, without any special priorization or rules.
This makes it quite fast and resource-friendly, but can cause important or short processes to wait for a long time.
FIFO avoids starvation for the most part, but multiple long processes can cause a starvation-like effect as the response time might become unacceptably long.
This algorithm does not support any preemptive actions.

#### Shortest Remaining Time First(SRTF) - Shortest Job First(SJF)
SJF picks the process with the shortest expected runtime. If a process is in execution and another shorter process arrives in the queue, the running process will preemptively stopped and put back in the queue. 
This causes additional context switches to happen and therefore introduces a small overhead. 
If the system is running on max or near max capacity, itll cause starvation of processes with longer execution time. 

#### Fixed-priority Pre-emptive Scheduling(FPPS)
FPPS causes the system to assign a fixed-priority rank to every process, and the scheduler arranges the processes in the ready queue in order of their priority. 
Lower-priority processes get interrupted by incoming higher-priority processes. 
This algorithm again causes a small overhead and can cause starvation under the same circumstances ad SJF - with the important difference of only starving low priority processes.

#### Round Robin Scheduling(RR)
The round robin approach assigns every process a fixed time unit. Then it uses the same logic as FIFO. 
After the process executes for the fixed amount of time, it is preemptively interrupted and rescheduled. If it finishes execution within defined time frame, it is terminated.
This algorithm causes a significant overhead due to the many context switches, but will never cause starvation.
It is something between FIFO and SJF; the longer the fixed time frame, the closer to FIFO and vice versa.


### Context Switching
A context switch is the action of saving a processes state and registers, stopping it from running and loading or reloading another one for execution. 
The trigger fo a context switch can be 3 different things:
 - The running process finishes execution 
 - The running process or the system sends a interrupt or system call. 
 - The system sends a interrupt to enforce preemtion policies
    - Max time slice reached
    - Higher priority process is ready

See the following diagram:
https://media.geeksforgeeks.org/wp-content/uploads/20250825182631440618/1223.webp

Its exclusively this saving, stopping and starting/reloading of processes - The selection of processes is not part of it.
Like previously mentioned, these switches happen every few ms. Linux usually single digit ms and windows mid double digits.


## Memory Management / Memory Management Unit (MMU)
This is the "part" of the kernel that manages all things memory. The memory management function keeps track of the status of each memory location - either free or allocated. 
It decides which process gets how much memory, for how long it gets it and when it does. It allocates and deallocates memory to processes, makes sure access is secure and abstracts the complex nature of physical memory to something called virtual memory.
But before tackling the virtual memory, we have to understand, how the physical system memory works.

### Physical Memory
Physical memory in a computer is always a limited resource. The hardware component providing this memory is the RAM stick. 
This physical memory is split into pages frames. The size of page frames is architecture specific. Usually the size is predefined (commonly 4KiB), but some architectures allow selection of the page size.
But not just the size of these pages depends on the architecture, how the addresses are actually mapped do as well.
The physical memory is not one big block of available memory space. It is fragmented into multiple blocks and looks more like this:
[0 - 640KB] usable
[640KB - 1MB] reserved
[1MB - 3GB] usable
[3GB - 4GB] reserved
[4GB - 16GB] usable

There are multiple reasons, why these reserved spaces exist such as firmware and I/O devices.

Physical memory, if utilized as is, would have some very impactful security flaws. One of them being, that processes would always see the entire address space, not just its own. This allows it malicious actors or bugs to bypass privilege rings, read and modify any part of memory and so on.  

All those given characteristics make the physical memory quite complex. It is common to reduce this complexity and introduce additional security measurements by utilizing different memory management techniques. 
One of these techniques is virtual memory (also paged memory management), which is the most common one in modern OS.
https://media.geeksforgeeks.org/wp-content/uploads/20250111161309142776/memory_management_techniques.webp
These techniques or rather the abstraction it provides, simplifies memory access for programs; the CPU handles all the physical complexity.

### Virtual Memory 
Main benefits of implementing virtual memory include:
 - Increased security due to process isolation
 - Allows swapping to be used, temporarily mapping more memory than physical RAM, though this is optional and not necessary if the computer has enough physical RAM
 - Hiding the memory fragmentation from programs, making development easier
 - The ability to share memory between certain processes (e.g libraries)

Like already mentioned, virtual memory is a abstraction of the physical memory to the programs. Each of these physical "page frames" can be mapped as one or multiple virtual "pages".
Virtual pages abstract the page frames into a single, continous virtual address space(VAS). 
These mappings are stored in a so called "page tables", which in turn allows for translation between virtual memory address and physical memory address.
The page tables are organized hierachially within the system and contain the physical address, virtual address, access rights and a present bit (displays whether page is in RAM). Virtual memory also allows the creation of two seperated areas, one for the kernel space and one for the user space.

When virtual memory is being utilized in a system, every program except the kernel itself uses a virtual address instead of the physical one. 
When a virtual address is being accessed, the memory management unit (MMU, special part of the CPU) translates this virtual address to its corresponding physical address.
The MMU manages page tables, handles page faults, performs page replacement and validates the access request to ensure security. 

Ive already mentioned process isolation in the benefits of virtual memory, but havent really explained why.
This is essentially how the kernel space and user space are mapped and seperated, how the MMU / CPU can detect and block processes attempting to access memory outside their own VAS and how the MMU grants controlled access to shared resources.
It is therefore, closely tied to the security and safety of the kernel. If it wasnt there, malware and malicious actors could access and write other processes memory or even the kernel space. Even just a bug in a software could corrupt the memory of another process.

While it has many upsides, it also has downsides. One of the downsides is that translating between the two addresses is quite resource and time intensive. To circumvent this overhead, the system reduces the amount of translations that have to be done. This is done by caching recently translated pages in something called a translation lookaside buffer (TLB).
A translation lookaside buffer (TLB) is a memory cache, storing recently translated pages in a table and has a fixed amount of slots for page table entries.
Modern systems commonly have multiple TLB's; One faster and smaller and one slower, but bigger. 
When translating a address, the MMU checks the TLB first and falls back to page tables if there are no related TLB entries.

### Memory Allocation Strategies
<--ToDo-->

### Swapping 
<--ToDo-->

## Device Management
The process of operating, maintaining and abstracting physical devices so user programs can access them in a uniform way is called device management. There are usually many devices connected to a system. 
To make these devices(mouse, keyboard, I/O devices etc.) available, the device management part of the kernel utilizes device specific drivers to connect with the devices.
Then it provides a interface to the OS, that allows programs to communicate with the devices. The device management selects, schedules and controls the requested I/O operations and ensures that they are executed efficiently.

### Device Drivers
<--ToDo-->

### I/O Management
The device drivers are what enables the kernel to talk to these devices and therefore allow it to provide a standardized api to the programs / processes. 
It is how the abstraction is provided(just a example, dont take the commands as guaranteed):
 - Programm wants to write on disk
 - Kernels system call interface exposes "write()" to do that
 - Programm sends write()
 - Inside the kernel, write() is mapped to device-specific operations (e.g., 0x_syswrite() for a Samsung 990 Pro NVMe SSD)
 - The kernel translates the request and sends the correct low-level command to the device. 
The device management is responsible for providing the device specific translations, allocate and deallocate devices(to processes similarly to process management with CPU time), monitoring the status of connected devices and storing/tracking all devices. 

Now that we know how they communicate, we need to look at the techniques the system utilizes to do this efficiently; Mainly buffering, caching and spooling

I/O operations are tasks that need to be executed efficiently. One key technique to do that is buffering. Buffering is the process of storing data temporarily in a reserved area of memory(called buffer), which makes the data more accessible than retrieving it directly from the SSD or respective source. 

### I/O Process scheduling
The allocation/scheduling of I/O devices is done based on process priority and a so called disk scheduling alorithm. Though, devices aren’t allocated to processes directly; rather, individual operations are scheduled and executed. 
Once an operation completes, the request is removed from the queue. Unlike CPU time, I/O operations typically run to completion without preemption. While this is being done, the actual process is paused/stopped as we discussed earlier.
The tracking and storing is done by a component called I/O controller. 

### Types of Devices
There are multiple types of devices, that can be connected to a system at any point. Actually, there are two different angles, we can look at it; From a allocation and access perspective, aswell how it handles data.
From the allocation and access perspective, we have:
 - Dedicated devices - a printer for example
 - Shared devices - e.g a HDD, SSD etc.
 - Virtual devices - a virtual device-like interface, that is not actually bound to any hardware device such as vNICs or a virtual router

Types based on how it handles data, we have the following:
 - Input devices - such as mouse, keyboard(for the most part)
 - Output devices - printers, headphones and such 
 - Character devices - serial ports, terminals
 - Block devices - HDD, SSD, USB etc.

Note, that devices can fit to multiple types of either perspective, but usually at least one of each fits.
Now lets go through them seperately:

#### Allocation and Access Types

Dedicated devices can only be used by a single process or user at a time. It does not support more than one process, therefore a operation needs to be finished before scheduling a new operation.
In case of a printer this would mean, that the print job must first be executed before starting another one. 

Shared devices are the opposite, they can be accessed by 2 or more processes concurrently. 
A SSD for example, can be accessed by multiple processes at once. Its the I/O controllers job to ensure that conflicting operations(such as two processes writing same file) do not corrupt data and security policies are enforced.

Virtual devices are, like previously mentioned virtual interfaces that are not bound to any hardware component. 
While the examples from before are correct, it can be extended on:
Virtual devices can be created to turn a dedicated device into a shared one. A common example for this would be the printer with its spooling program.
By converting a dedicated device to a shared device we can create many virtual instances of the current device and can increase its efficiency.


#### Data Handling Types

I/O devices are hardware that enables communication between the computer and the external world. Done by either sending/receiving data to/from the user or other systems
Input devices send inputs, commonly triggered by the user. Mouse, keyboard ect. all only send data to the system, while receiving none. There are some exceptions to this, for example modern programmable keyboards.

Output devices receive inputs from the system. It then typically provides this to the external world in a human-readable format.
Such devices are headphones, printers and so on.

Character devices are devices that perform I/O operations as a continous stream of bytes, one character at a time. 
Example of such would again be a keyboard or serial port. Every time you press a key, it directly sends this top the system, one event at a time.

Block devices do the same as the character devices, but in blocks(chunk of data, with a fixed size). 
This means I/O operations stay in RAM until they collectively reached a predefined size, or a timer forces it to flush. 
A SSD for example works like this, it doesnt write every single change immediately, but in mentioned blocks.

### Buffering, Caching and Spooling
<--ToDo-->

### Interrupt handling

## File System Interface
File System Interface (Kernel’s Role)

VFS (Virtual File System layer).

Inodes, file descriptors, system calls (open, read, write, close).

Journaling basics (ext4, NTFS, etc. at kernel abstraction level).

## Inter-Process Communication(IPC)
The kernel is responsible to provide a way for processes to communicate between each other. This is done via IPC
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
