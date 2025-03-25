
For the first chapter of this assessment, we will be showcasing the HTB Academy `Pwnbox` --- a fully functional Linux machine running [Parrot OS](https://parrotsec.org/docs/introduction/what-is-parrot/), accessible entirely through a web browser. We provide it to our students to serve as their workstation when completing the various exercises and labs available on our platform. If you've never used Linux before, have no fear. Everything will be completely guided. When you are ready to begin, scroll down to select your Pwnbox location, and click `Launch Instance`.

Once the Pwnbox is up and running, feel free to press the `Full Screen` button for more visibility. Then, use your mouse cursor to open the `Parrot Terminal` as shown in the example below.

#### Launching the Parrot Terminal

![GIF showcasing the process to launch the parrot terminal.](https://academy.hackthebox.com/storage/modules/289/Skills_Assessment/parrot-terminal-2.gif)


We will start by investigating the network interfaces available on the Pwnbox. Type the following command into the terminal and press enter.

Code: shell

```shell
ifconfig -a
```

The `ifconfig` command is used on Linux to display network interface information (with the Windows equivalent being `ipconfig`.) After we send the command, we are shown three network interfaces: `ens3`, `lo`, and `tun0`, along with quite a bit of information. Take a few moments to examine the output, and take note of any similarities or differences you see between the three interfaces. Don't worry if some of the information doesn't make sense yet.

  Keep me in the Loop

```shell-session
┌─[eu-academy-1]─[10.10.14.21]─[htb-ac-594497@htb-5mix2gkv1a]─[~]
└──╼ [★]$ ifconfig -a
 
ens3: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
       inet 209.50.61.235  netmask 255.255.252.0  broadcast 209.50.61.255
       inet6 fe80::a4ba:3bff:fe08:1e4e  prefixlen 64  scopeid 0x20<link>
       ether a6:ba:3b:08:1e:4e  txqueuelen 1000  (Ethernet)
       RX packets 30046  bytes 37369216 (35.6 MiB)
       RX errors 0  dropped 0  overruns 0  frame 0
       TX packets 20239  bytes 33367968 (31.8 MiB)
       TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

lo: flags=73<UP,LOOPBACK,RUNNING>  mtu 65536
       inet 127.0.0.1  netmask 255.0.0.0
       inet6 ::1  prefixlen 128  scopeid 0x10<host>
       loop  txqueuelen 1000  (Local Loopback)
       RX packets 44771  bytes 33774927 (32.2 MiB)
       RX errors 0  dropped 0  overruns 0  frame 0
       TX packets 44771  bytes 33774927 (32.2 MiB)
       TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

tun0: flags=4305<UP,POINTOPOINT,RUNNING,NOARP,MULTICAST>  mtu 1500
       inet 10.10.14.21  netmask 255.255.254.0  destination 10.10.14.21
       inet6 dead:beef:2::11bb  prefixlen 64  scopeid 0x0<global>
       inet6 fe80::3c16:f601:d437:d71b  prefixlen 64  scopeid 0x20<link>
       unspec 00-00-00-00-00-00-00-00-00-00-00-00-00-00-00-00  txqueuelen 500  (UNSPEC)
       RX packets 164  bytes 13776 (13.4 KiB)
       RX errors 0  dropped 0  overruns 0  frame 0
       TX packets 170  bytes 14064 (13.7 KiB)
       TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0
```

There is certainly a lot to unpack. We see three unique IPv4 addresses. We see some IPv6 addresses as well. However, one interface in particular stands out from the rest. Something seems very different about the `lo` interface.

  Keep me in the Loop

```shell-session
 lo: flags=73<UP,LOOPBACK,RUNNING>  mtu 65536 	   ### Greater MTU (maximum transmission unit) compared to other interfaces  
       inet 127.0.0.1  netmask 255.0.0.0
       inet6 ::1  prefixlen 128  scopeid 0x10<host> ###  ipv6 address is ::1 --- scopeid is "host" rather than "link"
       loop  txqueuelen 1000  (Local Loopback) 	 ### Layer-2 information has phrases "loop" and "local loopback"
       RX packets 44771  bytes 33774927 (32.2 MiB)
       RX errors 0  dropped 0  overruns 0  frame 0
       TX packets 44771  bytes 33774927 (32.2 MiB)
       TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

```

What we are looking at is known as the [loopback](https://www.geeksforgeeks.org/what-is-a-loopback-address/) address, and it is always associated to the IPv4 address `127.0.0.1`. It's the IP address used when a device needs to send network data to itself. You may be wondering what purpose this serves; there are actually several. It's often used for testing, as a way to make sure an application is working as intended before going live on the network. It is also used by servers to keep certain services hidden from outside users. Think of an e-commerce website that utilizes authentication with its clients (i.e. registered acounts with usernames and passwords). Credentials and session cookies are typically stored in a database. Rather than have the database exposed to the public, it instead can only be accessed by the server itself. When a user attempts to log into the website, the website acts as an API between the user and the database. The server queries its own database to retrieve information on behalf of the end user.

Let's see if the Pwnbox makes use of the loopback address. In your terminal, enter the following command:

Code: shell

```shell
netstat -tulnp4
```

This will show us all of the open/listening `tcp` and `udp` ports for IPv4 , in the format of "`IP:PORT`". Additionally, depending on our permissions, we might be able see the name of the Program that's causing a port to be open. Again, take some time and examine the output. Do you see any services running on the loopback address?

  Keep me in the Loop

```shell-session
┌─[eu-academy-1]─[10.10.14.21]─[htb-ac-594497@htb-5mix2gkv1a]─[~]
└──╼ [★]$ netstat -tulnp4
  
(Not all processes could be identified, non-owned process info
 will not be shown, you would have to be root to see it all.)
Active Internet connections (only servers)  
Proto Recv-Q Send-Q Local Address           Foreign Address         State       PID/Program name    
tcp        0      0 127.0.0.1:5901          0.0.0.0:*               LISTEN      2814/Xtigervnc      
tcp        0      0 127.0.0.1:631           0.0.0.0:*               LISTEN      -                   
tcp        0      0 0.0.0.0:22              0.0.0.0:*               LISTEN      -                   
tcp        0      0 0.0.0.0:111             0.0.0.0:*               LISTEN      -                   
tcp        0      0 209.50.61.235:80        0.0.0.0:*               LISTEN      -                   
udp        0      0 0.0.0.0:5353            0.0.0.0:*                           -                   
udp        0      0 0.0.0.0:43446           0.0.0.0:*                           -                   
udp        0      0 0.0.0.0:68              0.0.0.0:*                           -                   
udp        0      0 0.0.0.0:111             0.0.0.0:*                           -                   
udp        0      0 10.10.14.21:123         0.0.0.0:*                           -                   
udp        0      0 209.50.62.174:123       0.0.0.0:*                           -                   
udp        0      0 127.0.0.1:123           0.0.0.0:*                           -                   
udp        0      0 0.0.0.0:123             0.0.0.0:*                           -                   
udp        0      0 0.0.0.0:631             0.0.0.0:*                           -                   
udp        0      0 0.0.0.0:33423           0.0.0.0:*                           -   
```

Now, try running the command shown below.

  Keep me in the Loop

```shell-session
netstat tulp4
```

When we remove the `-n` option, the output will be displayed as "`hostname:service`" rather than "`IP:PORT`". We can see that the loopback IP address is resolved to `localhost`. The `ens3` IP address is resolved to the hostname of the Pwnbox. Also, it is worthwhile to note that a service listening on `0.0.0.0` is listening on all interfaces.

  Keep me in the Loop

```shell-session
┌──[eu-academy-1]─[10.10.14.21]─[htb-ac-594497@htb-5mix2gkv1a]─[~]
└──╼ [★]$ netstat -tulp4
  
(Not all processes could be identified, non-owned process info
 will not be shown, you would have to be root to see it all.)
Active Internet connections (only servers)
Proto Recv-Q Send-Q Local Address           Foreign Address         State       PID/Program name    
tcp        0      0 localhost:5901          0.0.0.0:*               LISTEN      2814/Xtigervnc      
tcp        0      0 localhost:ipp           0.0.0.0:*               LISTEN      -                   
tcp        0      0 0.0.0.0:ssh             0.0.0.0:*               LISTEN      -                   
tcp        0      0 0.0.0.0:sunrpc          0.0.0.0:*               LISTEN      -                   
tcp        0      0 htb-5mix2gkv1a.htb:http 0.0.0.0:*               LISTEN      -                   
udp        0      0 0.0.0.0:mdns            0.0.0.0:*                           -                   
udp        0      0 0.0.0.0:43446           0.0.0.0:*                           -                   
udp        0      0 0.0.0.0:bootpc          0.0.0.0:*                           -                   
udp        0      0 0.0.0.0:sunrpc          0.0.0.0:*                           -                   
udp        0      0 htb-5mix2gkv1a:ntp      0.0.0.0:*                           -                   
udp        0      0 htb-5mix2gkv1a.htb-:ntp 0.0.0.0:*                           -                   
udp        0      0 localhost:ntp           0.0.0.0:*                           -                   
udp        0      0 0.0.0.0:ntp             0.0.0.0:*                           -                   
udp        0      0 0.0.0.0:631             0.0.0.0:*                           -                   
udp        0      0 0.0.0.0:33423           0.0.0.0:*                           -   
```

With this information, we now have some insight as to how we are able to see, and interact, with the Pwnbox. Earlier in the module we learned that protocol used when browsing websites is `HTTP`, via the well-known port 80. As we can see, the Pwnbox is indeed listening on port 80. This explains how we are able to make a connection via web browser. Subsequently, we can state with confidence that the IP tied to the `ens3` interface is the public IP address of the Pwnbox. Remember, public IP's can be accessed over the internet.

We also see the VNC service running on the loopback address. VNC (Virtual Network Computing) is a protocol used for remote screen sharing and remote access. Since students can access the Pwnbox desktop environment through their web browser, there is likely some form of port forwarding in place. This would allow traffic sent over HTTP to be forwarded to the VNC service running on the loopback address.

Port forwarding is a technique that allows traffic sent to one TCP/UDP port to be redirected to another—even across different machines. This also another way the loopback address can be utilized. For example, in the scenario below, a Windows host forwards its local port 8888 to a Linux VM's SSH port (22). The Linux machine is running as a [virtual machine with NAT](https://www.virtualbox.org/manual/ch06.html#network_nat) enabled, meaning it does not have a directly accessible IP on the network. Instead, the Windows host acts as an intermediary, forwarding traffic to it.

![GIF showcasing the connection on an SSH service running on port 8888 through Command Prompt.](https://academy.hackthebox.com/storage/modules/289/Skills_Assessment/loopback-ssh.gif)

Note that the topic of port forwarding is beyond the scope of this module. However, it is certainly something to be aware of, and is a wonderful example of the power and possibilities available through computer networking. Now that we've investigated the `lo` interface (and the `ens3` interface in the process), `tun0` is all that remains. And with that, we conclude chapter one.

---

`Chapter 2. - Having Tuns of Fun`

**→ Click to Show ←**

---

[](https://academy.hackthebox.com/vpn)

---

`Chapter 3. - Target Acquired`

**→ Click to Show ←**

---

[](https://en.wikipedia.org/wiki/Netcat)

[](https://www.geeksforgeeks.org/difference-between-active-and-passive-ftp/)

||||
|---|---|---|
||||
||||

[](http://www.faqs.org/rfcs/rfc959.html)

---

[](https://en.wikipedia.org/wiki/List_of_HTTP_header_fields)

VPN Servers

Warning: Each time you "Switch", your connection keys are regenerated and you must re-download your VPN connection file.

All VM instances associated with the old VPN Server will be terminated when switching to a new VPN server.  
Existing PwnBox instances will automatically switch to the new VPN server.

                                                             EU Academy 2                                                             US Academy 6                                                             US Academy 4                                                             US Academy 1                                                             US Academy 2                                                             EU Academy 3                                                             EU Academy 1                                                             US Academy 3                                                             EU Academy 6                                                             US Academy 5                                                             EU Academy 5                                                             EU Academy 4                                                     

US Academy 5

low Load

PROTOCOL

UDP 1337

TCP 443

DOWNLOAD VPN CONNECTION FILE

# Connect to Pwnbox

Your own web-based Parrot Linux instance to play our labs.

Pwnbox Location

UK175ms

Terminate Pwnbox to switch location

Start Instance

 / 1 spawns left

Waiting to start...

Enable step-by-step solutions for all questions![sparkles-icon-decoration](https://academy.hackthebox.com/images/sparkles-solid.svg)

#### Questions

Answer the question(s) below to complete this Section and earn cubes!

Target(s): Click here to spawn the target system!  

[

Download VPN Connection File

](https://academy.hackthebox.com/vpn/key)

+ 2  What IPv4 address is used when a host wants to send and receive network traffic to itself?

+10 Streak pts

 Submit

+ 2  What is the the name of the Program listening on localhost:5901 of the Pwnbox?

+10 Streak pts

 Submit

+ 2  Which network interface allows us to interact with target machines in the HTB lab environment?

+10 Streak pts

 Submit

+ 2  What is the FTP command used to retrieve a file? (Format: XXXX)

+10 Streak pts

 Submit

+ 2  Bypass the request filtering found on the target machine's HTTP service, and submit the flag found in the response. The flag will be in the format: HTB{...}

+10 Streak pts

 Submit

 [Previous](https://academy.hackthebox.com/module/289/section/3245)

 [Go to Questions](https://academy.hackthebox.com/module/289/section/3246#questionsDiv)

##### Table of Contents

###### Networking Fundamentals

  [Introduction to Networks](https://academy.hackthebox.com/module/289/section/3235)  [Network Concepts](https://academy.hackthebox.com/module/289/section/3236)  [Components of a Network](https://academy.hackthebox.com/module/289/section/3237)

###### Network Communication and Addressing

  [Network Communication](https://academy.hackthebox.com/module/289/section/3238)  [Dynamic Host Configuration Protocol (DHCP)](https://academy.hackthebox.com/module/289/section/3239)  [Network Address Translation (NAT)](https://academy.hackthebox.com/module/289/section/3240)  [Domain Name System (DNS)](https://academy.hackthebox.com/module/289/section/3241)

###### Internet Architecture and Wireless Technologies

  [Internet Architecture](https://academy.hackthebox.com/module/289/section/3242)  [Wireless Networks](https://academy.hackthebox.com/module/289/section/3243)

###### Network Security and Data Flow Analysis

  [Network Security](https://academy.hackthebox.com/module/289/section/3244)  [Data Flow Example](https://academy.hackthebox.com/module/289/section/3245)

###### Skills Assessment

  [Skills Assessment](https://academy.hackthebox.com/module/289/section/3246)

##### My Workstation

OFFLINE

  Start Instance

 / 1 spawns left

Powered by   [![Hack The Box logo](https://academy.hackthebox.com/images/logo-htb.svg)](https://www.hackthebox.com/)

![](https://downloads.intercomcdn.com/i/o/369812/618f59eb6f3b1f6de8e11a97/efef1192e4fa386f159825fbf792ed52.png)