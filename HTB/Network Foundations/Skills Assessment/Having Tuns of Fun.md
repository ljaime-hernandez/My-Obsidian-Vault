At the beginning of chapter one, we mentioned that the Pwnbox is used to interact with target machines in our lab environments. At the end of chapter one, we successfully investigated two out of the three available network interfaces:

- Loopback (lo): Allows the Pwnbox to send network traffic to itself.
- Public IP (ens3): Enables the Pwnbox to communicate with us over the internet.

That leaves one remaining interface: tun0. Based on its name, we can infer that it’s a tunnel interface, commonly associated with VPNs (Virtual Private Networks). Since lab targets exist on a separate private network, the Pwnbox must establish a secure connection to that environment, enabling us to reach them.

Let's confirm this by checking which route the Pwnbox takes to communicate with the target. Scroll to the end of this section and press `Click here to spawn the target system!`. After a few moments, a target machine will spawn, and we will be given its IP address.

Then, return to the Pwnbox and enter the following command into the Parrot Terminal:

Code: shell

```shell
ip route get <target ip>
```

This command will display the route taken for any traffic sent from the Pwnbox to reach the target.

  Having Tuns of Fun

```shell-session
┌──[eu-academy-1]─[10.10.14.21]─[htb-ac-594497@htb-5mix2gkv1a]─[~]
└──╼ [★]$ ip route get 10.129.233.197
  
10.129.233.197 via 10.10.14.1 dev tun0 src 10.10.14.21 uid 1002 
    cache 
```

Our theory has been confirmed—all traffic to the target is routed through `tun0`, a VPN tunnel that connects the Pwnbox to the private lab network. This allows us to interact with lab machines as if they were on the same local network. By using a VPN configuration file and software such as `OpenVPN`, computers will connect to the VPN server, which provides access to the network. HTB Academy's VPN is available to download at [https://academy.hackthebox.com/vpn](https://academy.hackthebox.com/vpn), for those who prefer to use their own workstation rather than Pwnbox.

Let's begin our first interaction with the target machine. We typically always begin by using `ping`. The `ping` is a networking utility used to test the reachability of a host on a network. It does not use TCP or UDP ports, making it a `Layer 3` protocol in terms of the OSI model. Type the following command into your terminal and observe the output.

Code: shell

```shell
ping -c 4 <target ip>
```

Here, we are sending four pings towards our target. Note that in Linux, if we do not specify the ping count, it will send pings `indefinitely` until we press `Ctrl + C` into the terminal.

  Having Tuns of Fun

```shell-session
┌─[eu-academy-1]─[10.10.14.21]─[htb-ac-594497@htb-5mix2gkv1a]─[~]
└──╼ [★]$ ping -c4 10.129.233.197
  
PING 10.129.233.197 (10.129.233.197) 56(84) bytes of data.
64 bytes from 10.129.233.197: icmp_seq=1 ttl=127 time=71.6 ms
64 bytes from 10.129.233.197: icmp_seq=2 ttl=127 time=71.3 ms
64 bytes from 10.129.233.197: icmp_seq=3 ttl=127 time=71.8 ms
64 bytes from 10.129.233.197: icmp_seq=4 ttl=127 time=71.2 ms

--- 10.129.233.197 ping statistics ---
4 packets transmitted, 4 received, 0% packet loss, time 3003ms
rtt min/avg/max/mdev = 71.205/71.470/71.776/0.223 ms
```

We've now confirmed that the target is reachable from our attack host. A few things to take note of are the `ttl` and the `time`. The `ttl` , which stands for `time-to-live`, tells us how many "hops" our packets are allowed to take in order to reach the target. This rarely applies to devices on the same local network as us, but rather devices on the internet that require our packets to hop through multiple routers in order to reach their intended destination. The information next to `time` gives us an idea of how much latency there is on the network. In this example above, we see each ping takes roughly `71 milliseconds`.

We have just confirmed that our attack host can communicate with the target, by `pinging the IP address of the target machine`. Our next step is to enumerate open TCP/UDP ports on the machine. Just as we used the `netstat` utility to view the open ports on the Pwnbox, there is another tool we can use to determine the open ports on a remote machine. This tool is `nmap`, and it is absolutely fundamental for any current or aspiring infosec professional. Let's begin by enumerating the open TCP ports on our target. Enter the following command into your terminal.

Code: shell

```shell
nmap <target IP>
```

After a few moments, we will see `nmap` return the open TCP ports present on the target.

  Having Tuns of Fun

```shell-session
┌─[eu-academy-1]─[10.10.14.21]─[htb-ac-594497@htb-5mix2gkv1a]─[~]
└──╼ [★]$ nmap 10.129.233.197
  
Starting Nmap 7.94SVN ( https://nmap.org ) at 2025-02-08 18:07 CST
Nmap scan report for 10.129.233.197
Host is up (0.073s latency).
Not shown: 993 closed tcp ports (reset)
PORT     STATE SERVICE
21/tcp   open  ftp
80/tcp   open  http
135/tcp  open  msrpc
139/tcp  open  netbios-ssn
445/tcp  open  microsoft-ds
3389/tcp open  ms-wbt-server
5357/tcp open  wsdapi
```

We see several open ports available. Ports `135`,`139`,and `445` will typically always be open on a Windows-based host. Port `3389` is the port used for Remote Desktop Protocol, or RDP for short. It is another common service seen on Windows machines. Port `5357` is used for `Microsoft's Web Services for Devices API` - another Windows protocol, used for device discovery on the network.

We've now come to a thorough understanding of the three network interfaces of the Pwnbox, and have verified that we can interact with the target machine. Here, we will conclude chapter two. In our third and final chapter, we will interact with our target machine using specific ports and protocols.