The `Set User ID upon Execution` (`setuid`) permission can allow a user to execute a program or script with the permissions of another user, typically with elevated privileges. The `setuid` bit appears as an `s`.

```shell-session
KaoruRyusaki@htb[/htb]$ find / -user root -perm -4000 -exec ls -ldb {} \; 2>/dev/null

-rwsr-xr-x 1 root root 16728 Sep  1 19:06 /home/htb-student/shared_obj_hijack/payroll
-rwsr-xr-x 1 root root 16728 Sep  1 22:05 /home/mrb3n/payroll
-rwSr--r-- 1 root root 0 Aug 31 02:51 /home/cliff.moore/netracer
-rwsr-xr-x 1 root root 40152 Nov 30  2017 /bin/mount
-rwsr-xr-x 1 root root 40128 May 17  2017 /bin/su
-rwsr-xr-x 1 root root 27608 Nov 30  2017 /bin/umount
-rwsr-xr-x 1 root root 44680 May  7  2014 /bin/ping6
-rwsr-xr-x 1 root root 30800 Jul 12  2016 /bin/fusermount
-rwsr-xr-x 1 root root 44168 May  7  2014 /bin/ping
-rwsr-xr-x 1 root root 142032 Jan 28  2017 /bin/ntfs-3g
-rwsr-xr-x 1 root root 38984 Jun 14  2017 /usr/lib/x86_64-linux-gnu/lxc/lxc-user-nic
-rwsr-xr-- 1 root messagebus 42992 Jan 12  2017 /usr/lib/dbus-1.0/dbus-daemon-launch-helper
-rwsr-xr-x 1 root root 14864 Jan 18  2016 /usr/lib/policykit-1/polkit-agent-helper-1
-rwsr-sr-x 1 root root 85832 Nov 30  2017 /usr/lib/snapd/snap-confine
-rwsr-xr-x 1 root root 428240 Jan 18  2018 /usr/lib/openssh/ssh-keysign
-rwsr-xr-x 1 root root 10232 Mar 27  2017 /usr/lib/eject/dmcrypt-get-device
-rwsr-xr-x 1 root root 23376 Jan 18  2016 /usr/bin/pkexec
-rwsr-sr-x 1 root root 240 Feb  1  2016 /usr/bin/facter
-rwsr-xr-x 1 root root 39904 May 17  2017 /usr/bin/newgrp
-rwsr-xr-x 1 root root 32944 May 17  2017 /usr/bin/newuidmap
-rwsr-xr-x 1 root root 49584 May 17  2017 /usr/bin/chfn
-rwsr-xr-x 1 root root 136808 Jul  4  2017 /usr/bin/sudo
-rwsr-xr-x 1 root root 40432 May 17  2017 /usr/bin/chsh
-rwsr-xr-x 1 root root 32944 May 17  2017 /usr/bin/newgidmap
-rwsr-xr-x 1 root root 75304 May 17  2017 /usr/bin/gpasswd
-rwsr-xr-x 1 root root 54256 May 17  2017 /usr/bin/passwd
-rwsr-xr-x 1 root root 10624 May  9  2018 /usr/bin/vmware-user-suid-wrapper
-rwsr-xr-x 1 root root 1588768 Aug 31 00:50 /usr/bin/screen-4.5.0
-rwsr-xr-x 1 root root 94240 Jun  9 14:54 /sbin/mount.nfs
```

It may be possible to reverse engineer the program with the SETUID bit set, identify a vulnerability, and exploit this to escalate our privileges. Many programs have additional features that can be leveraged to execute commands and, if the `setuid` bit is set on them, these can be used for our purpose.

The Set-Group-ID (setgid) permission is another special permission that allows us to run binaries as if we were part of the group that created them. These files can be enumerated using the following command: `find / -uid 0 -perm -6000 -type f 2>/dev/null`. These files can be leveraged in the same manner as `setuid` binaries to escalate privileges.

  Special Permissions

```shell-session
KaoruRyusaki@htb[/htb]$ find / -user root -perm -6000 -exec ls -ldb {} \; 2>/dev/null

-rwsr-sr-x 1 root root 85832 Nov 30  2017 /usr/lib/snapd/snap-confine
```

This [resource](https://linuxconfig.org/how-to-use-special-permissions-the-setuid-setgid-and-sticky-bits) has more information about the `setuid` and `setgid` bits, including how to set the bits.

## GTFOBins

The [GTFOBins](https://gtfobins.github.io/) project is a curated list of binaries and scripts that can be used by an attacker to bypass security restrictions. Each page details the program's features that can be used to break out of restricted shells, escalate privileges, spawn reverse shell connections, and transfer files. For example, `apt-get` can be used to break out of restricted environments and spawn a shell by adding a Pre-Invoke command:

```shell-session
KaoruRyusaki@htb[/htb]$ sudo apt-get update -o APT::Update::Pre-Invoke::=/bin/sh

# id
uid=0(root) gid=0(root) groups=0(root)
```

It is worth familiarizing ourselves with as many GTFOBins as possible to quickly identify misconfigurations when we land on a system that we must escalate our privileges to move further.



#### Questions

Find a file with the setuid bit set that was not shown in the section command output (full path to the binary).

* Download the VPN file from the HTB Academy webpage
* Open your VM and go to your Downloads page from the terminal
* Run the command " openvpn academy-regular.ovpn " (if it doesnt work try running the same command with sudo)
* Use the user and password given by HTB and then SSH to the target system spawned for you by running this command "ssh htb-student@\<ip given>\"
* Run command " find / -user root -perm -4000 -exec ls -ldb {} \; 2>/dev/null "
* the binary thats not on the documents output is " /bin/sed "



Find a file with the setgid bit set that was not shown in the section command output (full path to the binary).

* Download the VPN file from the HTB Academy webpage
* Open your VM and go to your Downloads page from the terminal
* Run the command " openvpn academy-regular.ovpn " (if it doesnt work try running the same command with sudo)
* Use the user and password given by HTB and then SSH to the target system spawned for you by running this command "ssh htb-student@\<ip given>\"
* Run command " find / -user root -perm -4000 -exec ls -ldb {} \; 2>/dev/null "
* the binary thats not on the documents output is " /usr/bin/facter "