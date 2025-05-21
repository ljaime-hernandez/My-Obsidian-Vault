Now that we've dug into the environment and gotten the lay of the land and uncovered as much as possible about our user and group permissions as they relate to files, scripts, binaries, directories, etc. we'll take things one step further and look deeper into the internals of the host operating system. In this phase we will enumerate the following which will help to inform many of the attacks discussed in the later sections of this module.

- What services and applications are installed?
- What services are running?
- What sockets are in use?
- What users, admins, and groups exist on the system?
- Who is current logged in? What users recently logged in?
- What password policies, if any, are enforced on the host?
- Is the host joined to an Active Directory domain?
- What types of interesting information can we find in history, log, and backup files
- Which files have been modified recently and how often? Are there any interesting patterns in file modification that could indicate a cron job in use that we may be able to hijack?
- Current IP addressing information
- Anything interesting in the `/etc/hosts` file?
- Are there any interesting network connections to other systems in the internal network or even outside the network?
- What tools are installed on the system that we may be able to take advantage of? (Netcat, Perl, Python, Ruby, Nmap, tcpdump, gcc, etc.)
- Can we access the `bash_history` file for any users and can we uncover any thing interesting from their recorded command line history such as passwords?
- Are any Cron jobs running on the system that we may be able to hijack?

At this time we'll also want to gather as much network information as possible. What is our current IP address? Does the system have any other interfaces and, hence, could possibly be used to pivot into another subnet that was previously unreachable from our attack host? We do this with the `ip a` command or `ifconfig`, but this command will sometimes not work on certain systems if the [net-tools](https://packages.ubuntu.com/search?keywords=net-tools) package is not present.

## Internals

When we talk about the `internals`, we mean the internal configuration and way of working, including integrated processes designed to accomplish specific tasks. So we start with the interfaces through which our target system can communicate.

#### Network Interfaces

  Linux Services & Internals Enumeration

```shell-session
KaoruRyusaki@htb[/htb]$ ip a

1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host 
       valid_lft forever preferred_lft forever
2: ens192: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc mq state UP group default qlen 1000
    link/ether 00:50:56:b9:ed:2a brd ff:ff:ff:ff:ff:ff
    inet 10.129.203.168/16 brd 10.129.255.255 scope global dynamic ens192
       valid_lft 3092sec preferred_lft 3092sec
    inet6 dead:beef::250:56ff:feb9:ed2a/64 scope global dynamic mngtmpaddr 
       valid_lft 86400sec preferred_lft 14400sec
    inet6 fe80::250:56ff:feb9:ed2a/64 scope link 
       valid_lft forever preferred_lft forever
```

Is there anything interesting in the `/etc/hosts` file?

#### Hosts

  Linux Services & Internals Enumeration

```shell-session
KaoruRyusaki@htb[/htb]$ cat /etc/hosts

127.0.0.1 localhost
127.0.1.1 nixlpe02
# The following lines are desirable for IPv6 capable hosts
::1     ip6-localhost ip6-loopback
fe00::0 ip6-localnet
ff00::0 ip6-mcastprefix
ff02::1 ip6-allnodes
ff02::2 ip6-allrouters
```

It can also be helpful to check out each user's last login time to try to see when users typically log in to the system and how frequently. This can give us an idea of how widely used this system is which can open up the potential for more misconfigurations or "messy" directories or command histories.

#### User's Last Login

  Linux Services & Internals Enumeration

```shell-session
KaoruRyusaki@htb[/htb]$ lastlog

Username         Port     From             Latest
root                                       **Never logged in**
daemon                                     **Never logged in**
bin                                        **Never logged in**
sys                                        **Never logged in**
sync                                       **Never logged in**
...SNIP...
systemd-coredump                           **Never logged in**
mrb3n            pts/1    10.10.14.15      Tue Aug  2 19:33:16 +0000 2022
lxd                                        **Never logged in**
bjones                                     **Never logged in**
administrator.ilfreight                           **Never logged in**
backupsvc                                  **Never logged in**
cliff.moore      pts/0    127.0.0.1        Tue Aug  2 19:32:29 +0000 2022
logger                                     **Never logged in**
shared                                     **Never logged in**
stacey.jenkins   pts/0    10.10.14.15      Tue Aug  2 18:29:15 +0000 2022
htb-student      pts/0    10.10.14.15      Wed Aug  3 13:37:22 +0000 2022                          
```

In addition, let's see if anyone else is currently on the system with us. There are a few ways to do this, such as the `who` command. The `finger` command will work to display this information on some Linux systems. We can see that the `cliff.moore` user is logged in to the system with us.

#### Logged In Users

  Linux Services & Internals Enumeration

```shell-session
KaoruRyusaki@htb[/htb]$ w

 12:27:21 up 1 day, 16:55,  1 user,  load average: 0.00, 0.00, 0.00
USER     TTY      FROM             LOGIN@   IDLE   JCPU   PCPU WHAT
cliff.mo pts/0    10.10.14.16      Tue19   40:54m  0.02s  0.02s -bash
```

It is also important to check a user's bash history, as they may be passing passwords as an argument on the command line, working with git repositories, setting up cron jobs, and more. Reviewing what the user has been doing can give you considerable insight into the type of server you land on and give a hint as to privilege escalation paths.

#### Command History

  Linux Services & Internals Enumeration

```shell-session
KaoruRyusaki@htb[/htb]$ history

    1  id
    2  cd /home/cliff.moore
    3  exit
    4  touch backup.sh
    5  tail /var/log/apache2/error.log
    6  ssh ec2-user@dmz02.inlanefreight.local
    7  history
```

Sometimes we can also find special history files created by scripts or programs. This can be found, among others, in scripts that monitor certain activities of users and check for suspicious activities.

#### Finding History Files

  Linux Services & Internals Enumeration

```shell-session
KaoruRyusaki@htb[/htb]$ find / -type f \( -name *_hist -o -name *_history \) -exec ls -l {} \; 2>/dev/null

-rw------- 1 htb-student htb-student 387 Nov 27 14:02 /home/htb-student/.bash_history
```

It's also a good idea to check for any cron jobs on the system. Cron jobs on Linux systems are similar to Windows scheduled tasks. They are often set up to perform maintenance and backup tasks. In conjunction with other misconfigurations such as relative paths or weak permissions, they can leverage to escalate privileges when the scheduled cron job runs.

#### Cron

  Linux Services & Internals Enumeration

```shell-session
KaoruRyusaki@htb[/htb]$ ls -la /etc/cron.daily/

total 48
drwxr-xr-x  2 root root 4096 Aug  2 17:36 .
drwxr-xr-x 96 root root 4096 Aug  2 19:34 ..
-rwxr-xr-x  1 root root  376 Dec  4  2019 apport
-rwxr-xr-x  1 root root 1478 Apr  9  2020 apt-compat
-rwxr-xr-x  1 root root  355 Dec 29  2017 bsdmainutils
-rwxr-xr-x  1 root root 1187 Sep  5  2019 dpkg
-rwxr-xr-x  1 root root  377 Jan 21  2019 logrotate
-rwxr-xr-x  1 root root 1123 Feb 25  2020 man-db
-rw-r--r--  1 root root  102 Feb 13  2020 .placeholder
-rwxr-xr-x  1 root root 4574 Jul 18  2019 popularity-contest
-rwxr-xr-x  1 root root  214 Apr  2  2020 update-notifier-common
```

The [proc filesystem](https://man7.org/linux/man-pages/man5/proc.5.html) (`proc` / `procfs`) is a particular filesystem in Linux that contains information about system processes, hardware, and other system information. It is the primary way to access process information and can be used to view and modify kernel settings. It is virtual and does not exist as a real filesystem but is dynamically generated by the kernel. It can be used to look up system information such as the state of running processes, kernel parameters, system memory, and devices. It also sets certain system parameters, such as process priority, scheduling, and memory allocation.

#### Proc

  Linux Services & Internals Enumeration

```shell-session
KaoruRyusaki@htb[/htb]$ find /proc -name cmdline -exec cat {} \; 2>/dev/null | tr " " "\n"

...SNIP...
startups/usr/lib/packagekit/packagekitd/usr/lib/packagekit/packagekitd/usr/lib/packagekit/packagekitd/usr/lib/packagekit/packagekitdroot@10.129.14.200sshroot@10.129.14.200sshd:
htb-student
[priv]sshd:
htb-student
[priv]/usr/bin/ssh-agent-D-a/run/user/1000/keyring/.ssh/usr/bin/ssh-agent-D-a/run/user/1000/keyring/.sshsshd:
htb-student@pts/2sshd:
```

## Services

If it is a slightly older Linux system, the likelihood increases that we can find installed packages that may already have at least one vulnerability. However, current versions of Linux distributions can also have older packages or software installed that may have such vulnerabilities. Therefore, we will see a method to help us detect potentially dangerous packages in a bit. To do this, we first need to create a list of installed packages to work with.

#### Installed Packages

  Linux Services & Internals Enumeration

```shell-session
KaoruRyusaki@htb[/htb]$ apt list --installed | tr "/" " " | cut -d" " -f1,3 | sed 's/[0-9]://g' | tee -a installed_pkgs.list

Listing...                                                 
accountsservice-ubuntu-schemas 0.0.7+17.10.20170922-0ubuntu1                                                          
accountsservice 0.6.55-0ubuntu12~20.04.5                   
acl 2.2.53-6                                               
acpi-support 0.143                                         
acpid 2.0.32-1ubuntu1                                      
adduser 3.118ubuntu2                                       
adwaita-icon-theme 3.36.1-2ubuntu0.20.04.2                 
alsa-base 1.0.25+dfsg-0ubuntu5                             
alsa-topology-conf 1.2.2-1                                                                                            
alsa-ucm-conf 1.2.2-1ubuntu0.13                            
alsa-utils 1.2.2-1ubuntu2.1                                                                                           
amd64-microcode 3.20191218.1ubuntu1
anacron 2.3-29
apg 2.2.3.dfsg.1-5
app-install-data-partner 19.04
apparmor 2.13.3-7ubuntu5.1
apport-gtk 2.20.11-0ubuntu27.24
apport-symptoms 0.23
apport 2.20.11-0ubuntu27.24
appstream 0.12.10-2
apt-config-icons-hidpi 0.12.10-2
apt-config-icons 0.12.10-2
apt-utils 2.0.9
...SNIP...
```

It's also a good idea to check if the `sudo` version installed on the system is vulnerable to any legacy or recent exploits.

#### Sudo Version

  Linux Services & Internals Enumeration

```shell-session
KaoruRyusaki@htb[/htb]$ sudo -V

Sudo version 1.8.31
Sudoers policy plugin version 1.8.31
Sudoers file grammar version 46
Sudoers I/O plugin version 1.8.31
```

Occasionally it can also happen that no direct packages are installed on the system but compiled programs in the form of binaries. These do not require installation and can be executed directly by the system itself.

#### Binaries

  Linux Services & Internals Enumeration

```shell-session
KaoruRyusaki@htb[/htb]$ ls -l /bin /usr/bin/ /usr/sbin/

lrwxrwxrwx 1 root root     7 Oct 27 11:14 /bin -> usr/bin

/usr/bin/:
total 175160
-rwxr-xr-x 1 root root       31248 May 19  2020  aa-enabled
-rwxr-xr-x 1 root root       35344 May 19  2020  aa-exec
-rwxr-xr-x 1 root root       22912 Apr 14  2021  aconnect
-rwxr-xr-x 1 root root       19016 Nov 28  2019  acpi_listen
-rwxr-xr-x 1 root root        7415 Oct 26  2021  add-apt-repository
-rwxr-xr-x 1 root root       30952 Feb  7  2022  addpart
lrwxrwxrwx 1 root root          26 Oct 20  2021  addr2line -> x86_64-linux-gnu-addr2line
...SNIP...

/usr/sbin/:
total 32500
-rwxr-xr-x 1 root root      3068 Mai 19  2020 aa-remove-unknown
-rwxr-xr-x 1 root root      8839 Mai 19  2020 aa-status
-rwxr-xr-x 1 root root       139 Jun 18  2019 aa-teardown
-rwxr-xr-x 1 root root     14728 Feb 25  2020 accessdb
-rwxr-xr-x 1 root root     60432 Nov 28  2019 acpid
-rwxr-xr-x 1 root root      3075 Jul  4 18:20 addgnupghome
lrwxrwxrwx 1 root root         7 Okt 27 11:14 addgroup -> adduser
-rwxr-xr-x 1 root root       860 Dez  7  2019 add-shell
-rwxr-xr-x 1 root root     37785 Apr 16  2020 adduser
-rwxr-xr-x 1 root root     69000 Feb  7  2022 agetty
-rwxr-xr-x 1 root root      5576 Jul 31  2015 alsa
-rwxr-xr-x 1 root root      4136 Apr 14  2021 alsabat-test
-rwxr-xr-x 1 root root    118176 Apr 14  2021 alsactl
-rwxr-xr-x 1 root root     26489 Apr 14  2021 alsa-info
-rwxr-xr-x 1 root root     39088 Jul 16  2019 anacron
...SNIP...
```

[GTFObins](https://gtfobins.github.io/) provides an excellent platform that includes a list of binaries that can potentially be exploited to escalate our privileges on the target system. With the next oneliner, we can compare the existing binaries with the ones from GTFObins to see which binaries we should investigate later.

#### GTFObins

  Linux Services & Internals Enumeration

```shell-session
KaoruRyusaki@htb[/htb]$ for i in $(curl -s https://gtfobins.github.io/ | html2text | cut -d" " -f1 | sed '/^[[:space:]]*$/d');do if grep -q "$i" installed_pkgs.list;then echo "Check GTFO for: $i";fi;done

Check GTFO for: ab                                         
Check GTFO for: apt                                        
Check GTFO for: ar                                         
Check GTFO for: as         
Check GTFO for: ash                                        
Check GTFO for: aspell                                     
Check GTFO for: at     
Check GTFO for: awk      
Check GTFO for: bash                                       
Check GTFO for: bridge
Check GTFO for: busybox
Check GTFO for: bzip2
Check GTFO for: cat
Check GTFO for: comm
Check GTFO for: cp
Check GTFO for: cpio
Check GTFO for: cupsfilter
Check GTFO for: curl
Check GTFO for: dash
Check GTFO for: date
Check GTFO for: dd
Check GTFO for: diff
```

We can use the diagnostic tool `strace` on Linux-based operating systems to track and analyze system calls and signal processing. It allows us to follow the flow of a program and understand how it accesses system resources, processes signals, and receives and sends data from the operating system. In addition, we can also use the tool to monitor security-related activities and identify potential attack vectors, such as specific requests to remote hosts using passwords or tokens.

The output of `strace` can be written to a file for later analysis, and it provides a wealth of options that allow detailed monitoring of the program's behavior.

#### Trace System Calls

  Linux Services & Internals Enumeration

```shell-session
KaoruRyusaki@htb[/htb]$ strace ping -c1 10.129.112.20

execve("/usr/bin/ping", ["ping", "-c1", "10.129.112.20"], 0x7ffdc8b96cc0 /* 80 vars */) = 0
access("/etc/suid-debug", F_OK)         = -1 ENOENT (No such file or directory)
brk(NULL)                               = 0x56222584c000
arch_prctl(0x3001 /* ARCH_??? */, 0x7fffb0b2ea00) = -1 EINVAL (Invalid argument)
...SNIP...
access("/etc/ld.so.preload", R_OK)      = -1 ENOENT (No such file or directory)
...SNIP...
openat(AT_FDCWD, "/lib/x86_64-linux-gnu/libidn2.so.0", O_RDONLY|O_CLOEXEC) = 3
...SNIP...
read(3, "\177ELF\2\1\1\3\0\0\0\0\0\0\0\0\3\0>\0\1\0\0\0P\237\2\0\0\0\0\0"..., 832) = 832
pread64(3, "\6\0\0\0\4\0\0\0@\0\0\0\0\0\0\0@\0\0\0\0\0\0\0@\0\0\0\0\0\0\0"..., 784, 64) = 784
pread64(3, "\4\0\0\0 \0\0\0\5\0\0\0GNU\0\2\0\0\300\4\0\0\0\3\0\0\0\0\0\0\0"..., 48, 848) = 48
...SNIP...
socket(AF_INET, SOCK_DGRAM, IPPROTO_ICMP) = 3
socket(AF_INET6, SOCK_DGRAM, IPPROTO_ICMPV6) = 4
capget({version=_LINUX_CAPABILITY_VERSION_3, pid=0}, NULL) = 0
capget({version=_LINUX_CAPABILITY_VERSION_3, pid=0}, {effective=0, permitted=0, inheritable=0}) = 0
openat(AT_FDCWD, "/usr/lib/x86_64-linux-gnu/gconv/gconv-modules.cache", O_RDONLY) = 5
...SNIP...
socket(AF_INET, SOCK_DGRAM, IPPROTO_IP) = 5
connect(5, {sa_family=AF_INET, sin_port=htons(1025), sin_addr=inet_addr("10.129.112.20")}, 16) = 0
getsockname(5, {sa_family=AF_INET, sin_port=htons(39885), sin_addr=inet_addr("10.129.112.20")}, [16]) = 0
close(5)                                = 0
...SNIP...
sendto(3, "\10\0\31\303\0\0\0\1eX\327c\0\0\0\0\330\254\n\0\0\0\0\0\20\21\22\23\24\25\26\27"..., 64, 0, {sa_family=AF_INET, sin_port=htons(0), sin_addr=inet_addr("10.129.112.20")}, 16) = 64
...SNIP...
recvmsg(3, {msg_name={sa_family=AF_INET, sin_port=htons(0), sin_addr=inet_addr("10.129.112.20")}, msg_namelen=128 => 16, msg_iov=[{iov_base="\0\0!\300\0\3\0\1eX\327c\0\0\0\0\330\254\n\0\0\0\0\0\20\21\22\23\24\25\26\27"..., iov_len=192}], msg_iovlen=1, msg_control=[{cmsg_len=32, cmsg_level=SOL_SOCKET, cmsg_type=SO_TIMESTAMP_OLD, cmsg_data={tv_sec=1675057253, tv_usec=699895}}, {cmsg_len=20, cmsg_level=SOL_IP, cmsg_type=IP_TTL, cmsg_data=[64]}], msg_controllen=56, msg_flags=0}, 0) = 64
write(1, "64 bytes from 10.129.112.20: icmp_se"..., 57) = 57
write(1, "\n", 1)                       = 1
write(1, "--- 10.129.112.20 ping statistics --"..., 34) = 34
write(1, "1 packets transmitted, 1 receive"..., 60) = 60
write(1, "rtt min/avg/max/mdev = 0.287/0.2"..., 50) = 50
close(1)                                = 0
close(2)                                = 0
exit_group(0)                           = ?
+++ exited with 0 +++
```

Users can read almost all configuration files on a Linux operating system if the administrator has kept them the same. These configuration files can often reveal how the service is set up and configured to understand better how we can use it for our purposes. In addition, these files can contain sensitive information, such as keys and paths to files in folders that we cannot see. However, if the file has read permissions for everyone, we can still read the file even if we do not have permission to read the folder.

#### Configuration Files

  Linux Services & Internals Enumeration

```shell-session
KaoruRyusaki@htb[/htb]$ find / -type f \( -name *.conf -o -name *.config \) -exec ls -l {} \; 2>/dev/null

-rw-r--r-- 1 root root 448 Nov 28 12:31 /run/tmpfiles.d/static-nodes.conf
-rw-r--r-- 1 root root 71 Nov 28 12:31 /run/NetworkManager/resolv.conf
-rw-r--r-- 1 root root 72 Nov 28 12:31 /run/NetworkManager/no-stub-resolv.conf
-rw-r--r-- 1 root root 0 Nov 28 12:37 /run/NetworkManager/conf.d/10-globally-managed-devices.conf
-rw-r--r-- 1 systemd-resolve systemd-resolve 736 Nov 28 12:31 /run/systemd/resolve/stub-resolv.conf
-rw-r--r-- 1 systemd-resolve systemd-resolve 607 Nov 28 12:31 /run/systemd/resolve/resolv.conf
...SNIP...
```

The scripts are similar to the configuration files. Often administrators are lazy and convinced of network security and neglect the internal security of their systems. These scripts, in some cases, have such wrong privileges that we will deal with later, but the contents are of great importance even without these privileges. Because through them, we can discover internal and individual processes that can be of great use to us.

#### Scripts

  Linux Services & Internals Enumeration

```shell-session
KaoruRyusaki@htb[/htb]$ find / -type f -name "*.sh" 2>/dev/null | grep -v "src\|snap\|share"

/home/htb-student/automation.sh
/etc/wpa_supplicant/action_wpa.sh
/etc/wpa_supplicant/ifupdown.sh
/etc/wpa_supplicant/functions.sh
/etc/init.d/keyboard-setup.sh
/etc/init.d/console-setup.sh
/etc/init.d/hwclock.sh
...SNIP...
```

Also, if we look at the process list, it can give us information about which scripts or binaries are in use and by which user. So, for example, if it is a script created by the administrator in his path and whose rights have not been restricted, we can run it without going into the `root` directory.

#### Running Services by User

  Linux Services & Internals Enumeration

```shell-session
KaoruRyusaki@htb[/htb]$ ps aux | grep root

...SNIP...
root           1  2.0  0.2 168196 11364 ?        Ss   12:31   0:01 /sbin/init splash
root         378  0.5  0.4  62648 17212 ?        S<s  12:31   0:00 /lib/systemd/systemd-journald
root         409  0.8  0.1  25208  7832 ?        Ss   12:31   0:00 /lib/systemd/systemd-udevd
root         457  0.0  0.0 150668   284 ?        Ssl  12:31   0:00 vmware-vmblock-fuse /run/vmblock-fuse -o rw,subtype=vmware-vmblock,default_permissions,allow_other,dev,suid
root         752  0.0  0.2  58780 10608 ?        Ss   12:31   0:00 /usr/bin/VGAuthService
root         755  0.0  0.1 248088  7448 ?        Ssl  12:31   0:00 /usr/bin/vmtoolsd
root         772  0.0  0.2 250528  9388 ?        Ssl  12:31   0:00 /usr/lib/accountsservice/accounts-daemon
root         773  0.0  0.0   2548   768 ?        Ss   12:31   0:00 /usr/sbin/acpid
root         774  0.0  0.0  16720   708 ?        Ss   12:31   0:00 /usr/sbin/anacron -d -q -s
root         778  0.0  0.0  18052  2992 ?        Ss   12:31   0:00 /usr/sbin/cron -f
root         779  0.0  0.2  37204  8964 ?        Ss   12:31   0:00 /usr/sbin/cupsd -l
root         784  0.4  0.5 273512 21680 ?        Ssl  12:31   0:00 /usr/sbin/NetworkManager --no-daemon
root         790  0.0  0.0  81932  3648 ?        Ssl  12:31   0:00 /usr/sbin/irqbalance --foreground
root         792  0.1  0.5  48244 20540 ?        Ss   12:31   0:00 /usr/bin/python3 /usr/bin/networkd-dispatcher --run-startup-triggers
root         793  1.3  0.2 239180 11832 ?        Ssl  12:31   0:00 /usr/lib/policykit-1/polkitd --no-debug
root         806  2.1  1.1 1096292 44976 ?       Ssl  12:31   0:01 /usr/lib/snapd/snapd
root         807  0.0  0.1 244352  6516 ?        Ssl  12:31   0:00 /usr/libexec/switcheroo-control
root         811  0.1  0.2  17412  8112 ?        Ss   12:31   0:00 /lib/systemd/systemd-logind
root         817  0.0  0.3 396156 14352 ?        Ssl  12:31   0:00 /usr/lib/udisks2/udisksd
root         818  0.0  0.1  13684  4876 ?        Ss   12:31   0:00 /sbin/wpa_supplicant -u -s -O /run/wpa_supplicant
root         871  0.1  0.3 319236 13828 ?        Ssl  12:31   0:00 /usr/sbin/ModemManager
root         875  0.0  0.3 178392 12748 ?        Ssl  12:31   0:00 /usr/sbin/cups-browsed
root         889  0.1  0.5 126676 22888 ?        Ssl  12:31   0:00 /usr/bin/python3 /usr/share/unattended-upgrades/unattended-upgrade-shutdown --wait-for-signal
root         906  0.0  0.2 248244  8736 ?        Ssl  12:31   0:00 /usr/sbin/gdm3
root        1137  0.0  0.2 252436  9424 ?        Ssl  12:31   0:00 /usr/lib/upower/upowerd
root        1257  0.0  0.4 293736 16316 ?        Ssl  12:31   0:00 /usr/lib/packagekit/packagekitd
```

This would give us a pretty good overview of our target system, so we can go into more detail next and figure out the individual permissions for the components we found.



#### Question

Enumerate the Linux environment and look for interesting files that might contain sensitive data. Submit the flag as the answer

* 