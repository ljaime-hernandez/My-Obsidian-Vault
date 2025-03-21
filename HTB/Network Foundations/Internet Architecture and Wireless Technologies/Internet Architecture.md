`Internet Architecture` describes how data is organized, transmitted, and managed across networks. Different architectural models serve different needs—some offer a straightforward client-server setup (like a website), while others rely on a more distributed approach (like file-sharing platforms). Understanding these models helps us see why networks are designed and operated the way they are. Different architectures solve different problems. Often, we see a combination of architectures creating hybrid models. Each model comes with its own set of trade-offs in terms of scalability, performance, security, and manageability. In the following paragraphs, we will describe the different architectures in more detail.

## Peer-to-Peer (P2P) Architecture

In a `Peer-to-Peer (P2P`) network, each node, whether it's a computer or any other device, acts as both a client and a server. This setup allows nodes to communicate directly with each other, sharing resources such as files, processing power, or bandwidth, without the need for a central server. P2P networks can be fully decentralized, with no central server involved, or partially centralized, where a central server may coordinate some tasks but does not host data.

Imagine a group of friends who want to share vacation photos with each other. Instead of uploading all the photos to a single website or server, each of them sets up a folder on their own computer that can be accessed by the others. They use a file-sharing program that connects their computers directly.

First, they install a Peer-to-Peer (P2P) file-sharing application on their computer. Then, they select the folder containing the vacation photos to share with the other friends. Everyone performs the same setup on their computers. Once everyone is connected through the P2P application, they can all browse and download photos directly from each other’s shared folders, allowing for a direct exchange of files without the need for a central server.

A popular example of Peer-to-Peer (P2P) architecture is torrenting, as seen with applications like BitTorrent. In this system, anyone who has the file, referred to as a `seeder`, can upload it, allowing others to download it from multiple sources simultaneously.

![Network diagram with interconnected devices: PC, laptop, smartphone, server, and printer.](https://academy.hackthebox.com/storage/modules/289/Internet_Arch_Models/P2P-1.png)

In the following table, we can see the advantages and disadvantages of a Peer-to-Peer architecture.

|**Advantage**|**Description**|
|---|---|
|`Scalability`|Adding more nodes can increase total resources (storage, CPU, etc.).|
|`Resilience`|If one node goes offline, others can continue functioning.|
|`Cost distribution`|Resource burden, like bandwidth and storage, is distributed among peers, making it more cost-efficient.|

|**Disadvantage**|**Description**|
|---|---|
|`Management complexity`|Harder to control and manage updates/security policies across all nodes|
|`Potential reliability issues`|If too many peers leave, resources could be unavailable.|
|`Security challenges`|Each node is exposed to potential vulnerabilities.|

## Client-Server Architecture

The `Client-Server` model is one of the most widely used architectures on the Internet. In this setup, clients, which are user devices, send requests, such as a web browser asking for a webpage, and servers respond to these requests, like a web server hosting the webpage. This model typically involves centralized servers where data and applications reside, with multiple clients connecting to these servers to access services and resources.

Let's assume we want to check the weather forecast on a website. We start by opening the web browser on our phone or computer, and proceed to type in the website's name, e.g., `weatherexample.com`. When we press enter, the browser sends a request over the Internet to the server that hosts `weatherexample.com`. This server, a powerful computer set up specifically to store the website’s data and handle requests, receives the query and processes it by locating the requested page. It then sends back the data (regarding the weather, we requested) to our browser, which receives this information and displays the webpage, allowing us to see the latest weather updates.

![Network diagram with Internet connected to clients (PC, laptop, smartphone) and servers.](https://academy.hackthebox.com/storage/modules/289/Internet_Arch_Models/Client_Server_Arch-1.png)

A key component of this architecture is the tier model, which organizes server roles and responsibilities into layers. This enhances scalability and manageability, as well as security and performance.

#### Single-Tier Architecture

In a `single-tier` architecture, the client, server, and database all reside on the same machine. This setup is straightforward but is rarely used for large-scale applications due to significant limitations in scalability and security.

#### Two-Tier Architecture

The `two-tier` architecture splits the application environment into a client and a server. The client handles the presentation layer, and the server manages the data layer. This model is typically seen in desktop applications where the user interface is on the user's machine, and the database is on a server. Communication usually occurs directly between the client and the server, which can be a database server with query-processing capabilities.

**Note:** In a typical web application, the client (browser) does not directly interact with the database server. Instead, the browser requests web pages from a **web server**, which in turn sends it's response (HTML, CSS, JavaScript) back to the browser for rendering. The web server *may* interact with an application server or database in order to formulate it's response, but in general, the scenario of a person visiting a website does not constitute a Two-Tier Architecture.

#### Three-Tier Architecture

A `three-tier` architecture introduces an additional layer between the client and the database server, known as the application server. In this model, the client manages the presentation layer, the application server handles all the business logic and processing, and the third tier is a database server. This separation provides added flexibility and scalability because each layer can be developed and maintained independently.

#### N-Tier Architecture

In more complex systems, an `N-tier` architecture is used, where `N` refers to any number of separate tiers used beyond three. This setup involves multiple levels of application servers, each responsible for different aspects of business logic, processing, or data management. N-tier architectures are highly scalable and allow for distributed deployment, making them ideal for web applications and services that demand robust, flexible solutions.

While tiered client-server architectures offer many improvements, they also introduce complexity in deployment and maintenance. Each tier needs to be correctly configured and secured, and communication between tiers must be efficient and secure to avoid performance bottlenecks and security vulnerabilities. In the following table, we can see the advantages and disadvantages of a Client-Server architecture in general.

|**Advantage**|**Description**|
|---|---|
|`Centralized control`|Easier to manage and update.|
|`Security`|Central security policies can be applied.|
|`Performance`|Dedicated servers can be optimized for their tasks.|

|**Disadvantage**|**Description**|
|---|---|
|`Single point of failure`|If the central server goes down, clients lose access.|
|`High Cost and Maintenance`|Setting up and sustaining a client-server architecture is expensive, requiring constant operation and expert management , making it costly to maintain.|
|`Network Congestion`|High traffic on the network can lead to congestion, slowing down or even disrupting connections when too many clients access the server simultaneously.|

## Hybrid Architecture

A `Hybrid` model blends elements of both `Client-Server` and `Peer-to-Peer (P2P)` architectures. In this setup, central servers are used to facilitate coordination and authentication tasks, while the actual data transfer occurs directly between peers. This combination leverages the strengths of both architectures to enhance efficiency and performance. The following example gives a high-level explanation of how a hybrid architecture works.

When we open a video conferencing app and log in, the credentials (username and password) are verified by central servers, which also manage the session by coordinating who is in the meeting and controlling access. Once we're logged in and the meeting begins, the actual video and audio data is transferred directly between our device and those of other participants, bypassing the central server to reduce lag and enhance video quality. This setup combines both models: it uses the central server for initial connection and control tasks, while the bulk of data transfer occurs in a peer-to-peer style, reducing the server load and leveraging direct, fast connections between peers. The following table refers to some of the advantages and disadvantages of a Hybrid Architecture.

![Network diagram with Internet connected to multiple devices: PC, laptop, smartphone, and server.](https://academy.hackthebox.com/storage/modules/289/Internet_Arch_Models/Hybrid_Architecture-1.png)

|**Advantage**|**Description**|
|---|---|
|`Efficiency`|Relieves workload from servers by letting peers share data.|
|`Control`|Central server can still manage user authentication, directory services, or indexing.|

| **Disadvantage**                    | **Description**                                                                           |
| ----------------------------------- | ----------------------------------------------------------------------------------------- |
| `Complex Implementation`            | Requires more sophisticated design to handle both centralized and distributed components. |
| `Potential Single Point of Failure` | If the central coordinating server fails, peer discovery might stop.                      |

## Cloud Architecture

`Cloud Architecture` refers to computing infrastructure that is hosted and managed by third-party providers, such as AWS, Azure, and Google Cloud. This architecture operates on a virtualized scale following a client-server model. It provides on-demand access to resources such as servers, storage, and applications, all accessible over the Internet. In this model, users interact with these services without controlling the underlying hardware.

![Cloud network diagram with components: Servers, Apps, Database, Storage connected to Internet, linking to devices: Laptop, PC, Smartphone.](https://academy.hackthebox.com/storage/modules/289/Internet_Arch_Models/Cloud_Arch-1.png)

Services like Google Drive or Dropbox are some examples of Cloud Architecture operating under the `SaaS` (Software as a Service) model, where we access applications over the internet without managing the underlying hardware. Below are five essential characteristics that define a Cloud Architecture.

| **Characteristic**          | **Description**                                                        |
| --------------------------- | ---------------------------------------------------------------------- |
| `1. On-demand self-service` | Automatically set up and manage the services without human help.       |
| `2. Broad network access`   | Access services from any internet-connected device.                    |
| `3. Resource pooling`       | Share and allocate service resources dynamically among multiple users. |
| `4. Rapid elasticity`       | Quickly scale services up or down based on demand.                     |
| `5. Measured service`       | Only pay for the resources you use, tracked with precision.            |

The below table shows some of the advantages and disadvantages of the Cloud Architecture.

|**Advantage**|**Description**|
|---|---|
|`Scalability`|Easily add or remove computing resources as needed.|
|`Reduced cost & maintenance`|Hardware managed by the cloud provider.|
|`Flexibility`|Access services from anywhere with Internet connectivity.|

|**Disadvantage**|**Description**|
|---|---|
|`Vendor lock-in`|Migrating from one cloud provider to another can be complex.|
|`Security/Compliance`|Relying on a third party for data hosting can introduce concerns about data privacy.|
|`Connectivity`|Requires stable Internet access.|

## 6. Software-Defined Architecture (SDN)

`Software-Defined Networking (SDN)` is a modern networking approach that separates the control plane, which makes decisions about where traffic is sent, from the data plane, which actually forwards the traffic. Traditionally, network devices like routers and switches housed both of these planes. However, in SDN, the control plane is centralized within a software-based controller. This configuration allows network devices to simply execute instructions they receive from the controller. SDN provides a programmable network management environment, enabling administrators to dynamically adjust network policies and routing as required. This separation makes the network more flexible and improves how it's managed.

![Network diagram: Remote Servers connect to Internet, then SDN Switches, SDN Controller, and Users, linking to Laptop, PC, Smartphone.](https://academy.hackthebox.com/storage/modules/289/Internet_Arch_Models/Software-Defined_Arch-1.png)

Large enterprises or cloud providers use SDN to dynamically allocate bandwidth and manage traffic flows according to real-time demands. Below is a table with the advantages and disadvantages of the Software-Defined architecture.

| **Advantage**                  | **Description**                                                                                             |
| ------------------------------ | ----------------------------------------------------------------------------------------------------------- |
| `Centralized control`          | Simplifies network management.                                                                              |
| `Programmability & Automation` | Network configurations can be changed quickly through software instead of manually configuring each device. |
| `Scalability & Efficiency`     | Can optimize traffic flows dynamically, leading to better resource utilization.                             |

| **Disadvantage**           | **Description**                                                               |
| -------------------------- | ----------------------------------------------------------------------------- |
| `Controller Vulnerability` | If the central controller goes down, the network might be adversely affected. |
| `Complex Implementation`   | Requires new skill sets and specialized software/hardware.                    |

## Key Comparisons

Below is a comparison table that outlines key characteristics of different network architectures

|`Architecture`|`Centralized`|`Scalability`|`Ease of Management`|`Typical Use Cases`|
|---|---|---|---|---|
|`P2P`|Decentralized (or partial)|High (as peers grow)|Complex (no central control)|File-sharing, blockchain|
|`Client-Server`|Centralized|Moderate|Easier (server-based)|Websites, email services|
|`Hybrid`|Partially central|Higher than C-S|More complex management|Messaging apps, video conferencing|
|`Cloud`|Centralized in provider’s infra|High|Easier (outsourced)|Cloud storage, SaaS, PaaS|
|`SDN`|Centralized control plane|High (policy-driven)|Moderate (needs specialized tools)|Datacenters, large enterprises|

## Conclusion

Each architecture has its unique benefits and challenges, and in practice, we often see these models blended to balance performance, scalability, and cost. Understanding these distinctions is important for anyone planning to set up or improve network systems.



What type of architecture allows nodes to act as both client and server?

+ Peer-to-Peer (P2P)

What architecture combines elements of both Client-Server and Peer-to-Peer models?

+ Hybrid

Which cloud service model involves accessing applications over the internet without managing the underlying infrastructure?

+ SaaS

In which architecture is the control plane separated from the data plane? (Format: two words, one of which is hyphenated)

+ Software-Defined Networking


Which architecture is known for decentralized data sharing without a central server?

+ Peer-to-Peer

What model is used by video conferencing apps to combine centralized coordination with peer-to-peer data transfer?

+ Hybrid

