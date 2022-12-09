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

```shell-session
┌─[cry0l1t3@parrotos]─[~]
└──╼ $ cat tools.list

netcat
ncat
nmap
wireshark
tcpdump
hashcat
ffuf
gobuster
hydra
zaproxy
proxychains
sqlmap
radare2
metasploit-framework
python2.7
python3
spiderfoot
theharvester
remmina
xfreerdp
rdesktop
crackmapexec
exiftool
curl
seclists
testssl.sh
git
vim
tmux
```

For you to install them, type the command:

```shell-session
┌─[cry0l1t3@parrotos]─[~]
└──╼ $ sudo apt install netcat ncat (... etc)
```

As it might be tedious to install one by one and some of the dependencies might get updated every now and then, is best to create a document which you can manipulate as list and we can use the command below to install them all at once:

```shell-session
┌─[cry0l1t3@parrotos]─[~]
└──╼ $ sudo apt install $(cat tools.list | tr "\n" " ") -y
```

### Using Github

We will also come across tools that are not found in the repositories and therefore have to download them manually from Github. For example, we are still missing specific tools for Privilege Escalation and want to download the [Privilege-Escalation-Awesome-Scripts-Suite](https://github.com/carlospolop/privilege-escalation-awesome-scripts-suite). We can do that using the following command:

```shell-session
┌─[cry0l1t3@parrotos]─[~]
└──╼ $ git clone https://github.com/carlospolop/privilege-escalation-awesome-scripts-suite.git
```

### Snapshot

After installing all known and relevant packages and repositories, it is highly recommended to take a `VM snapshot`. In the following steps, we will make changes to specific configuration files. If we are not careful, this can make parts of the system or even the entire system unusable. We do not have to repeat all our previous steps, and we should now create a snapshot and name it "`Initial Setup`".

Nevertheless, `before` we create this snapshot, we should shut down the OS. This will significantly reduce the time required to create the snapshot. Otherwise, the snapshot will be taken from a running system that we will return to when we return to it.

If we break the system in some way while performing subsequent configuration steps we can revert back to a good, known, working copy. A snapshot (powered off) should be taken after every major configuration stage. It is also a good idea to periodically take a VM snapshot during a penetration test in case something goes wrong.

![](https://academy.hackthebox.com/storage/modules/87/vm_snapshot.png)

![](https://academy.hackthebox.com/storage/modules/87/vm_snapshot3.png)

### Terminal Adjustment

Now that we have created a snapshot and can work with our configurations, let us look at our terminal environment. Many different terminal emulators emulate the actual command line input we use to enter and execute the system's commands.

A very efficient alternative, which can also be used as an extension, is [Tmux](https://github.com/tmux/tmux/wiki). `Tmux` is a terminal multiplexer that allows creating a whole shell session with multiple windows and subwindows from a single shell window. As we know, started processes abort when the terminal session or SSH connection disappears. Tmux's console keeps the process alive by working with sessions. For example, if we are connected to a constantly running server in this way, we can close the terminal or shut down the computer on the local client without terminating the Tmux session. If we log back into the remote server via SSH, we can view the existing sessions and rejoin the desired session.

Another handy component that we should adapt to our needs is the Bash prompt. The [bashrcgenerator](http://bashrcgenerator.com/) makes it very easy for us to design our bash prompt the way we want it to be displayed. For our penetration tests, it is crucial to have the order of the given commands to configure our Bash prompt to display timestamps.

Customize Bash Prompt

```shell-session
┌─[cry0l1t3@parrotos]─[~]
└──╼ $ echo 'export PS1="-[\[$(tput sgr0)\]\[\033[38;5;10m\]\d\[$(tput sgr0)\]-\[$(tput sgr0)\]\[\033[38;5;10m\]\t\[$(tput sgr0)\]]-[\[$(tput sgr0)\]\[\033[38;5;214m\]\u\[$(tput sgr0)\]@\[$(tput sgr0)\]\[\033[38;5;196m\]\h\[$(tput sgr0)\]]-\n-[\[$(tput sgr0)\]\[\033[38;5;33m\]\w\[$(tput sgr0)\]]\\$ \[$(tput sgr0)\]"' >> .bashrc
┌─[cry0l1t3@parrotos]─[~]
└──╼ $ cp .bashrc .bashrc.bak
```

Customized Bash Prompt

```shell-session
-[Tue Mar 23-00:39:51]-[cry0l1t3@parrotos]-
-[~]$ 
```

Another advantage of this is that we can filter out our commands by the `minus` (`-`) at the beginning later in our logs and thus see a list with only the date, time, and command specified. There are countless variations on how we can also design the look and feel of our Linux distro

### Automation

The automation process is also an essential part of our preparation for penetration testing. Especially when it comes to internal penetration tests, where we have internet access and can adapt the workstation we are working on to our needs. This should be fast and efficient. For this, we have to create (in the best case) Bash scripts that automatically adjust our settings to the new system. Let us take the configuration and adjustment of our Bash prompt as an example. An example script can consist of the same commands we already configured.
