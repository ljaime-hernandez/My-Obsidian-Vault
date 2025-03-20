The Domain Name System (DNS) is like the phonebook of the internet. It helps us find the right number (an IP address) for a given name (a domain such as `www.google.com`). Without DNS, we would need to memorize long, often complex IP addresses for every website we visit. DNS makes our lives easier by allowing us to use human-friendly names to access online resources.

#### Domain Names vs. IP Addresses

|**Address**|**Description**|
|---|---|
|`Domain Name`|A readable address like `www.example.com` that people can easily remember.|
|`IP Address`|A numerical label (e.g., `93.184.216.34`|

DNS bridges the gap between these two, so we can just type `www.google.com` without needing to remember the underlying IP address.

#### DNS Hierarchy

DNS is organized like a tree, starting from the root and branching out into different layers.

| **Layer**                  | **Description**                                                                   |
| -------------------------- | --------------------------------------------------------------------------------- |
| `Root Servers`             | The top of the DNS hierarchy.                                                     |
| `Top-Level Domains (TLDs)` | Such as `.com`, `.org`, `.net`, or country codes like `.uk`, `.de`.               |
| `Second-Level Domains`     | For example, `example` in `example.com`.                                          |
| `Subdomains or Hostname`   | For instance, `www` in `www.example.com`, or `accounts` in `accounts.google.com`. |

![URL breakdown: Scheme, Subdomains, 2nd-Level Domain, Top-Level Domain, Page name, Root.](https://academy.hackthebox.com/storage/modules/289/DNS/DNS-2.png)

#### DNS Resolution Process (Domain Translation)

When we enter a domain name in our browser, the computer needs to find the corresponding IP address. This process is known as `DNS resolution` or `domain translation`. The steps below show how this process works.

| **Step** | **Description**                                                                                                                                                  |
| -------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `Step 1` | We type `www.example.com` into our browser.                                                                                                                      |
| `Step 2` | Our computer checks its local DNS cache (a small storage area) to see if it already knows the IP address.                                                        |
| `Step 3` | If not found locally, it queries a `recursive DNS server`. This is often provided by our Internet Service Provider or a third-party DNS service like Google DNS. |
| `Step 4` | The recursive DNS server contacts a `root server`, which points it to the appropriate `TLD name server` (such as the `.com` domains, for instance).              |
| `Step 5` | The TLD name server directs the query to the `authoritative name server` for `example.com`.                                                                      |
| `Step 6` | The authoritative name server responds with the IP address for `www.example.com`.                                                                                |
| `Step 7` | The recursive server returns this IP address to your computer, which can then connect to the website’s server directly.                                          |

This all happens in just fractions of a second. Below we can see a simple example of the Domain Translation process. Suppose you want to visit the website at `www.example.com`. Without the Domain Name System (DNS), we would need to know and type the IP address, such as `93.184.216.34`, every time you want to access that site. With DNS in place, we can simply type `www.example.com` into our browser. Behind the scenes, DNS automatically finds and translates this domain name into the correct IP address for us, ensuring a seamless connection to the website. The diagram below illustrates the diagram of the `DNS Query Process`.

![DNS query process: Personal computer requests www.example.com'sl IP from Recursive DNS Server, which queries Root Server, then TLD Server, and finally Authoritative DNS, receiving IP 93.184.216.34.](https://academy.hackthebox.com/storage/modules/289/DNS/DNS_Query_Process-2.png)


What type of domain is `.com` considered as? (Format: Three words, example: One-Two Three)

+ Top-Level Domain

In the domain `www.example.com`, what is `example` called?

+ Second-Level Domain

What is checked first in the DNS resolution process when you enter a domain name into a browser? (Format: Two words)

+ DNS cache

What type of DNS server is typically provided by an Internet Service Provider?

+ recursive DNS server

Which server directs the recursive DNS server to the appropriate TLD name server?

+ Root Server

What numerical label uniquely identifies a device on a network?

+ IP Address

In the URL "accounts.google.com", what is `accounts` considered as?

+ Subdomain

