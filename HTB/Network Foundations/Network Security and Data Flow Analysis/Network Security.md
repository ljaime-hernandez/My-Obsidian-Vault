In networking, the term security refers to the measures taken to protect data, applications, devices, and systems within this network from unauthorized access or damage. The goal is to uphold and maintain the `CIA triad`:

|**Principle**|**Description**|
|---|---|
|`Confidentiality`|Only authorized users can view the data.|
|`Integrity`|The data remains accurate and unaltered.|
|`Availability`|Network resources are accessible when needed.|

In the next paragraphs, we will discuss two critical components of network security: `Firewalls` and `Intrusion Detection/Prevention Systems (IDS/IPS)`.

## Firewalls

A `Firewall` is a network security device, either hardware, software, or a combination of both, that monitors incoming and outgoing network traffic. Firewalls enforce a set of rules (known as `firewall policies` or `access control lists`) to determine whether to `allow` or `block` specific traffic. We can imagine a firewall as a security guard at the entrance of a building, checking who is allowed in or out based on a list of rules. If a visitor doesn’t meet the criteria (e.g., not on the guest list), they are denied entry.

_**The open source router/firewall [pfSense](https://www.pfsense.org/). It's large number of plugins (known as "Packages") give it a range of capabilities.**_ ![GIF showcasing the firewall rule creation in pfSense.](https://academy.hackthebox.com/storage/modules/289/Internet_Security/pfsense.gif)

Firewalls operate by analyzing packets of data according to predefined rules and policies, commonly focusing on factors such as IP addresses, port numbers, and protocols. This process, known as traffic filtering, is defined by system administrators as permitting or denying traffic based on specific conditions, ensuring that only authorized connections are allowed. Additionally, firewalls can log traffic events and generate alerts about any suspicious activity. Below are some of the different types of firewalls.

#### 1. Packet Filtering Firewall

|**Description**|
|---|
|Operates at Layer 3 (Network) and Layer 4 (Transport) of the OSI model.|
|Examines source/destination IP, source/destination port, and protocol type.|
|`Example`: A simple router ACL that only allows HTTP (port 80) and HTTPS (port 443) while blocking other ports.|

#### 2. Stateful Inspection Firewall

|**Description**|
|---|
|Tracks the state of network connections.|
|More intelligent than packet filters because they understand the entire conversation.|
|`Example`: Only allows inbound data that matches an already established outbound request.|

#### 3. Application Layer Firewall (Proxy Firewall)

|**Description**|
|---|
|Operates up to Layer 7 (Application) of the OSI model.|
|Can inspect the actual content of traffic (e.g., HTTP requests) and block malicious requests.|
|`Example`: A web proxy that filters out malicious HTTP requests containing suspicious patterns.|

#### 4. Next-Generation Firewall (NGFW)

|**Description**|
|---|
|Combines stateful inspection with advanced features like deep packet inspection, intrusion detection/prevention, and application control.|
|`Example`: A modern firewall that can block known malicious IP addresses, inspect encrypted traffic for threats, and enforce application-specific policies.|

Firewalls stand between the internet and the internal network, examining traffic before letting it through. In a home environment, our router/modem often has a built-in firewall (software-based). In that case, it’s all in one device, and the firewall function is `inside` the router. In larger networks (e.g., business environments), the firewall is often a separate device placed after the modem/router and before the internal network, ensuring all traffic must pass through it.

![Network diagram: Internet connects to Firewall, then Router/Modem, linking to Laptop, PC, Smartphone.](https://academy.hackthebox.com/storage/modules/289/Internet_Security/Firewall-1.png)

## Intrusion Detection and Prevention Systems (IDS/IPS)

Intrusion Detection and Prevention Systems (IDS/IPS) are security solutions designed to monitor and respond to suspicious network or system activity. An Intrusion Detection System (IDS) observes traffic or system events to identify malicious behavior or policy violations, generating alerts but not blocking the suspicious traffic. In contrast, an Intrusion Prevention System (IPS) operates similarly to an IDS but takes an additional step by preventing or rejecting malicious traffic in real time. The key difference lies in their actions: an IDS detects and alerts, while an IPS detects and prevents.

_**The widely used [Suricata](https://suricata.io/) software can function as both an IDS and an IPS. Here, we see the user enable a detection rule, then begin inline monitoring.**_ ![GIF showcasing the rule enablement in Suricata.](https://academy.hackthebox.com/storage/modules/289/Internet_Security/suricata-2.gif)

Both IDS and IPS solutions analyze network packets and compare them to known attack signatures or typical traffic patterns. This process involves:

|**Techniques**|**Description**|
|---|---|
|`Signature-based detection`|Matches traffic against a database of known exploits.|
|`Anomaly-based detection`|Detects anything unusual compared to normal activity.|

When suspicious or malicious behavior is identified, an IDS will generate an alert for further investigation, while an IPS goes one step further by blocking or rejecting the malicious traffic in real time.

_**Suricata in IDS mode.**_ ![GIF showcasing Suricata in IDS mode.](https://academy.hackthebox.com/storage/modules/289/Internet_Security/suricata-3.gif)

Below are some of the different types of firewalls IDS/IPS.

#### 1. Network-Based IDS/IPS (NIDS/NIPS)

|**Description**|
|---|
|Hardware device or software solution placed at strategic points in the network to inspect all passing traffic.|
|`Example`: A sensor connected to the core switch that monitors traffic within a data center.|

#### 2. Host-Based IDS/IPS (HIDS/HIPS)

|**Description**|
|---|
|Runs on individual hosts or devices, monitoring inbound/outbound traffic and system logs for suspicious behavior on that specific machine.|
|`Example`: An antivirus or endpoint security agent installed on a server.|

IDS/IPS can be placed at several strategic locations in a network. One option is to position them behind the firewall, where the firewall filters obvious threats, and the IDS/IPS inspects any remaining traffic. Another common placement is in the DMZ (Demilitarized Zone), a separate network segment within the larger network directly exposed to the internet, where they monitor traffic moving in and out of publicly accessible servers. Finally, IDS/IPS solutions can also run directly on endpoint devices, such as servers or workstations, to detect suspicious activity at the host level. The following diagram shows an IDS/IPS positioned after the firewall.

![Network diagram: Internet connects to Firewall, then IPS/IDS, Router/Modem, linking to Laptop, PC, Smartphone.](https://academy.hackthebox.com/storage/modules/289/Internet_Security/IPS_IDS-1.png)

## Best Practices

Here are the best practices for enhancing network security, summarized in the following table:

|**Practice**|**Description**|
|---|---|
|`Define Clear Policies`|Consistent firewall rules based on the principle of `least privilege` (only allow what is necessary).|
|`Regular Updates`|Keep firewall, IDS/IPS signatures, and operating systems up to date to defend against the latest threats.|
|`Monitor and Log Events`|Regularly review firewall logs, IDS/IPS alerts, and system logs to identify suspicious patterns early.|
|`Layered Security`|Use `defense in depth` (a strategy that leverages multiple security measures to slow down an attack) with multiple layers: Firewalls, IDS/IPS, antivirus, and endpoint protection to cover different attack vectors.|
|`Periodic Penetration Testing`|Test the effectiveness of the security policies and devices by simulating real attacks.|


What device monitors network traffic and enforces rules to allow or block specific traffic?

 + Firewall

Which type of firewall operates at the network and transport layers of the OSI model? (Format: two words)

+ Packet Filtering

What advanced feature does a Next-Generation Firewall include beyond stateful inspection? (Format: three words)

+ deep packet inspection

Which system generates alerts for suspicious network activity without blocking it? (Format: acronym)

+ IDS

Which system not only detects but also prevents suspicious network activity by blocking it? (Format: acronym)

+ IPS

What detection method involves comparing network traffic against a database of known exploits? (Format: three words)

+ Signature-based detection

