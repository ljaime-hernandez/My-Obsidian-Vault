## History

Many events led up to creating the first Linux kernel and, ultimately, the Linux operating system (OS), starting with the Unix operating system's release by Ken Thompson and Dennis Ritchie (whom both worked for AT&T at the time) in 1970. The Berkeley Software Distribution (BSD) was released in 1977, but since it contained the Unix code owned by AT&T, a resulting lawsuit limited the development of BSD. Richard Stallman started the GNU project in 1983. His goal was to create a free Unix-like operating system, and part of his work resulted in the GNU General Public License (GPL) being created. Projects by others over the years failed to result in a working, free kernel that would become widely adopted until the creation of the Linux kernel.

At first, Linux was a personal project started in 1991 by a Finnish student named Linus Torvalds. His goal was to create a new, free operating system kernel. Over the years, the Linux kernel has gone from a small number of files written in C under licensing that prohibited commercial distribution to the latest version with over 23 million source code lines (comments excluded), licensed under the GNU General Public License v2.

Linux is available in over 600 distributions (or an operating system based on the Linux kernel and supporting software and libraries). Some of the most popular and well-known being Ubuntu, Debian, Fedora, OpenSUSE, elementary, Manjaro, Gentoo Linux, RedHat, and Linux Mint.

Linux is generally considered more secure than other operating systems, and while it has had many kernel vulnerabilities in the past, it is becoming less and less frequent. It is less susceptible to malware than Windows operating systems and is very frequently updated. Linux is also very stable and generally affords very high performance to the end-user. However, it can be more difficult for beginners and does not have as many hardware drivers as Windows.

Since Linux is free and open-source, the source code can be modified and distributed commercially or non-commercially by anyone. Linux-based operating systems run on servers, mainframes, desktops, embedded systems such as routers, televisions, video game consoles, and more. The overall Android operating system that runs on smartphones and tablets is based on the Linux kernel, and because of this, Linux is the most widely installed operating system.

Linux is an operating system like Windows, iOS, Android, or macOS. An OS is software that manages all of the hardware resources associated with our computer. That means that an OS manages the whole communication between software and hardware. Also, there exist many different distributions (distro). It is like a version of Windows operating systems.

With the interactive instances, we get access to the Pwnbox, a customized version of Parrot OS. This will be the primary OS we will work with through the modules. Parrot OS is a Debian-based Linux distribution that focuses on security, privacy, and development.

## Philosophy

Linux follows five core principles:


| **Principle** | **Description** |
| --- | --- | 
| `Everything is a file` | All configuration files for the various services running on the Linux operating system are stored in one or more text files.
| `Small, single-purpose programs` | Linux offers many different tools that we will work with, which can be combined to work together.
| `Ability to chain programs together to perform complex tasks` | The integration and combination of different tools enable us to carry out many large and complex tasks, such as processing or filtering specific data results.
| `Avoid captive user interfaces` | Linux is designed to work mainly with the shell (or terminal), which gives the user greater control over the operating system.
| `Configuration data stored in a text file` | An example of such a file is the `/etc/passwd` file, which stores all users registered on the system.

## Components

| **Component** | **Description** |
| --- | --- |
| `Bootloader` | A piece of code that runs to guide the booting process to start the operating system. Parrot Linux uses the GRUB Bootloader.
| `OS Kernel` | The kernel is the main component of an operating system. It manages the resources for system's I/O devices at the hardware level.
| `Daemons` | Background services are called "daemons" in Linux. Their purpose is to ensure that key functions such as scheduling, printing, and multimedia are working correctly. These small programs load after we booted or log into the computer.
| `OS Shell` | The operating system shell or the command language interpreter (also known as the command line) is the interface between the OS and the user. This interface allows the user to tell the OS what to do. The most commonly used shells are Bash, Tcsh/Csh, Ksh, Zsh, and Fish.
| `Graphics server` | This provides a graphical sub-system (server) called "X" or "X-server" that allows graphical programs to run locally or remotely on the X-windowing system.
| `Window Manager` | Also known as a graphical user interface (GUI). There are many options, including GNOME, KDE, MATE, Unity, and Cinnamon. A desktop environment usually has several applications, including file and web browsers. These allow the user to access and manage the essential and frequently accessed features and services of an operating system.
| `Utilities` | Applications or utilities are programs that perform particular functions for the user or another program.

## Linux Architecture

The Linux operating system can be broken down into layers:

| **Layer** | **Description** |
| --- | --- |
| `Hardware` | Peripheral devices such as the system's RAM, hard drive, CPU, and others.
| `Kernel` | The core of the Linux operating system whose function is to virtualize and control common computer hardware resources like CPU, allocated memory, accessed data, and others. The kernel gives each process its own virtual resources and prevents/mitigates conflicts between different processes.
| `Shell` | A command-line interface (**CLI**), also known as a shell that a user can enter commands into to execute the kernel's functions.
| `System Utility` | Makes available to the user all of the operating system's functionality. |

## File System Hierarchy

The Linux operating system is structured in a tree-like hierarchy and is documented in the [Filesystem Hierarchy](http://www.pathname.com/fhs/) Standard (FHS). Linux is structured with the following standard top-level directories:

![iamge](https://academy.hackthebox.com/storage/modules/18/NEW_filesystem.png)

| **Path** | **Description** |
| --- | --- |
| `/` | The top-level directory is the root filesystem and contains all of the files required to boot the operating system before other filesystems are mounted as well as the files required to boot the other filesystems. After boot, all of the other filesystems are mounted at standard mount points as subdirectories of the root.
| `/bin` | Contains essential command binaries.
| `/boot` | Consists of the static bootloader, kernel executable, and files required to boot the Linux OS.
| `/dev` | Contains device files to facilitate access to every hardware device attached to the system.
| `/etc` | Local system configuration files. Configuration files for installed applications may be saved here as well.
| `/home` | Each user on the system has a subdirectory here for storage.
| `/lib` | Shared library files that are required for system boot.
| `/media` | External removable media devices such as USB drives are mounted here.
| `/mnt` | Temporary mount point for regular filesystems.
| `/opt` | Optional files such as third-party tools can be saved here.
|`/root` | The home directory for the root user.
| `/sbin` | This directory contains executables used for system administration (binary system files).
| `/tmp` | The operating system and many programs use this directory to store temporary files. This directory is generally cleared upon system boot and may be deleted at other times without any warning.
|`/usr` | Contains executables, libraries, man files, etc.
| `/var` | This directory contains variable data files such as log files, email in-boxes, web application related files, cron files, and more.
