# Operating System Fundamentals

## Software -> Thread Definitions
### Software
Software or applications, especially larger ones, often consists of multiple programs or modules. This is to simplify programming, allows for easier collaboration and enables independent updates. 
Theoretically this could look like this:
 Application
    -> Program for GUI
    -> Program for network communication
    -> Program for settings

Each program may run as its own process when the application is executed.
Together, these programs form the full software/application experience for the user.

### Program
A program is a set of instructions written in a programming language. These instructions can exist as source code (human-readable) or be compiled into machine code (executable). 
A program becomes a process when the operating system loads it into memory for execution.
See this illustration for clarity:
https://upload.wikimedia.org/wikipedia/commons/2/25/Concepts-_Program_vs._Process_vs._Thread.jpg

### Process
Process

A process is an instance of a computer program that is being executed by one or more threads. While a program is a set of instructions stored in a file, a process is the active execution of those instructions after being loaded into memory.

Opening the same program multiple times typically creates multiple process instances, each with its own resources.

A process usually consists of the following resources:

 - Executable code image – a copy of the program instructions in memory
 - Memory – often a region of virtual memory, including stack, heap, and data segments
 - OS descriptors – e.g., file descriptors on Unix or handles on Windows
 - Processor state – CPU registers, program counter, stack pointer, etc.
 - Security attributes – such as owner, permissions, and access controls

### Thread
Threads are single sequence streams of instructions within a process. A thread always belongs to exactly 1 process. 
In an operating system that supports multithreading (which is very common nowadays), a process can consist of many threads.
Threads of the same process share the process’s address space and resources, but each thread keeps its own execution context (registers, stack, thread-local data).

Visualization of such: 
https://media.geeksforgeeks.org/wp-content/uploads/18446744073709551615/multithreading-in-os.png

Multithreading allows for threads to run concurrently, which leads to increased responsiveness and performance. This is due to the fact, that multiple things can happen at the same time (such as saving and accepting further user input for example)

## Memory Basics
### Process Memory Layout
The memory layout of a process consists of a text-, a data-, a heap- and a stack segment. They are layered on top of each other in mentioned order from low to high. 

See the following:
https://media.geeksforgeeks.org/wp-content/uploads/20250122155858092295/Memory-Layout-of-C-Program.webp

### Text/Code Segment
The text segment is where the executable code is stored. It contains the compiled machine code of the instructions and functions of the process. 
This segment starts at the lowest virtual memory address in the assigned memory space and is usually read only. 

### Data Segment
This lays right above the text segment and contains static and global variables created by the program (programmer).

### Heap Segment
As you might expect, the heap segment is just above the data segment. This segment is used for dynamic memory allocation and is usually managed by functions such as malloc() and free().
The heap segment is shared by/with all shared libraries and dynamically loaded modules. 


### Stack Segment
The stack segment is used for local variables and function call management. When a function is called, a stack frame is generated to store local variables, function parameters and return addresses, which is then stored in this segment. 



<!-- 
Author: cturpn
File: os-fundamentals.md
Purpose: Documentation of the basic linux architecture to further understand the different components
Created: 2025-08-22
Edited: 2025-08-22
-->
