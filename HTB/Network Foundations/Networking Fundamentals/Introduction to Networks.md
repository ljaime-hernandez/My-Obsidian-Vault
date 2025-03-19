Welcome to Network Foundations! In this introductory module, we will explore the technology behind computer networking - also known as "networking" or "networks" - and why it is essential to our lives. We will mostly focus on two primary types of networks: `Local Area Networks (LANs)` and `Wide Area Networks (WANs)`.

Understanding how devices are able communicate with one another, from inside our homes to across the globe, is fundamental knowledge for those looking to enter the field of cyber security. The interconnectedness of almost every device in our world today is what sets the backdrop for the ever increasing demand for security professionals.

## What is a Network?

A `network` is a collection of interconnected devices that can communicate - sending and receiving data, and also sharing resources with each other. These individual endpoint devices, often called `nodes`, include computers, smartphones, printers, and servers. However, `nodes` alone do not comprise the entire network. The table below shows some networking `key concepts`.

|**Concepts**|**Description**|
|---|---|
|`Nodes`|Individual devices connected to a network.|
|`Links`|Communication pathways that connect nodes (wired or wireless).|
|`Data Sharing`|The primary purpose of a network is to enable data exchange.|

Let's explain the above using a real-world example. Think of a group of friends chatting in a room. Each person represents a device (`node`), and their ability to talk and listen represents the `communication links`. The conversation is the `data` being shared.

## Why Are Networks Important?

Networks, particularly since the advent of the Internet, have radically transformed society, enabling a multitude of possibilities that are now essential to our lives. Below are just a few of the benefits afforded to us by this incredible technology.

|**Function**|**Description**|
|---|---|
|`Resource Sharing`|Multiple devices can share hardware (like printers) and software resources.|
|`Communication`|Instant messaging, emails, and video calls rely on networks.|
|`Data Access`|Access files and databases from any connected device.|
|`Collaboration`|Work together in real-time, even when miles apart.|

## Types of Networks

Networks vary in size and scope. The two primary types are `Local Area Network (LAN)` and `Wide Area Network (WAN)`.

#### Local Area Network (LAN)

A `Local Area Network (LAN)` connects devices over a short distance, such as within a home, school, or small office building. Here are some of its key characteristics:

|**Characteristic**|**Description**|
|---|---|
|`Geographical Scope`|Covers a small area.|
|`Ownership`|Typically owned and managed by a single person or organization.|
|`Speed`|High data transfer rates.|
|`Media`|Uses wired (Ethernet cables) or wireless (Wi-Fi) connections.|

The following diagram shows how a home's Wi-Fi connects devices such as laptops, smartphones, and smart TVs, allowing them to share files and access the internet.

![Network diagram showing Internet connected to a modem, then a router, with wired and Wi-Fi connections to a PC, laptop, smartphone, and printer.](https://academy.hackthebox.com/storage/modules/289/introduction/lan_1-1.png)

#### Wide Area Network (WAN)

A `Wide Area Network (WAN)` spans a large geographical area, connecting multiple LANs. Below are some of its key characteristics:

|**Characteristic**|**Description**|
|---|---|
|`Geographical Scope`|Covers cities, countries, or continents.|
|`Ownership`|Often a collective or distributed ownership (e.g., internet service providers).|
|`Speed`|Slower data transfer rates compared to LANs due to long-distance data travel.|
|`Media`|Utilizes fiber optics, satellite links, and leased telecommunication lines.|

The Internet is the largest example of a `WAN`, connecting millions of `LANs` globally.

![Network diagram showing three identical setups: Internet to modem to router, with wired and Wi-Fi connections to PC, laptop, smartphone, and printer.](https://academy.hackthebox.com/storage/modules/289/introduction/wan-2.png)

#### Comparing LAN and WAN

|Aspect|LAN|WAN|
|---|---|---|
|`Size`|Small, localized area|Large, broad area|
|`Ownership`|Single person or organization|Multiple organizations/service providers|
|`Speed`|High|Lower compared to LAN|
|`Maintenance`|Easier and less expensive|Complex and costly|
|`Example`|Home or office network|The Internet|

## How Do LANs and WANs Work Together?

Local Area Networks (LANs) can connect to Wide Area Networks (WANs) to access broader networks beyond their immediate scope. This connectivity allows for expanded communication and resource sharing on a much larger scale.

For instance, when accessing the Internet, a home LAN connects to an `Internet Service Provider's (ISP's)` WAN, which grants Internet access to all devices within the home network. An ISP is a company that provides individuals and organizations with access to the Internet. In this setup, a device called a modem (modulator-demodulator) plays a crucial role. The modem acts as a bridge between your home network and the ISP's infrastructure, converting digital signals from your router into a format suitable for transmission over various media like telephone lines, cable systems, and fiber optics. This connection transforms a simple local network into a gateway to the resources available online.

In a business setting, companies link multiple office LANs via WANs to achieve unified communication and collaboration across different geographic locations. By connecting these LANs through a WAN, employees in various offices can share information, access centralized databases, and work together in real-time, enhancing productivity within the organization.

Let's consider the following scenario to illustrate how LANs and WANs work together. At home, our devices—such as laptops, smartphones, and tablets—connect to our home router, forming a LAN. This router doesn't just manage local traffic; it also communicates with our ISP's WAN. Through this connection to the WAN, our home network gains the ability to access websites and online services hosted all over the world. This seamless integration between the LAN and WAN enables us to reach global content and interact with services beyond our local network.



What is the term for a collection of interconnected devices that can communicate and share resources with each other?

+ network

In network terminology, what is the term for individual devices connected to a network?

+ nodes

What is the largest Wide Area Network (WAN) that connects millions of Local Area Networks (LANs) globally?

+ internet

 What is the acronym for a network that connects devices over a short distance, such as within a home, school, or small office building?

+ LAN

In networking, what term describes the communication pathways (wired or wireless) that connect nodes?

+ links