The Internet relies on a system of numerical addresses, known as IP addresses, to route data from one device to another. The original addressing scheme, IPv4, offers a finite number of IP addresses (approximately 4.3 billion). Although this might sound like a lot, the explosive growth of the internet has meant these addresses are in short supply. One solution to this insufficiency issue is `Network Address Translation (NAT)`. The idea is that `NAT` allows multiple devices on a private network to share a single public IP address. This not only helps conserve the limited pool of public IP addresses but also adds a layer of security to the internal network.

#### Private vs. Public IP Addresses

`Public IP` addresses are globally unique identifiers assigned by Internet Service Providers (ISPs). Devices equipped with these IP addresses can be accessed from anywhere on the Internet, allowing them to communicate across the global network. For example, the IP address 8.8.8.8 is used for Google's DNS server, and 142.251.46.174 identifies one of Google’s web servers. These addresses ensure that devices can uniquely identify and reach each other over the internet.

`Private IP` addresses are designated for use within local networks such as homes, schools, and offices. These addresses are not routable on the global internet, meaning packets sent to these addresses are not forwarded by internet backbone routers. Defined by RFC 1918, common IPv4 private address ranges include 10.0.0.0 to 10.255.255.255, 172.16.0.0 to 172.31.255.255, and 192.168.0.0 to 192.168.255.255. This setup ensures that these private networks operate independently of the internet while facilitating internal communication and device connectivity.

Private IP addresses contribute to conserving public IP addresses. Using Network Address Translation (NAT), a local network can utilize private IP addresses while sharing a single public IP address, reducing the number of public IPs needed. This setup makes devices accessible from the internet without using multiple public addresses. Additionally, private IPs help secure the network by isolating internal devices from direct exposure to the internet, protecting them from potential external threats.

#### What is NAT?

`Network Address Translation (NAT)` is a process carried out by a router or a similar device that modifies the source or destination IP address in the headers of IP packets as they pass through. This modification is used to translate the private IP addresses of devices within a local network to a single public IP address that is assigned to the router.

#### How NAT Works

Consider a home network with several devices, such as a laptop, a smartphone, and a gaming console, each assigned a unique private IP address: the laptop at 192.168.1.10, the smartphone at 192.168.1.11, and the gaming console at 192.168.1.12. The home router managing this network has two critical interfaces. The LAN (Local Area Network) interface connects to the private network with an IP address of 192.168.1.1, while the WAN (Wide Area Network) interface, connected to the ISP’s network, carries a public IP address, 203.0.113.50.

The process of NAT translation begins when a device, say the laptop, sends a request to visit a website like [www.google.com](http://www.google.com/). This request packet, originating with the private IP of 192.168.1.10, is sent to the router. Here, the NAT function of the router modifies the source IP in the packet header from the private IP to the public IP of the router, 203.0.113.50. This packet then travels across the internet to reach the intended web server. Upon receiving the packet, the web server sends a response back to the router's public IP. As the response arrives, the router's NAT table, which keeps track of IP mappings, identifies that 203.0.113.50:4444 corresponds to the laptop at 192.168.1.10:5555 (ports 4444 and 5555 are dynamic). The router then translates the public IP back to the laptop’s private IP and forwards the internal response to the laptop, completing the communication cycle.

![Network diagram: LAN with PC, printer, smartphone, and laptop (private IPs 192.168.1.10 to 192.168.1.13) connected to a router/NAT with public IP 203.0.113.50, linked to a remote server with public IP 503.0.135.60. NAT modifies host and destination IPs.](https://academy.hackthebox.com/storage/modules/289/NAT/NAT-2.png)

#### Types of NAT

To better understand Network Address Translation (NAT), It's helpful to know that there are several types of Network Address Translation (NAT), each designed for specific networking needs. Below are the different types of NAT.

|**Type**|**Description**|
|---|---|
|`Static NAT`|Involves a one-to-one mapping, where each private IP address corresponds directly to a public IP address.|
|`Dynamic NAT`|Assigns a public IP from a pool of available addresses to a private IP as needed, based on network demand.|
|`Port Address Translation (PAT)`|Also known as NAT Overload, is the most common form of NAT in home networks. Multiple private IP addresses share a single public IP address, differentiating connections by using unique port numbers. This method is widely used in home and small office networks, allowing multiple devices to share a single public IP address for internet access.|

#### Benefits and Trade-Offs

Network Address Translation (NAT) offers a number of benefits and presents some trade-offs as well.

|**Benefits**|
|---|
|Conserves the limited IPv4 address space.|
|Provides a basic layer of security by not exposing internal network structure directly.|
|Flexible for internal IP addressing schemes.|

| **Trade-Offs**                                                                                                         |
| ---------------------------------------------------------------------------------------------------------------------- |
| Complex services like hosting a public server behind NAT can require additional configuration (e.g., port forwarding). |
| NAT can break certain protocols that rely on end-to-end connectivity without special handling.                         |
| Adds complexity to troubleshooting connectivity issues.                                                                |


What type of NAT allows multiple private IP addresses to share one public IP address using unique port numbers?

+ Port Address Translation

What RFC specifies private IP ranges?

+ RFC 1918

Which NAT type involves a one-to-one mapping of private IP addresses to public IP addresses?

+ Static NAT

What type of NAT assigns a public IP from a pool as needed?

+ Dynamic NAT

What device typically performs NAT in a home network?

+ Router

