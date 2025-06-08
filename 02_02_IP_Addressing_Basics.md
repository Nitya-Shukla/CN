# Day 2: MAC cont., Network Layer

## 2. Topic: IP Addressing (Classes & Basics)

### **IPv4 Address Structure**

*   **Fundamental Concept:** An IPv4 address is a unique numerical label assigned to each device participating in a computer network that uses the Internet Protocol for communication.

*   **32-bit Address:**
    *   IPv4 addresses are 32 bits long.
    *   This allows for a theoretical maximum of 2<sup>32</sup> (approximately 4.29 billion) unique addresses.
    *   These bits are typically grouped into four 8-bit segments called **octets** (or bytes).

*   **Dotted Decimal Notation:**
    *   For human readability, IPv4 addresses are most commonly written in dotted decimal notation.
    *   Each octet (8 bits) is converted to its decimal equivalent (a number between 0 and 255).
    *   These four decimal numbers are separated by dots (periods).
    *   **Example:** The 32-bit binary address `11000000 10101000 00000001 00000001` is written as `192.168.1.1` in dotted decimal notation.
        *   `11000000`<sub>2</sub> = 192<sub>10</sub>
        *   `10101000`<sub>2</sub> = 168<sub>10</sub>
        *   `00000001`<sub>2</sub> = 1<sub>10</sub>
        *   `00000001`<sub>2</sub> = 1<sub>10</sub>

*   **Hierarchical Structure (Network and Host Portions):**
    *   An IPv4 address is divided into two main parts:
        1.  **Network Portion (Network ID or Prefix):** The initial bits of the address identify the specific network to which the device belongs. All devices on the same logical network share the same network portion.
        2.  **Host Portion (Host ID):** The remaining bits of the address identify a specific device (host) within that network. Each device on the same network must have a unique host portion.
    *   **Analogy:** Think of a physical mailing address:
        *   The **Network Portion** is like the Street Name and City/Zip Code (identifies the neighborhood/area).
        *   The **Host Portion** is like the House Number (identifies a specific house on that street).
    *   The boundary between the network and host portions was traditionally defined by address classes (Class A, B, C), but is now more flexibly defined by a **subnet mask** or CIDR prefix length.

*   **Conceptual Example (before discussing classes/subnet masks):**
    *   Consider the IP address `192.168.1.100`.
    *   If we *conceptually* say (for this example, without formally defining a class or mask) that the first three octets (`192.168.1`) form the Network ID and the last octet (`100`) forms the Host ID:
        *   **Network ID:** `192.168.1.0` (Host portion bits set to 0 often represents the network itself).
        *   **Host ID:** `100` (within the `192.168.1.0` network).
        *   Another device, `192.168.1.101`, would be on the **same network** (`192.168.1.0`) but would be a **different host** (`101`).
        *   A device `192.168.2.50` would be on a **different network** (`192.168.2.0`).

*   **Key Implications of the Structure:**
    *   **Routing:** Routers use the network portion of an IP address to forward packets towards the destination network.
    *   **Uniqueness:** Within a given network, host IDs must be unique. Globally, for public IP addresses, the combination of Network ID and Host ID must be unique.

*   **Exam Angle (from pyqs.md):**
    *   Be ready to explain that IPv4 addresses are 32-bit long.
    *   Explain the dotted decimal notation with an example.
    *   Explain the hierarchical structure, clearly distinguishing between the network portion and the host portion, and their roles in identifying a network and a specific host within that network, respectively. 

### **IPv4 Address Classes (Classful Addressing)**

Historically, IPv4 addresses were divided into five classes (A, B, C, D, E) to define the boundary between the network and host portions. This is known as **classful addressing**. While largely superseded by Classless Inter-Domain Routing (CIDR), understanding classes is crucial for foundational knowledge and some older systems/exam questions.

**Key Characteristics of Address Classes:**

| Feature             | Class A                       | Class B                       | Class C                       | Class D (Multicast)          | Class E (Experimental)       |
|---------------------|-------------------------------|-------------------------------|-------------------------------|------------------------------|------------------------------|
| **Identifying Bits** | Starts with **0**             | Starts with **10**            | Starts with **110**           | Starts with **1110**         | Starts with **1111**         |
| (First Octet Binary)| `0xxxxxxx`                    | `10xxxxxx`                    | `110xxxxx`                    | `1110xxxx`                   | `1111xxxx`                   |
| **Range of First Octet (Decimal)** | 1 - 126*                      | 128 - 191                     | 192 - 223                     | 224 - 239                    | 240 - 255                    |
| **Network ID Bits**   | 8 (first octet)               | 16 (first two octets)         | 24 (first three octets)       | N/A (Used for multicast group ID) | N/A (Reserved)               |
| **Host ID Bits**      | 24 (last three octets)        | 16 (last two octets)          | 8 (last octet)                | N/A                          | N/A                          |
| **Default Subnet Mask** | `255.0.0.0`                   | `255.255.0.0`                 | `255.255.255.0`               | N/A                          | N/A                          |
| **CIDR Notation of Default Mask** | `/8`                          | `/16`                         | `/24`                         | N/A                          | N/A                          |
| **Number of Networks** | 2<sup>7</sup> - 2 = 126             | 2<sup>14</sup> = 16,384            | 2<sup>21</sup> = 2,097,152        | N/A                          | N/A                          |
| (Networks available) | (0 and 127 are special)       |                               |                               |                              |                              |
| **Number of Usable Hosts per Network** | 2<sup>24</sup> - 2                  | 2<sup>16</sup> - 2                  | 2<sup>8</sup> - 2                   | N/A                          | N/A                          |
|                     | (16,777,214 hosts)            | (65,534 hosts)                | (254 hosts)                   |                              |                              |

*Note on Class A Range: First octet `0` (binary `00000000`) is reserved (represents "this network"). First octet `127` (binary `01111111`) is reserved for loopback addresses (e.g., `127.0.0.1`). So, assignable Class A network numbers are 1 to 126.*
*Note on Number of Networks/Hosts: We subtract 2 from host calculations because the all-0s host ID represents the network address itself, and the all-1s host ID represents the broadcast address for that network. For Class A networks, 0.x.x.x and 127.x.x.x are reserved, reducing available networks.*

**Detailed Breakdown:**

*   **Class A:**
    *   **Identifying Bits:** The very first bit is `0`.
    *   **First Octet Range:** `00000001` to `01111110` (1 to 126 decimal).
        *   `0.x.x.x` is reserved.
        *   `127.x.x.x` is reserved for loopback and diagnostic functions (e.g., `127.0.0.1` is localhost).
    *   **Network/Host ID Bits:** 8 bits for Network ID (N), 24 bits for Host ID (H). Format: `N.H.H.H`.
    *   **Default Subnet Mask:** `255.0.0.0` or `/8`.
    *   **Number of Networks:** 2<sup>(8-1)</sup> = 2<sup>7</sup> = 128. Since networks 0 and 127 are reserved, there are 126 usable Class A networks.
    *   **Number of Hosts per Network:** 2<sup>24</sup> - 2 = 16,777,216 - 2 = 16,777,214 usable hosts.
    *   **Use Case:** Designed for a small number of very large organizations.

*   **Class B:**
    *   **Identifying Bits:** The first two bits are `10`.
    *   **First Octet Range:** `10000000` to `10111111` (128 to 191 decimal).
    *   **Network/Host ID Bits:** 16 bits for Network ID, 16 bits for Host ID. Format: `N.N.H.H`.
    *   **Default Subnet Mask:** `255.255.0.0` or `/16`.
    *   **Number of Networks:** 2<sup>(16-2)</sup> = 2<sup>14</sup> = 16,384 networks.
    *   **Number of Hosts per Network:** 2<sup>16</sup> - 2 = 65,536 - 2 = 65,534 usable hosts.
    *   **Use Case:** Designed for medium-sized organizations.

*   **Class C:**
    *   **Identifying Bits:** The first three bits are `110`.
    *   **First Octet Range:** `11000000` to `11011111` (192 to 223 decimal).
    *   **Network/Host ID Bits:** 24 bits for Network ID, 8 bits for Host ID. Format: `N.N.N.H`.
    *   **Default Subnet Mask:** `255.255.255.0` or `/24`.
    *   **Number of Networks:** 2<sup>(24-3)</sup> = 2<sup>21</sup> = 2,097,152 networks.
    *   **Number of Hosts per Network:** 2<sup>8</sup> - 2 = 256 - 2 = 254 usable hosts.
    *   **Use Case:** Designed for a large number of small organizations/networks.

*   **Class D:**
    *   **Identifying Bits:** The first four bits are `1110`.
    *   **First Octet Range:** `11100000` to `11101111` (224 to 239 decimal).
    *   **Purpose:** Reserved for **multicast addressing**. Multicast allows a single packet to be sent to multiple specific recipients (members of a multicast group) simultaneously.
    *   **Structure:** There is no Network/Host ID division in the same way as unicast classes. The entire address (after the leading `1110`) identifies a multicast group.
    *   **No Default Subnet Mask** in the traditional sense.

*   **Class E:**
    *   **Identifying Bits:** The first four bits are `1111` (often described as first four bits `1111`, or `11110` for a 5-bit prefix, but the key is that the first octet range starts from 240).
    *   **First Octet Range:** `11110000` to `11111111` (240 to 255 decimal).
    *   **Purpose:** Reserved for **experimental or future use**. These addresses are not typically used on the public internet.
    *   **No Default Subnet Mask**.

**Practice: Identifying Class, Network ID, Host ID, and Default Mask**

Given an IP Address, say `150.100.20.30`:

1.  **Identify the Class:** Look at the first octet (150).
    *   Class A: 1-126
    *   Class B: 128-191  <- 150 falls in this range.
    *   Class C: 192-223
    *   So, `150.100.20.30` is a **Class B** address.

2.  **Determine Default Subnet Mask:** For Class B, it is `255.255.0.0` (or `/16`).

3.  **Determine Network ID Bits and Host ID Bits:** For Class B, the first 16 bits are Network ID, and the last 16 bits are Host ID.
    *   IP Address: `150.100 . 20.30`
    *   Network Bits: `150.100`
    *   Host Bits: `20.30`

4.  **Determine Network Address:** Set all Host ID bits to 0.
    *   Network Address: `150.100.0.0`

5.  **Determine Host ID within the network:** The host portion itself.
    *   Host ID: `0.0.20.30` (conceptually, or simply `20.30` as the host identifier within network `150.100.0.0`).

**Example 2: `198.12.34.56`**
1.  **Class:** First octet is 198. This is between 192-223 -> **Class C**.
2.  **Default Subnet Mask:** `255.255.255.0` (or `/24`).
3.  **Network/Host Bits:** First 24 bits are Network, last 8 bits are Host.
    *   IP Address: `198.12.34 . 56`
    *   Network Bits: `198.12.34`
    *   Host Bits: `56`
4.  **Network Address:** `198.12.34.0`
5.  **Host ID:** `56` (within network `198.12.34.0`).

**Exam Angle (from pyqs.md):**
*   **"Explain various classes of IP addresses (network-ID bits, host-ID bits, etc.)"**: This requires a full description of Classes A, B, and C, including their identifying bits, first octet ranges, the number of bits allocated for network ID vs. host ID, their default subnet masks, and the resulting number of networks and hosts per network. Briefly mention Class D for multicast and Class E for experimental.
*   **"Identify class"**: Be able to quickly determine the class of an IP address by looking at its first octet value. From the class, you should be able to state its default network and host portions. You should also be able to state the default subnet mask.

**Limitations of Classful Addressing:**
*   **Inflexibility:** Organizations were assigned a whole Class A, B, or C block. A Class C with 254 hosts might be too small, while a Class B with 65,534 hosts might be too large, leading to wasted address space.
*   **Address Exhaustion:** The rapid growth of the internet meant that IPv4 addresses (especially Class B) were being depleted quickly.
These limitations led to the development of **subnetting** and **Classless Inter-Domain Routing (CIDR)**.

### **Special IPv4 Addresses**

Beyond the general class-based unicast addresses, several ranges and specific addresses have special meanings and purposes in IPv4 networking.

*   **1. Loopback Address:**
    *   **Range:** `127.0.0.0` to `127.255.255.255` (entire `127.0.0.0/8` block is reserved for loopback).
    *   **Most Common Example:** `127.0.0.1` (often referred to as `localhost`).
    *   **Purpose:**
        *   Used for **inter-process communication** on the local host. Applications can use this address to communicate with other applications running on the same machine without needing to know the machine's actual IP address.
        *   Used for **testing the TCP/IP stack** on the local machine. Pinging `127.0.0.1` tests whether the TCP/IP software is installed and functioning correctly, without any packets leaving the local machine.
        *   Packets sent to a loopback address are "looped back" within the host's networking stack and are not forwarded onto any physical network.

*   **2. Private IP Addresses:**
    *   **Purpose:** To allow organizations to create internal networks using IP addresses without needing to request globally unique public IP addresses from Internet Assigned Numbers Authority (IANA) or regional registries. This conserves the limited supply of public IPv4 addresses.
    *   **Non-Routable on Public Internet:** Routers on the public internet are configured to **not** forward packets originating from or destined to these private IP address ranges.
    *   **Network Address Translation (NAT):** To communicate with the public internet, devices using private IP addresses typically go through a router or firewall performing NAT. NAT translates the private source IP address to a public IP address (often the router's own public IP) before sending the packet out, and does the reverse for incoming packets.
    *   **Ranges (defined in RFC 1918):**
        *   **Class A type:** `10.0.0.0` to `10.255.255.255` (a single Class A network: `10.0.0.0/8`)
            *   Allows for one very large private network with millions of hosts.
        *   **Class B type:** `172.16.0.0` to `172.31.255.255` (16 contiguous Class B networks: `172.16.0.0/12`)
            *   Allows for many medium-to-large private networks.
        *   **Class C type:** `192.168.0.0` to `192.168.255.255` (256 contiguous Class C networks: `192.168.0.0/16`)
            *   Commonly used for small office/home office (SOHO) networks.
    *   **Usage:** These ranges can be used by anyone for their internal networks, and different organizations can use the same private IP address ranges without conflict, as they are isolated from each other and the public internet.

*   **3. Network Address:**
    *   **Definition:** An IP address where all bits in the **host portion** are set to `0`.
    *   **Purpose:** It identifies the network itself, rather than a specific host on that network.
    *   **Example:** For a Class C network `192.168.1.0` with a default mask `255.255.255.0`:
        *   The IP address `192.168.1.0` is the network address.
        *   Individual hosts would be `192.168.1.1`, `192.168.1.2`, ..., `192.168.1.254`.
    *   **Non-Assignable to Hosts:** The network address cannot be assigned to an individual device as its IP address.
    *   **Usage:** Used in routing tables to specify routes to networks.

*   **4. Broadcast Address:**
    There are two main types of broadcast addresses:

    *   **a) Directed Broadcast Address (or Network Broadcast Address):**
        *   **Definition:** An IP address for a specific network where all bits in the **host portion** are set to `1`.
        *   **Purpose:** To send a packet to all hosts on a *specific remote network*. However, due to security concerns (e.g., Smurf attacks), modern routers often disable the forwarding of directed broadcasts.
        *   **Example:** For network `192.168.1.0` (mask `255.255.255.0`), the directed broadcast address is `192.168.1.255`. A packet sent to this address from outside the `192.168.1.0` network (if allowed by routers) would be delivered to all hosts on `192.168.1.0`.
        *   **Local Delivery:** If a host within `192.168.1.0` sends a packet to `192.168.1.255`, it is broadcast to all other hosts on that same local network.
        *   **Non-Assignable to Hosts:** The directed broadcast address cannot be assigned to an individual device.

    *   **b) Limited Broadcast Address:**
        *   **Address:** `255.255.255.255` (all 32 bits are set to `1`).
        *   **Purpose:** To send a packet to all hosts on the **local physical network segment** of the sending device.
        *   **Non-Routable:** Routers do **not** forward packets addressed to `255.255.255.255`. These packets are confined to the local network segment.
        *   **Usage Example:** DHCP clients use this address to find a DHCP server when they first join a network and don't yet have an IP address.

*   **5. "This Host" or "This Network" Address (All Zeros):**
    *   **Address:** `0.0.0.0`
    *   **Purpose:**
        *   When used as a **source address**: It means "this host" and is used by a device that does not yet know its own IP address (e.g., during the DHCP discovery process when a client sends a DHCPDISCOVER message).
        *   When used as a **destination address** (less common in application specification): Can sometimes mean "this host" for certain local configurations.
        *   In **routing tables**: An entry for network `0.0.0.0/0` (or just `0.0.0.0` with mask `0.0.0.0`) represents the **default route**. This is the route used if no other more specific route matches the destination IP address of a packet.

**Exam Angle (from pyqs.md):**
*   Clearly define and state the purpose of:
    *   **Loopback Address (e.g., 127.0.0.1):** Testing TCP/IP, local IPC.
    *   **Private IP Ranges (10.x.x.x, 172.16.x.x-172.31.x.x, 192.168.x.x):** Internal use, NAT for internet access, address conservation.
    *   **Network Address:** All host bits are zero, identifies the network.
    *   **Broadcast Addresses:**
        *   Directed Broadcast: All host bits are one for a specific network.
        *   Limited Broadcast (`255.255.255.255`): All hosts on the local segment, not routed.
*   Understand why these addresses are "special" and generally not assignable to individual hosts for normal unicast communication (except for the host itself using loopback or `0.0.0.0` as a source in specific scenarios).

### **Subnet Mask Concept (Introduction)**

*   **Purpose of a Subnet Mask:**
    *   A subnet mask is a 32-bit number used in conjunction with an IPv4 address to define the boundary between the **network portion** and the **host portion** of the address.
    *   While classful addressing provided a default boundary (e.g., /8 for Class A, /16 for Class B, /24 for Class C), subnet masks allow for more flexible division.
    *   The primary purpose is to **subdivide a larger network (defined by its class or a given IP block) into smaller, manageable subnetworks (subnets).**
    *   This subdivision allows an organization to:
        *   Organize its internal network structure (e.g., by department, floor, or function).
        *   Improve network security by isolating segments.
        *   Reduce the size of broadcast domains (improving performance by limiting broadcast traffic).
        *   Efficiently utilize the assigned IP address space.

*   **How it Works (Structure and Logic):**
    *   **Structure:** A subnet mask, like an IPv4 address, is 32 bits long and often written in dotted decimal notation.
        *   It consists of a sequence of continuous `1`s from the left, followed by a sequence of continuous `0`s to the right.
        *   The `1`s in the subnet mask correspond to the **network portion** (which now includes both the original network ID and the newly created subnet ID bits).
        *   The `0`s in the subnet mask correspond to the **host portion** within that specific subnet.
    *   **Example Masks:**
        *   `255.0.0.0` (Class A default) -> `11111111 00000000 00000000 00000000` (8 network bits, 24 host bits)
        *   `255.255.0.0` (Class B default) -> `11111111 11111111 00000000 00000000` (16 network bits, 16 host bits)
        *   `255.255.255.0` (Class C default) -> `11111111 11111111 11111111 00000000` (24 network bits, 8 host bits)
        *   `255.255.255.192` (A custom mask for subnetting) -> `11111111 11111111 11111111 11000000` (26 network/subnet bits, 6 host bits)

*   **Determining the Network Address (Logical AND Operation):**
    *   To find the network address (or subnet address) to which an IP address belongs, a device performs a **bitwise logical AND** operation between the IP address and its subnet mask.
    *   **How AND works:**
        *   `1 AND 1 = 1`
        *   `1 AND 0 = 0`
        *   `0 AND 1 = 0`
        *   `0 AND 0 = 0`
    *   The result of this AND operation is the **network address** of the subnet.

    *   **Example:**
        *   IP Address: `192.168.1.75`
        *   Subnet Mask: `255.255.255.0` (Class C default or a /24 subnet)

        *   In Binary:
            ```
            IP Address:  11000000.10101000.00000001.01001011  (192.168.1.75)
            Subnet Mask: 11111111.11111111.11111111.00000000  (255.255.255.0)
            ---------------------------------------------------
            Network Addr:11000000.10101000.00000001.00000000  (192.168.1.0)
            ```
        *   So, the IP address `192.168.1.75` with subnet mask `255.255.255.0` belongs to the network `192.168.1.0`.
        *   Any IP address from `192.168.1.1` to `192.168.1.254` using this mask would belong to the same network `192.168.1.0`.

*   **Key Takeaway:**
    *   The subnet mask is essential for a device to determine:
        1.  Which part of an IP address is the network/subnet identifier.
        2.  Which part is the host identifier.
        3.  Whether a destination IP address is on the same local subnet or on a remote network (requiring a router to reach).

*   **Exam Angle (from pyqs.md):**
    *   **Explain its purpose:** To divide a larger network into smaller subnets, allowing for better organization, security, and address management. It defines the boundary between the network/subnet portion and the host portion of an IP address.
    *   **How it works (ANDing):** Explain that the network address is found by performing a bitwise AND operation between the IP address and the subnet mask. Provide a simple example in binary and dotted decimal form.

This introduction lays the groundwork for understanding detailed subnetting calculations, which is the next major topic. 