Windows computers serve an essential role as testbeds and victims for aspiring penetration testers like ourselves. However, it can make for a great penetration testing platform as well. There are some advantages to using Windows as our daily driver. It will blend in most enterprise environments so that we will appear physically and virtually less suspicious. It is easier to navigate and communicate with other hosts on an Active Directory domain if we use Windows versus Linux and some Python tooling. Traversing SMB and utilizing shares is much easier this way. With this in mind, it can be beneficial to familiarize ourselves with Windows and set a standard that ensures we have a stable and effective platform to perform our actions.

Building our penetration testing platform can help us in multiple ways:

1.  Since we built it and installed only the tools necessary, we should have a better understanding of what is happening under the hood. This also allows us to ensure we do not have any unnecessary services running that could potentially be a risk to ourselves and the customer when on an engagement.
    
2.  It provides us the flexibility of having multiple operating system types at our disposal if needed. These same systems used for our engagements can also serve as a testbed for payloads and exploits before launching them at the customer.
    
3.  By building and testing the systems ourselves, we know they will function as intended during the penetration test and save ourselves time troubleshooting during the engagement.
    

With all this in mind, where do we start? Fortunately for us, there are many new features with Windows that were not available just a few years ago. `Windows Subsystem for Linux (WSL)` is an excellent example of this. It allows for Linux operating systems to run alongside our Windows install. This can help us by giving us a space to run tools developed for Linux right inside our Windows host without the need for a hypervisor program or installation of a third-party application such as VirtualBox or Docker.

This section will examine and install the core components we will need to get our systems in fighting shape, such as `WSL, Visual Studio Code, Python, Git, and the Chocolatey Package Manager`. Since we are utilizing this platform to perform penetration test functions, it will also require us to make changes to our host's security settings. Keep in mind, most exploitation tools and code are just that, `USED for EXPLOITATION` and can be harmful to your host if not careful. Be mindful of what we install and run. If we do not isolate these tools off, Windows Defender will almost certainly delete any detected files and applications it deems harmful, breaking our setup.

### Installation Requirements

The installation of the Windows VM is done in the same way as the Linux VM. We can do this on a bare-metal host or in a hypervisor. With either option, we have some requirements to think about when installing Windows 10.

#### Hardware Requirements

Processor that runs at 1GHz or greater. Dual-core or better is ideal.

2G of RAM minimum, 4G or more is ideal.

60G of Hard Drive space. This ensures there is room for the OS and some tools. Size can vary based on the number of tools we install on the host.

Network connectivity, if possible, two network adapters.

Ideally, we have a moderate processor that can handle intensive loads at times. If we are attempting to run Windows virtualized, our host will need at least four cores to give two to the VM. Windows can get a bit beefy with updates and tool installs, so 80G of storage or more is ideal. When it comes to RAM, 4G would be a minimum to ensure we do not have any latency or issues while performing our penetration tests.

#### Software Requirements

Unlike most Linux distributions, Windows is a licensed product. To stay in good standing, ensure we are adhering to the terms of use. For now, a great place to start is to grab a copy of a Developer VM [here](https://developer.microsoft.com/en-us/windows/downloads/virtual-machines/). We can use this to begin building out our platform. The Developer Evaluation platform comes pre-configured with:

Windows 10 Version 2004

Windows 10 SDK Version 2004

Visual Studio 2019 with the UWP, .NET desktop, and Azure workflows enabled and also includes the Windows Template Studio extension

Visual Studio Code

Windows Subsystem for Linux with Ubuntu installed

Developer mode enabled

The VM comes pre-configured with a user: `IEUser` and Password `Passw0rd!`. It is a trial virtual machine, so it has an expiration date of 90 days. Keep this in mind when configuring it. Once we have a baseline VM, take a snapshot.

### Core Changes

To prepare our Windows host, we have to make a few changes before installing our fun tools:

1.  We will need to update our host to ensure it is working at the required level and keep our security posture as strong as possible.
    
2.  We will want to install the Windows Subsystem for Linux and the [Chocolatey Package manager](https://chocolatey.org/). Once these tasks are completed, we can make our exclusions to Windows Defender scanning policies to ensure they will not quarantine our newly installed tools and scripts. From this point, it is now time to install our tools and scripts of choice.
    
3.  We will finish our buildout by taking a backup or snapshot of the host to have a fallback point if something happens to it.

### Updates

To keep with our command-line use, we will work at utilizing the command-line whenever possible. To start installing updates on our host, we will need the PSWindowsUpdate module. To acquire it, we will open an administrator Powershell window and issue the following commands:

```powershell-session
PS C:\htb> Get-ExecutionPolicy -List

Scope ExecutionPolicy
----- ---------------
MachinePolicy Undefined
UserPolicy Undefined
Process Undefined
CurrentUser Undefined
LocalMachine Undefined 
```
We must first check our systems Execution Policy to ensure we can download, load, and run modules and scripts. The above command will show us a list output with the policy set for each scope. In our case, we do not want this change to be permanent, so we will only change the ExecutionPolicy for the scope of `Process`.

Execution Policy

```powershell-session
PS C:\htb> Set-ExecutionPolicy Unrestricted -Scope Process

Execution Policy Change
The execution policy helps protect you from scripts that you do not trust. 
Changing the execution policy might expose you to the security risks described in the about_Execution_Policies help topic at https:/go.microsoft.com/fwlink/?LinkID=135170. Do you want to change the execution policy?
[Y] Yes [A] Yes to All [N] No [L] No to All [S] Suspend [?] Help (default is "N"): A

PS C:\htb> Get-ExecutionPolicy -List

Scope ExecutionPolicy
----- ---------------
MachinePolicy Undefined
UserPolicy Undefined
Process Unrestricted
CurrentUser Undefined
LocalMachine Undefined
```
Once we set our ExecutionPolicy, recheck it to make sure our change took effect. By changing the Process scope policy, we ensure our change is temporary and only applies to the current Powershell process. Changing it for any other scope will modify a registry setting and persist until we change it again.

Now that we have our ExecutionPolicy set, let us install the PSWindowsUpdate module and apply our updates. We can do so by:

PSWindowsUpdate

```powershell-session
PS C:\htb> Install-Module PSWindowsUpdate 

Untrusted repository 
You are installing the modules from an untrusted repository. If you trust this repository, 
change its InstallationPolicy value by running the Set-PSRepository cmdlet. 
Are you sure you want to install the modules from 'PSGallery'?
[Y] Yes [A] Yes to All [N] No [L] No to All [S] Suspend [?] Help (default is "N"): A 
```

Once the module installation completes, we can import it and run our updates.

```powershell-session
PS C:\htb> Import-Module PSWindowsUpdate 

PS C:\htb> Install-WindowsUpdate -AcceptAll

X ComputerName Result KB Size Title
- ------------ ------ -- ---- -----
1 DESKTOP-3... Accepted KB2267602 510MB Security Intelligence Update for Microsoft Defender Antivirus - KB2267602...
1 DESKTOP-3... Accepted 17MB VMware, Inc. - Display - 8.17.2.14
2 DESKTOP-3... Downloaded KB2267602 510MB Security Intelligence Update for Microsoft Defender Antivirus - KB2267602...
2 DESKTOP-3... Downloaded 17MB VMware, Inc. - Display - 8.17.2.14 3 DESKTOP-3... Installed KB2267602 510MB Security Intelligence Update for Microsoft Defender Antivirus - KB2267602... 3 DESKTOP-3... Installed 17MB VMware, Inc. - Display - 8.17.2.14 

PS C:\htb> Restart-Computer -Force
```

The above Powershell example will import the `PSWindowsUpdate` module, run the update installer, and then reboot the PC to apply changes. Be sure to run updates regularly, especially if we plan to use this host frequently and not destroy it at the end of each engagement. Now that we have our updates installed let us get our package manager and other essential core tools.

### Chocolatey Package Manager

Chocolatey is a free and open software package management solution that can manage the installation and dependencies for our software packages and scripts. It also allows for automation with Powershell, Ansible, and several other management solutions. Chocolatey will enable us to install the tools we need from one source instead of downloading and installing each tool individually from the internet. Follow the Powershell windows below to learn how to install Chocolatey and use it to gather and install our tools.

Chocolatey

```powershell-session
PS C:\htb> Set-ExecutionPolicy Bypass -Scope Process -Force; [System.Net.ServicePointManager]::SecurityProtocol = [System.Net.ServicePointManager]::SecurityProtocol -bor 3072; iex ((New-Object System.Net.WebClient).DownloadString('https://chocolatey.org/install.ps1'))

Forcing web requests to allow TLS v1.2 (Required for requests to Chocolatey.org)
Getting latest version of the Chocolatey package for download.
Not using proxy.
Getting Chocolatey from https://community.chocolatey.org/api/v2/package/chocolatey/0.10.15.
Downloading https://community.chocolatey.org/api/v2/package/chocolatey/0.10.15 to C:\Users\DEMONS~1\AppData\Local\Temp\chocolatey\chocoInstall\chocolatey.zip
Not using proxy.
Extracting C:\Users\DEMONS~1\AppData\Local\Temp\chocolatey\chocoInstall\chocolatey.zip to C:\Users\DEMONS~1\AppData\Local\Temp\chocolatey\chocoInstall
Installing Chocolatey on the local machine
Creating ChocolateyInstall as an environment variable (targeting 'Machine')
Setting ChocolateyInstall to 'C:\ProgramData\chocolatey'

...SNIP...

Chocolatey (choco.exe) is now ready.
You can call choco from the command-line or PowerShell by typing choco.
Run choco /? for a list of functions.
You may need to shut down and restart powershell and/or consoles
first prior to using choco.
Ensuring Chocolatey commands are on the path
Ensuring chocolatey.nupkg is in the lib folder
```

We have now installed chocolatey. The Powershell string we issued sets our ExecutionPolicy for the session and then downloads the installer from chocolatey.org and runs the script. Next, we will update chocolatey then start installing packages. To ensure no issues arise, it is recommended that we periodically restart our host.

```powershell-session
PS C:\htb> choco upgrade chocolatey -y 

Chocolatey v0.10.15
Upgrading the following packages:
chocolatey
By upgrading, you accept licenses for the packages.
chocolatey v0.10.15 is the latest version available based on your source(s).

Chocolatey upgraded 0/1 packages.
See the log for details (C:\ProgramData\chocolatey\logs\chocolatey.log).
```

Now that we are sure chocolatey is up-to-date let us run our packages. We can use `choco` to install packages by issuing the `choco install pkg1 pkg2 pkg3` command listing out the package you need one by one separated by spaces. Alternatively, we can use a `packages.config` file for the installation. This is an XML file formatted so that chocolatey can install a list of packages. One helpful command to use is `choco info pkg`. It will show us various information about a package if it is available in the choco repository. See the [install page](https://docs.chocolatey.org/en-us/choco/commands/install) for more info on how to utilize chocolatey.

```powershell-session
PS C:\htb> choco install python vscode git wsl2 openssh openvpn 

Chocolatey v0.10.15
Installing the following packages:
python;vscode;git;wsl2;openssh;openvpn 
...SNIP... 

Chocolatey installed 20/20 packages.
See the log for details (C:\ProgramData\chocolatey\logs\chocolatey.log).

Installed:
- kb2919355 v1.0.20160915
- python v3.9.4
- kb3033929 v1.0.5
- chocolatey-core.extension v1.3.5.1
- kb2999226 v1.0.20181019
- python3 v3.9.4
- openssh v8.0.0.1
- vcredist2015 v14.0.24215.20170201
- gpg4win-vanilla v2.3.4.20191021
- vscode.install v1.55.1
- wsl2 v2.0.0.20210122
- kb2919442 v1.0.20160915
- openvpn v2.4.7
- git.install v2.31.1
- vscode v1.55.1
- vcredist140 v14.28.29913
- kb3035131 v1.0.3
- dotnet4.5.2 v4.5.2.20140902
- git v2.31.1
- chocolatey-windowsupdate.extension v1.0.4

PS C:\htb> refresh env 
```

We can see in the terminal above that `choco` installed the packages we requested and pulled any dependencies required. Issuing the `refreshenv` command will update Powershell and any environment variables that were applied. Up to this point, we have our core tools installed. These tools will enable our operations. To install other packages, use the `choco install pkg` command to pull any operational tools we need. We have included a list of helpful packages that can aid us in completing a penetration test below. See the automation section further down to begin automating installing the tools and packages we commonly need and use.

### Windows Terminal

Windows Terminal is Microsoft's updated release for a GUI terminal emulator. It supports using many different command-line tools to include Command Prompt, PowerShell, and Windows Subsystem for Linux. The terminal allows for the use of customizable themes, configurations, command-line arguments, and custom actions. A terminal is a versatile tool for managing multiple shell types and will quickly become a staple for most.

To install Terminal with Chocolatey:

```powershell-session
PS C:\htb> choco install microsoft-windows-terminal
```

### Windows Subsystem for Linux 2

`Windows Subsystem for Linux 2` (`WSL2`) is the second iteration of Microsoft's architecture that allows users to run Linux instances, provides the ability to run Bash scripts and other apps like Vim, Python, etc. WSL also allows us to interact with the Windows operating system and file structure from a Unix instance. Best of all, it is done without the use of a hypervisor like VirtualBox or Hyper-V.

What does this mean for us? Having the ability to interact and utilize Linux native tools and applications from our Windows host provides us with a hybrid environment and the flexibility that comes with it. To install the subsystem, the quickest route is to utilize chocolatey.

Chocolatey - WSL2

```powershell-session
PS C:\htb> choco install WSL2
```

Once WSL is installed, we can add the Linux platform of our choice. The most common one to find is Ubuntu on the Microsoft store. Current Linux distributions supported for WSL are:

-   [Ubuntu 16.04 LTS](https://www.microsoft.com/store/apps/9pjn388hp8c9), [18.04 LTS](https://www.microsoft.com/store/apps/9N9TNGVNDL3Q), and [20.04 LTS](https://www.microsoft.com/store/apps/9n6svws3rx71)
-   [openSUSE Leap 15.1](https://www.microsoft.com/store/apps/9NJFZK00FGKV)
-   [SUSE Linux Enterprise Server 12 SP5](https://www.microsoft.com/store/apps/9MZ3D1TRP8T1) and [15 SP1](https://www.microsoft.com/store/apps/9PN498VPMF3Z)
-   [Kali Linux](https://www.microsoft.com/store/apps/9PKR34TNCV07)
-   [Debian GNU/Linux](https://www.microsoft.com/store/apps/9MSVKQC78PK6)
-   [Fedora Remix for WSL](https://www.microsoft.com/store/apps/9n6gdm4k2hnc)
-   [Pengwin](https://www.microsoft.com/store/apps/9NV1GV1PXZ6P) and [Pengwin Enterprise](https://www.microsoft.com/store/apps/9N8LP0X93VCP)
-   [Alpine WSL](https://www.microsoft.com/store/apps/9p804crf0395)

To install the distribution of our choice, just click the link above, and it will take us to the Microsoft Store page for the distro. Once we have it installed, we need to open a PowerShell prompt and type `bash`.

### Security Configurations and Defender Modifications

Since we will be using this platform as a penetration testing host, we may run into some issues with Windows Defender finding our tools unsavory. Windows Defender will scan and quarantine or remove anything it deems potentially harmful. To make sure Defender does not mess up our plans, we will add some exclusion rules to ensure our tools stay in place.

#### Windows Defender Exemptions for the Tools' Folders.

-   `C:\Users\your user here\AppData\Local\Temp\chocolatey\`
    
-   `C:\Users\your user here\Documents\git-repos\`
    
-   `C:\Users\your user here\Documents\scripts\`
    

These three folders are just a start. As we add more tools and scripts, we may need to add more exclusions. To exclude these files, we will run a PowerShell command.

#### Adding Exclusions

  Adding Exclusions

```powershell-session
PS C:\htb> Add-MpPreference -ExclusionPath "C:\Users\your user here\AppData\Local\Temp\chocolatey\"
```

Repeat the same steps for each folder we wish to exclude.


### Tool Install Automation

Utilizing Chocolatey for package management makes it super easy to automate the initial install of core tools and applications. We can use a simple PowerShell script to pull everything for us in one run. Here is an example of a simple script to install some of our requirements. As usual, before executing any scripts, we need to change the execution policy. Once we have our initial script built, we can modify it as our toolkit changes and reuse it to speed up our setup process.

Choco Build Script

Code: powershell

```powershell
# Choco build script

write-host "*** Initial app install for core tools and packages. ***"

write-host "*** Configuring chocolatey ***"
choco feature enable -n allowGlobalConfirmation

write-host "*** Beginning install, go grab a coffee. ***"
choco upgrade wsl2 python git vscode openssh openvpn netcat nmap wireshark burp-suite-free-edition heidisql sysinternals putty golang neo4j-community openjdk

write-host "*** Build complete, restoring GlobalConfirmation policy. ***"
choco feature disable -n allowGlobalCOnfirmation
```

When scripting with Chocolatey, the developers recommend a few rules to follow:

-   always use `choco` or `choco.exe` as the command in your scripts. `cup` or `cinst` tends to misbehave when used in a script
-   when utilizing options like `-n` it is recommended that we use the extended option like `--name`. 
-   Do not use `--force` in scripts. It overrides Chocolatey's behavior.

Not all of our packages can be acquired from Chocolatey. Fortunately for us, a majority of what is left resides in Github. We can set up a script for this and download the repositories and binaries we need, then extract them to our scripts folder. 

## Testing VMs

It is common and very relevant to prepare our penetration testing VMs and VMs for the most common operating systems and their patch levels. This is especially necessary if we want to mirror our target machines and test our exploits before applying them to real machines. For example, we can install a Windows 10 VM that is built on different [patches](https://support.microsoft.com/en-us/topic/windows-10-update-history-24ea91f4-36e7-d8fd-0ddb-d79d9d0cdbda) and [releases](https://docs.microsoft.com/en-us/windows/release-health/release-information). This will save us considerable time in the course of our penetration tests to configure them again. These VMs will help us test our approach and exploits to understand better how the interconnected system might react to them because it may be that we will only have one attempt to execute the exploit.

The good thing here is that we do not have to set up 20 VMs for this but can work with snapshots. For example, we can start with Windows 10 version 1607 (OS build 14393) and update our system step by step and create a snapshot of the clean system from each of these updates and patches. Updates and patches can be downloaded from the [Microsoft Update Catalog](https://www.catalog.update.microsoft.com/Search.aspx?q=KB4550994). We just need to use the `Kb article` designation, and there we will find the appropriate files to download and patch our systems.

Tools that can be used to install older versions of Windows:

-   [Chocolatey](https://community.chocolatey.org/)
    
-   [MediaCreationTool.bat](https://gist.github.com/AveYo/c74dc774a8fb81a332b5d65613187b15)
    
-   [Microsoft Windows and Office ISO Download Tool](https://www.heidoc.net/joomla/technology-science/microsoft/67-microsoft-windows-and-office-iso-download-tool%EF%BB%BF)
    
-   [Rufus](https://rufus.ie/en_US/)