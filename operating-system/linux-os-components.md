# Linux OS Components
All things regarding the "basic" Linux OS Components.
For more in-depth documentation check /components

## Components
The components are the parts that work together and when combined are called operating system.
Although there are multiple architectures that define how these components work together, the 5 listed components stay mainly the same. 

First take a look at the following diagram:
https://media.geeksforgeeks.org/wp-content/uploads/20230918130118/linux.jpg

or also this one:
https://media.geeksforgeeks.org/wp-content/uploads/20230918140328/shell.png

It shows the components of the linux system, such as:

 -  Hardware
 -  Kernel
 -  Shell 
 -  Applications
 -  System utilities

## Hardware
The bottom-most layer consisting of the physical components like CPU, GPU, I/O devices, network interfaces and peripherals. 
Provides the basic computing resources that the OS and kernel use to execute operations, manage components, and abstract functionality for applications and commands.

Not much else regarding Hardware, except 1-2 Points regarding CPU/GPU choice: 
 -  nowadays CPU brand (Intel / AMD) does not matter, they both do well.
 -  In the past AMD was preferred, though over the recent years this has changed. 
    AMD works great out of the box, Nvidia requires you to install their proprietary drivers.

## Kernel
The kernel is the core of the OS. While all components are important, the kernel allows them to function.
All components communicate in one way or another with the kernel:

https://upload.wikimedia.org/wikipedia/commons/thumb/8/8f/Kernel_Layout.svg/250px-Kernel_Layout.svg.png

The kernel is responsible for:

 -  Memory management
 -  Resource allocation
 -  Device management
 -  Process management
 -  Application interaction
 -  Security

The kernel receives commands and inputs from the shell, utilities as well as applications and talks directly with the hardware to execute operations and manage resources

## Shell
While technically not true, you can imagine it as the direct interface to the kernel.
There are many types of shells in Linux (e.g., bash, zsh, fish, ksh), each with different features, syntax extensions, and usability quirks.
Functionally, however, all shells perform the same core task.

Another important distinction is shell vs terminal: 
The shell is the software that interprets and executes commands, while the terminal is the window that hosts the shell and is often highly customizable.

## System Utility
System utilities are commandline tools that perform various tasks to make administration and system management easier. 
Most distros come with preinstalled system utilities, but the system is fully extensible, allowing the user to install additional tools.




<!-- 
Author: cturpn
File: linux_architecture.md
Purpose: Documentation of the basic linux architecture to further understand the different components
Created: 2025-08-22
Edited: 2025-08-22
-->

