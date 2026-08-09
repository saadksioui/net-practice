*This project has been created as part of the 42 curriculum by sksioui*

# NetPractice

## Description
NetPractice is a general practical exercise designed to introduce the basics of computer networking. The goal is to solve 10 networking problems by configuring broken network diagrams so that they function correctly.

Through this project, I learned how to:
- Configure IP addresses and subnet masks correctly
- Connect devices through a router and understand the role of a gateway
- Build and read routing tables with static routes
- Understand how TCP/IP addressing works in small-scale networks

Each level presents a non-functioning network diagram. The task is to modify the unshaded fields (IP addresses, masks, routes, gateways) until the network configuration is correct and all communication goals are met.

## Instructions
### Running the Training Interface
1. Download the project file attached to the project page on the 42 intranet.
2. Extract the archive into any folder of your choice.
3. In that folder, run the provided shell script:
```bash
   bash run.sh
```
This will start a local web server and automatically open the training interface in your default web browser.

4. If run.sh does not work, you can start the server manually:
```bash
   python3 -m http.server 49242
```
Then open your browser and navigate to: http://localhost:49242

### Using the Interface
- On the welcome page, enter your 42 intranet login in the field provided. This is required so that Moulinette can identify your personal configuration.
- Use the Training tab to practice the 10 levels at your own pace.
- Use the Evaluation tab to generate a random configuration for exam-like practice.

## Resources
### What is a Network?
At its core, a computer network is a group of two or more devices—often called nodes—that communicate by sending and receiving data packets.

The Internet is the largest and most well-known example of a network, connecting billions of devices worldwide for global data sharing. Because it is operated by telecommunications providers to offer open access with minimal restrictions, the Internet is classified as a public network.

In contrast, a private network restricts communication to a specific location or domain. Unregistered users cannot access the network, making it a much more secure and controlled environment. A standard home Wi-Fi network is a common example: it allows you to print or share files locally while keeping unauthorized outside users from connecting.

### What is TCP and IP Adress?
To transfer data reliably across a network, various protocols work together in distinct layers to handle access, transport, security, and stability.

![TCP/IP](https://media.geeksforgeeks.org/wp-content/uploads/20251215161451592856/tcp-layers.webp)

Two fundamental protocols at the core of this process are TCP and IP:

- **Transmission Control Protocol (TCP):** Operates at the transport layer. It breaks data into smaller chunks (packets), manages their transmission to prevent data loss, and reassembles them in exact order at the receiving end.

- **Internet Protocol (IP):** Operates at the internet layer. It uses an IP Address—a unique series of numbers assigned to every device on a network—to route those packets to the correct destination.

Although frequently paired together, TCP and IP are separate protocols with complementary jobs.

### IPv4 Addresses and Subnet Masks

An **IPv4 address** is a 32-bit numerical identifier formatted as four numbers (octets) separated by dots, ranging from `0.0.0.0` to `255.255.255.255`. Every IPv4 address consists of two distinct components: a **Network ID** (identifying the specific network) and a **Host ID** (identifying a specific device on that network).

A **Subnet Mask** is a corresponding series of numbers used by devices to determine where the Network ID ends and the Host ID begins. In a standard subnet mask, `255` designates network bits, while `0` designates host bits.

### Examples

#### Example 1: Standard Home/Local Network (`255.255.255.0` or `/24`)
* **IP Address:** `192.168.1.50`
* **Subnet Mask:** `255.255.255.0`
* **Network ID:** `192.168.1.0` (First 3 octets)
* **Host ID:** `.50` (Last octet)
* **Usable Host Range:** `192.168.1.1` to `192.168.1.254` (254 available devices)
* **How it works:** Devices with IP addresses sharing the `192.168.1.x` prefix can communicate directly with each other on the local network without passing through a router.

#### Example 2: Enterprise Network (`255.255.0.0` or `/16`)
* **IP Address:** `10.0.15.22`
* **Subnet Mask:** `255.255.0.0`
* **Network ID:** `10.0.0.0` (First 2 octets)
* **Host ID:** `.15.22` (Last 2 octets)
* **Usable Host Range:** `10.0.0.1` to `10.0.255.254` (up to 65,534 available devices)
* **How it works:** By shortening the subnet mask, the network expands to accommodate thousands of additional devices on a single logical network segment.

### Connecting Multiple Devices

Public networks like the **Internet** are established by Internet Service Providers (ISPs) to connect devices across the globe. Your ISP assigns a **Public IP Address** directly to your router, enabling your home or enterprise network to communicate externally.

Internally, the router uses an embedded **switch** to build your local network and assigns a **Private IP Address** to each connected device according to your subnet configuration.

### Reserved IP Address Ranges
Certain IP ranges are reserved for special uses and must not be assigned as standard public addresses:

| IP Address Range | Reserved Purpose |
| :--- | :--- |
| `10.0.0.0 – 10.255.255.255` | Private Networks (Class A) |
| `172.16.0.0 – 172.31.255.255` | Private Networks (Class B) |
| `192.168.0.0 – 192.168.255.255` | Private Networks (Class C) |
| `127.0.0.0 – 127.255.255.255` | Loopback / Internal testing |
| `224.0.0.0 – 239.255.255.255` | Multicast traffic |
| `240.0.0.0 – 255.255.255.255` | Experimental / Research |

---

## Switches vs. Routers

### Switch
A **switch** routes packets between devices within the *same* local network (LAN). It operates strictly within its local domain and cannot communicate directly with outside networks.

![Switch](https://www.iconshock.com/image/Stroke/Computer_gadgets/switch)


### Router
A **router** connects *multiple distinct networks* together, possessing a separate interface for each network it joins.

![Router](https://cdn-icons-png.magnific.com/256/6085/6085180.png?semt=ais_white_label)


> **Crucial Rule:** The IP address ranges on a router's interfaces must **never overlap**. Overlapping ranges imply that the interfaces belong to the same network, which breaks routing functionality.

---

## Routing Tables

A **Routing Table** is a data set stored on a router or host that defines the paths packets take to reach specific network destinations.

![Alternative Text](https://raw.githubusercontent.com/caroldaniel/42sp-cursus-netpractice/refs/heads/main/imgs/route-table.png)

In standard networking exercises (such as *Net Practice*), a routing table entry relies on two core pieces of information:

* **Destination (Left Column):** The target IP address and CIDR prefix (e.g., `190.3.2.252/30`). To match all unlisted destinations, use `0.0.0.0/0` (or `default`).
* **Next Hop (Right Column):** The IP address of the next adjacent router interface that packets must pass through to reach the destination network.