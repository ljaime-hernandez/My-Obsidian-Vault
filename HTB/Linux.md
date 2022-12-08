Linux is the most widely used Operating System for penetration testing purposes. Therefore, we must be very familiar with it. If we have to prepare an operating system, it is best to develop a certain standard for it that always leads to the same setup we are used to.

## Penetration Testing Distributions

There are many different distributions that we can use, all of which have different advantages and disadvantages. Many people ask themselves which system is the best. Nevertheless, what many do not understand is that it is a personal decision. This means that it depends a lot on our own needs and desires, which is the best for us. The tools installed on these operating systems can be installed on pretty much any distribution, so we should instead ask ourselves what we should expect from this Linux distribution for penetration testing.
In this case, we will deal with [ParrotOS Security](https://www.parrotsec.org/download/) as our penetration testing distribution of choice.

### VM Setup


1. `create a VM` (in VMware Workstation in this example). Here we also specify which installation file will be used for the operating system (`.iso` file).
2. Since VMware does not know all operating systems, it may be that the system will not be recognized. Therefore, we have to specify which distribution the system we want to install is based upon in the next step. In the case of ParrotOS, this is a `Debian`-based operating system.
3. After that, we assign a `name` for the VM with the label we want to have for it and then set the `path` where it will be stored.
4. we can set the `maximum size` of the VM. Here it is recommended to set the size larger than 20 GB because the VM will not be equal to the size we set but will grow with the packages and applications we install during setup and regular usage. It is also recommended to save the VM in a `single file` and not split it into several to make moving the VM easier in the future if necessary.

### ParrotOS Installation

Once we have created our VM, we can start it and get to the GRUB menu to select our options. Since we want to `install ParrotOS`, we should select this option. Once we click on the VM window, our mouse will be trapped there. This means that our mouse cannot leave this window. To move the mouse freely again, we have to press `[CTRL] + [ALT]` (in most cases). However, we should verify this key combination in the VMware Workstation window under `Edit > Preferences > Hot Keys`.

Since we want to encrypt our data and information on the VM using [Logical Volume Manager](https://en.wikipedia.org/wiki/Logical_volume_management) (`LVM`), we should select the "`Encrypt system`" option. It is also recommended to create a `Swap (no Hibernate)` for our system.

`LVM` is a partitioning scheme mainly used in Unix and Linux environments, which provides a level of abstraction between disks, partitions, and file systems. Using LVM, it is possible to form dynamically changeable partitions, which can also extend over several disks. After that we will be asked to specify our username, hostname and a password.

### LUKS Encryption

`LVM` is an additional abstraction layer between physical data storage and the computer's operating system with its logical data storage area and the file system. LVM supports the organization of logical volumes in a RAID array to protect computers from individual hard disk failure. Unlike RAID, however, the LVM concept does not provide redundancy. It has been present in almost all Unix and Linux distributions but also for other operating systems. Windows or macOS also have the concept of LVM but use different names for it like [Storage Spaces](https://docs.microsoft.com/en-us/windows-server/storage/storage-spaces/overview) (Windows) or [CoreStorage](https://en.wikipedia.org/wiki/Core_Storage) (macOS).

Once we get to the partition disk step, we will be asked for an `encryption passphrase` for encryption and decryption. We should keep in mind that `this passphrase should be very strong`, and we should use a password manager of our choice to store it. We will then have to enter this passphrase again to make sure that we did not make a mistake when we first entered it.

### Updates & APT Package Manager

Now that we have installed the operating system, we need to bring it up to date. For this, we will use the `APT` package management tool. The `Advanced Packaging Tool` (`APT`) is a package management system that originated in the Debian operating system that uses `dpkg` for actual package management. The package manager is used for package management. This means that we can search, update, and install program packages. APT uses repositories (thus package sources), which are deposited in the directory `/etc/apt/sources.list` (in our case for ParrotOS: `/etc/apt.sources.list.d/parrot.list`).

#### ParrotOS Sources List
```bash
┌─[kopa@parrot]─[~]
└──╼ $`cat /etc/apt.sources.list.d/parrot.list`.
```

Here the package manager can access a list of HTTP and FTP servers and obtain and install the corresponding packages from there. If packages are searched for, they are automatically loaded from the list of available repositories. Since program versions can be compared quickly under APT and can be loaded automatically from the repositories list, updating existing program packages under APT is relatively easy and comfortable.

#### Updating ParrotOS

```bash
┌─[kopa@parrot]─[~]
└──╼ $`sudo apt update -y && sudo apt full-upgrade -y && sudo apt autoremove -y && sudo apt autoclean -y                                          
```

Then we can install the most necessary tools we need for our penetration tests. Here it is recommended to create a list of tools to simplify our automation process (note that many of these come pre-installed in distros such as Parrot).

#### Tools List

