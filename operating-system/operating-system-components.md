# Operating System Components
This document is intended to be a easy to understand, rather simple documentation regarding the different components of a operating system. 
While most informations are based on linux, they often apply to other operating systems as well. Some components have additional comments regarding the windows operating system.

For more in-depth documentation check /components

## Components
The components are the parts that work together and when combined are called operating system. 
Although there are multiple OS architectures, kernel architectures and further variations of design approaches, the main components of an operating system stay mostly the same. 
I will split them into three categories: "is necessary" and "optional, but important" and "optional". 
Take the examples with a grain of salt - its to help visualize how important each of the components is and show that a OS is not a strict defined term.

### Necessary components are:
 - Hardware
 - Kernel
 - Shell

While minimal and inconvenient, the system can boot and run with just these components. You could run shell built in commands, but even this is not guaranteed, as most modern shells still rely on libraries to execute them.

### Optional, but important components are:
 - System libraries
 - System utilities and services 

With these things installed you still wouldnt have a gui, but a proper commandline with possibilities to install further system utilities, applications and start programming.
Although, if you were to install firefox or any gui application, it wouldnt launch. To launch a gui application, we would need at display server and a window manager(Like Wayland & Hyprland)

### Optional
 - Graphical / UI Layer
 - Applications

## Hardware
The bottom-most layer consisting of the physical components like CPU, GPU, I/O devices, network interfaces and peripherals. 
Provides the basic computing resources that the OS and kernel use to execute operations, manage components, and abstract functionality for applications and commands.
The kernel communicates with hardware through drivers, which are part of the kernels device management subsystem.
Although not directly part of the OS itself, it is necessary to operate it.

Not much else regarding Hardware, except 1-2 Points regarding CPU/GPU choice: 
 -  nowadays CPU brand (Intel / AMD) does not matter, they both do well.
 -  In the past AMD was preferred, though over the recent years this has changed. 
    AMD works great out of the box, Nvidia requires you to install their proprietary drivers.

## Kernel
The kernel is the core of the OS. While all components are important, the kernel allows them to function.
All components communicate in one way or another with the kernel:

https://upload.wikimedia.org/wikipedia/commons/thumb/8/8f/Kernel_Layout.svg/250px-Kernel_Layout.svg.png

The kernel is responsible for:
 - Memory management
 - Resource allocation
 - Device management
 - Process management
 - File system management
 - Security and access control
 - Inter-process communication

Simply put, the kernel receives commands and inputs from the shell, utilities as well as applications and talks directly with the hardware to execute operations and manage resources.
It translates the requests by applications and programs into operations the hardware can understand.

## Shell
The shell acts as a direct interface to the kernel.
There are many types of shells in Linux (e.g., bash, zsh, fish, ksh), each with different features, syntax extensions, and usability quirks.
Functionally, however, all shells perform the same core task.
The shell comes with some built in commands for basic navigation. These commands vary by shell and are documented in the shells respective wiki

Another important distinction is shell vs terminal: 
The shell is the software that interprets and executes commands, while the terminal is the window that hosts the shell and is often highly customizable.

## System Libraries
System libraries, also called shared libraries contain pre-written functions that allow programs to perform specific tasks. By using these libraries, developers dont need to write the same code repeatedly.
These libraries act as interface between programs and the kernel. 

While this might sound unimportant, many system utilities and programs are written in a way, where they depend on those libraries. 
These libraries are almost always installed automatically during OS installation.
Some OS specific library examples would be the "glibc" for Linux/Unix and the "Kernel32.dll" and "NTDLL.dll" for windows.

## System Utilities & Services
System utilities are user-invoked commandline tools that perform various tasks to make administration and system management easier.  
This includes things like the "ls" command. They often require aforementioned libraries.
Most distros come with preinstalled system utilities, but the system is fully extensible, allowing the user to install additional tools.

Services programs include daemons(background services), specifically init systems(e.g systemd), hardware managers(e.g udev) and loggers(e.g journald). 
Package managers are also part of this component.

## Graphical / UI Layer
There are multiple programs that are responsible for the UI. In linux this includes:
 - A display server
 - A window manager
 - A compositor

Additional UI objects, like a task bar, can be seperately installed. There is also the possibility to install a desktop enviroment(also "DE"), which is usually a finished graphical stack, preconfigured and ready to use.
This DE also contains mentioned programs and often more. 

Windows has this built into the OS. It utilizes the DWM(Desktop Window Manager) and Win32k to draw the UI.


<!-- 
Author: cturpn
File: linux_architecture.md
Purpose: Documentation of the basic linux architecture to further understand the different components
Created: 2025-08-22
Edited: 2025-08-22
-->

