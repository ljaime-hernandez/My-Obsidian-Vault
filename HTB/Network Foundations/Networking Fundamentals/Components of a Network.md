As we continue our journey into infosec, understanding the various components that formulate a network is essential. We know that currently, devices are able to communicate with each other, share resources, and access the internet with almost uniform consistency. What exactly facilitates this? The primary components of such a network include:

| **Component**                           | **Description**                                           |
| --------------------------------------- | --------------------------------------------------------- |
| `End Devices`                           | Computers, Smartphones, Tablets, IoT / Smart Devices      |
| `Intermediary Devices`                  | Switches, Routers, Modems, Access Points                  |
| `Network Media and Software Components` | Cables, Protocols, Management and Firewalls Software      |
| `Servers`                               | Web Servers, File Servers, Mail Servers, Database Servers |

Let's explore each of these in detail.

## End Devices

An `end device`, also known as a `host`, is any device that ultimately ends up sending or receiving data within a network. Personal computers and smart devices (such as phones and smart TVs) are common end devices; users routinely interact with them directly to perform tasks like browsing the web, sending messages, and creating documents. In most networks, such devices play a crucial role in both data generation and data consumption, like when users stream videos or read web content. End devices serve as the primary user interface to the world wide web, enabling users to access network resources and services seamlessly, through both wired (Ethernet) and wireless (Wi-Fi) connections. Another typical example of this would be a student using a notebook to connect to a school’s Wi-Fi network, allowing them to access online learning materials, submit assignments, and communicate with instructors.

## Intermediary Devices

An `intermediary device` has the unique role of facilitating the flow of data between `end devices`, either within a local area network, or between different networks. These devices include routers, switches, modems, and access points - all of which play crucial roles in ensuring efficient and secure data transmission. Intermediary devices are responsible for `packet forwarding`, directing data packets to their destinations by reading network address information and determining the most efficient paths. They connect networks and control traffic to enhance performance and reliability. By managing data flow with protocols, they ensure smooth transmission and prevent congestion. Additionally, intermediary devices often incorporate security features like `firewalls` to protect certain networks from unauthorized access and potential threats. Operating at different layers of the OSI model—for example, routers at the `Network Layer (Layer 3)` and switches at the `Data Link Layer (Layer 2)`—use routing tables and protocols to make informed decisions about data forwarding . A common example is a home network where intermediary devices like routers and switches connect all household devices—including notebooks, smartphones, and smart TVs—to the internet, enabling communication and access to online resources.

#### Network Interface Cards (NICs)

A `Network Interface Card (NIC)` is a hardware component installed in a computer, or other device, that enables connection to a network. It provides the physical interface between the device and the network media, handling the sending and receiving of data over the network. Each NIC has a unique Media Access Control (MAC) address, which is essential for devices to identify each other, and facilitate communication at the data link layer. NICs can be designed for wired connections, such as Ethernet cards that connect via cables, or for wireless connections, like Wi-Fi adapters utilizing radio waves. For example, a desktop computer might use a wired NIC, along with an Ethernet cable, to connect to a local network, while a laptop uses a wireless NIC to connect via Wi-Fi.

#### Routers

A `router` is an intermediary device that plays a hugely important role - the forwarding of data packets between networks, and ultimately directing internet traffic. Operating at the network layer (Layer 3) of the OSI model, routers read the network address information in data packets to determine their destinations. They use routing tables and routing protocols—such as `Open Shortest Path First (OSPF)` or `Border Gateway Protocol (BGP)`—to find the most efficient path for data to travel across interconnected networks, including the internet.

They fulfill this role by `examining incoming data packets` and forwarding them toward their destinations, based on IP addresses. By `connecting multiple networks`, routers enable devices on different networks to communicate. They also manage network traffic by selecting optimal paths for data transmission, which helps prevent congestion—a process known as `traffic management`. Additionally, routers enhance `security` by incorporating features like firewalls and access control lists, protecting the network from unauthorized access and potential threats.

#### Example

In a home network, a router connects all household devices—such as computers, smartphones, and smart TVs—to the internet provided by an Internet Service Provider (ISP). The router directs incoming and outgoing internet traffic efficiently, ensuring that each device can communicate with external networks and with each other.

#### Switches

The `switch` is another integral component, with it's primary job being to connect multiple devices within the same network, typically a Local Area Network (LAN). Operating at the data link layer (Layer 2) of the OSI model, switches use MAC addresses to forward data only to the intended recipient. By managing data traffic between connected devices, switches reduce network congestion and improve overall performance. They enable devices like computers, printers, and servers to communicate directly with each other within the network. For instance, in a corporate office, switches connect employees' computers, allowing for quick file sharing and access to shared resources like printers and servers.

#### Hubs

A `hub` is a basic (and now antiquated) networking device. It connects multiple devices in a network segment and broadcasts incoming data to all connected ports, regardless of the destination. Operating at the physical layer (Layer 1) of the OSI model, hubs are simpler than switches and do not manage traffic intelligently. This indiscriminate data broadcasting can lead to network inefficiencies and collisions, making hubs less suitable for modern networks. For example, in a small home network setup from earlier times, a hub might connect a few computers, but today, switches are preferred due to their better performance and efficiency.

## Network Media and Software Components

`Network Media and Software Components` are vital elements that enable seamless communication and operation within a network. `Network media`, such as cables and wireless signals, provide the physical pathways that connect devices and allow data to be transmitted between them. This includes wired media like Ethernet cables and fiber-optic cables, which offer high-speed connections, as well as wireless media like Wi-Fi and Bluetooth, which provide mobility and flexibility. On the other hand, `software components` like network protocols and management software define the rules and procedures for data transmission, ensuring that information is correctly formatted, addressed, transmitted, routed, and received. `Network protocols` such as TCP/IP, HTTP, and FTP enable devices to communicate over the network, while `network management software` allows administrators to monitor network performance, configure devices, and enhance security through tools like software firewalls.

#### Cabling and Connectors

`Cabling and connectors` are the physical materials used to link devices within a network, forming the pathways through which data is transmitted. This includes the various types of cables mentioned previously, but also connectors like the RJ-45 plug, which is used to interface cables with network devices such as computers, switches, and routers. The quality and type of cabling and connectors can affect network performance, reliability, and speed. For example, in an office setting, Ethernet cables with RJ-45 connectors might connect desktop computers to network switches, enabling high-speed data transfer across the local area network.

#### Network Protocols

`Network protocols` are the set of rules and conventions that control how data is formatted, transmitted, received, and interpreted across a network. They ensure that devices from different manufacturers, and with varying configurations, can adhere to the same standard and communicate effectively. Protocols encompass a wide range of aspects. such as:

- `Data Segmentation`
- `Addressing`
- `Routing`
- `Error Checking`
- `Synchronization`

Common network protocols include:

- `TCP/IP`: ubiquitous across all internet communications
    
- `HTTP/HTTPS`: The standard for Web traffic
    
- `FTP`: File transfers
    
- `SMTP` : Email transmissions
    
    For instance, when we browse a website, the HTTP or HTTPS protocol dictates how our browser communicates with the webserver to request and receive web pages, ensuring that the data is correctly formatted and securely transmitted.`
    

#### Network Management Software

`Network management software` consists of tools and applications used to monitor, control, and maintain network components and operations. These software solutions provide functionalities for:

- `performance monitoring`
- `configuration management`
- `fault analysis`
- `security management`

They help network administrators ensure that the network operates efficiently, remains secure, and can quickly address any issues that arise. For example, in a corporate environment, the IT department might use network management software to oversee all devices connected to the company network, monitor traffic for unusual activity, update device configurations remotely, and enforce security policies, maintaining optimal network performance and security.

#### Software Firewalls

A `software firewall` is a security application installed on individual computers or devices that monitors and controls incoming and outgoing network traffic based on predetermined security rules. Unlike hardware firewalls that protect entire networks, software firewalls (also called Host-based firewalls) provide protection at the device level, guarding against threats that may bypass the network perimeter defenses. They help prevent unauthorized access, reject incoming packets that contain suspicious or malicious data, and can be configured to restrict access to certain applications or services. For example, most operating systems include a built-in software firewall that can be set up to block incoming connections from untrusted sources, ensuring that only legitimate network traffic reaches the device.

_**The Linux-based software firewall [IPTables](https://linux.die.net/man/8/iptables) being used to drop incoming ICMP traffic.**_ ![GIF showcasing the ping command against a host without a firewall and one with firewall enabled.](https://academy.hackthebox.com/storage/modules/289/Components_of_a_Network/software-firewall.gif)

## Servers

A `server` is a powerful computer designed to provide services to other computers, known as clients, over a network. Servers are the backbone behind websites, email, files, and applications. In the realm of computer networking, servers play a crucial role by hosting services that clients access—for example, web pages and email services—facilitating `service provision`. They enable `resource sharing` by allowing multiple users to access resources like files and printers. Servers also handle `data management` by storing and managing data centrally, which simplifies backup processes and enhances security management. Additionally, they manage `authentication` by controlling user access and permissions, across multiple components in the network. Servers often run specialized operating systems optimized for handling multiple, simultaneous requests in what is known as the `Client-Server Model`, where the server waits for requests from clients and responds accordingly. Whether you knew it or not, this is what was happening under-the-hood the last time you accessed a website from your notebook. Your browser sends a request to the web server hosting the site, and the server subsequently processes the request and sends back the web page data in its response.

## Conclusion

As we have seen, the technology stack needed for world-wide computer networking requires multiple components. End devices are the users' primary interface with the network, intermediary devices manage data traffic and connectivity, and servers provide resources and services. Together, they enable the seamless flow of information that powers modern communication.



What type of network cable is used to transmit data over long distances with minimal signal loss?

+ fiber-optic

Which protocol manages data routing and delivery across networks?

+ TCP/IP

What software is used to oversee and administer network operations? (Format: 3 words)

+ Network management software

What software is used to protect individual devices from unauthorized network access? (Format: 1 word)

+ Firewall

What type of cable is used to connect components within a local area network for high-speed data transfer?

+ Ethernet

Which device connects multiple networks and manages data traffic to optimize performance?

+ Router

