Virtualization involves the abstraction of physical computing resources such as hardware, software, storage, and network components. The aim is to make these resources available on a virtual level and to distribute them to different customers in a manner that is as flexible as it is demand-driven. This should ensure improved utilization of computer resources. The aim is to run applications on a system that is not supported by it. In virtualization, we distinguish between:

-   Hardware virtualization
-   Application virtualization
-   Storage virtualization
-   Data virtualization
-   Network virtualization

Hardware virtualization is about technologies that enable hardware components to be made available independently of their physical basis using [hypervisor](https://en.wikipedia.org/wiki/Hypervisor) software. The best-known example of this is the `virtual machine (VM)`. A VM is a virtual computer that behaves like a physical computer, including hardware and operating system. Virtual machines run as virtual guest systems on one or more physical systems referred to as `hosts`.

![](https://www.tech-hounds.com/wp-content/uploads/2020/12/0115_hardware_virtualization_stack_showing_layers_ppt_slide_Slide01.jpg)


# Virtual Machines

A `virtual machine (VM)` is a virtual operating system that runs on a host system (an actual physical computer system). Several VMs isolated from each other can be operated in parallel. The physical hardware resources of the host system are allocated via `hypervisors`. This is a sealed-off, virtualized environment with which several guest systems can be operated, independent of the operating system, in parallel, on one physical computer. The VMs act independently of each other and do not influence each other. A hypervisor manages the hardware resources, and from the virtual machine's point of view, allocated computing power, RAM, hard disk capacity, and network connections are exclusively available.

VMs offer numerous advantages over running an operating system or application directly on a physical system. The most important benefits are:

1. Applications and services of a VM do not interfere with each other
2. Complete independence of the guest system from the host system's operating system and the underlying physical hardware
3. VMs can be moved or cloned to other systems by simple copying
4. Hardware resources can be dynamically allocated via the hypervisor
5. Better and more efficient utilization of existing hardware resources
6. Shorter provisioning times for systems and applications
7. Simplified management of virtual systems
8. Higher availability of VMs due to independence from physical resources

## VMware

VMware produces software products for the virtualization of computer systems. `VMware Workstation Pro` and `VMware Workstation Player` are the most interesting. The difference between the two is that, unlike Workstation Pro, Workstation Player can only run one VM at a time.

VMware produces software products for the virtualization of computer systems.

For both operating systems, Windows and Linux, the installation files can be downloaded [here](https://www.vmware.com/products/workstation-player/workstation-player-evaluation.html). 

