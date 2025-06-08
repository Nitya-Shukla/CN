# Day 2: MAC cont., Network Layer

## 3. Topic: Subnetting & CIDR

### **Subnetting**

Subnetting is the process of dividing a single, larger IP network into multiple smaller, logical networks called subnets. This is achieved by "borrowing" bits from the host portion of an IP address and using them to create a subnet identifier.

*   **Purpose of Subnetting:**
    *   **Improved Organization & Manageability:** Networks can be structured logically (e.g., by department, floor, function), making them easier to manage and troubleshoot.
    *   **Enhanced Security:** Subnets can isolate different parts of a network. Traffic between subnets can be controlled by routers acting as firewalls, restricting access and improving security.
    *   **Reduced Broadcast Domain Size:** Broadcast traffic (e.g., ARP requests) is confined within its own subnet. Smaller broadcast domains mean less broadcast traffic for each host to process, improving overall network performance.
    *   **Efficient Use of IP Address Space:** Instead of assigning a large, monolithic block of IPs where many might go unused, subnetting allows for right-sizing networks, allocating only the necessary number of IPs to each subnet. This was particularly important with classful addressing.
    *   **Overcome Shortage of Network IDs (Historically):** In classful addressing, an organization might get a Class B address but need more than one logical network. Subnetting allowed them to create these internal networks without needing additional public network IDs.

*   **Process: "Borrowing Host Bits"**
    *   The core idea is to take a standard network (like a Class A, B, or C network, or a given IP block) and extend its network portion by using some of the bits originally allocated for the host portion.
    *   These "borrowed" bits create a **Subnet ID**.
    *   The IP address structure then becomes: `Network ID | Subnet ID | Host ID`.
    *   The **Subnet Mask** is crucial here. It's modified to reflect these borrowed bits. The 1s in the subnet mask now cover both the original Network ID and the new Subnet ID bits.

    *   **Example:**
        *   Suppose you have a Class C network: `192.168.1.0`
        *   Default Mask: `255.255.255.0` (Binary: `11111111.11111111.11111111.00000000`)
        *   This gives 8 bits for hosts (2<sup>8</sup>-2 = 254 hosts).
        *   If you decide to borrow **3 bits** from the host portion for subnetting:
            *   Original Host Bits: `hhhhhhhh` (8 bits)
            *   Borrowed for Subnet: `sss` (3 bits)
            *   Remaining for Host: `hhhhh` (5 bits)
            *   New structure of the last octet: `sss hhhhh`
            *   The subnet mask will change to reflect these 3 borrowed bits. The first 3 bits of the host portion in the mask will become 1s.
            *   New Subnet Mask (last octet): `11100000` (Decimal: 224)
            *   Full New Subnet Mask: `255.255.255.224`

*   **Calculations (Given an IP block/network and subnetting requirement):**

    Let:
    *   `s` = number of bits borrowed for the Subnet ID.
    *   `h` = number of bits remaining for the Host ID.

    **1. New Subnet Mask:**
        *   To calculate the new subnet mask, turn the `s` borrowed bits (starting from the left of the original host portion) to `1`s in the default mask.
        *   **Dotted Decimal:** Convert each octet of the new binary mask to decimal.
        *   **CIDR Notation:** Add `s` to the original prefix length of the network. (e.g., if original was /24 and you borrow 3 bits, new CIDR is /27).

        *   **Example (Continuing from `192.168.1.0`, borrowing 3 bits):**
            *   Default mask (binary, last octet): `00000000`
            *   Borrow 3 bits: The first 3 bits become `1`s -> `11100000`
            *   New mask (binary, last octet): `11100000`
            *   New mask (decimal, last octet): 128 + 64 + 32 = 224
            *   New Full Subnet Mask (dotted decimal): `255.255.255.224`
            *   New Subnet Mask (CIDR): Original was /24. Borrowed 3 bits. New CIDR = / (24+3) = `/27`.

    **2. Number of Subnets Created:**
        *   Formula: **2<sup>s</sup>**
        *   Where `s` is the number of bits borrowed.
        *   **Example (borrowing 3 bits):**
            *   Number of Subnets = 2<sup>3</sup> = 8 subnets.
            *   These subnets will have Subnet IDs from `000` to `111` in binary.

    **3. Number of Usable Hosts per Subnet:**
        *   Formula: **2<sup>h</sup> - 2**
        *   Where `h` is the number of bits *remaining* in the host portion.
        *   We subtract 2 because:
            *   An address with all host bits as `0` is the Network Address of the subnet itself.
            *   An address with all host bits as `1` is the Broadcast Address for that subnet.
        *   **Example (borrowing 3 bits from an 8-bit host portion):**
            *   Original host bits = 8. Borrowed (s) = 3.
            *   Remaining host bits (h) = 8 - 3 = 5 bits.
            *   Number of Usable Hosts per Subnet = 2<sup>5</sup> - 2 = 32 - 2 = 30 hosts.

    **4. For each Subnet, determine:**
        *   This involves listing out all the subnets created. The Subnet ID increments.

        *   **a) Network Address (Subnet ID):**
            *   This is the first address in each subnet's range. The host portion bits are all `0`.
            *   The value of the borrowed bits (`s`) determines the specific subnet.

        *   **b) First Usable Host Address:**
            *   Network Address + 1 (i.e., the host portion is all `0`s followed by a `1`).

        *   **c) Last Usable Host Address:**
            *   Broadcast Address - 1 (i.e., the host portion is all `1`s preceded by a `0`).

        *   **d) Broadcast Address for that Subnet:**
            *   This is the last address in each subnet's range. The host portion bits are all `1`.

        *   **Example: Network `192.168.1.0`, Mask `255.255.255.224` (/27)**
            *   Borrowed bits (s) = 3. Subnet IDs from `000` to `111`.
            *   Remaining host bits (h) = 5. Increment for hosts = 2<sup>5</sup> = 32. Each subnet has 32 addresses (0-31, 32-63, etc.).

            *   **Subnet 1 (Subnet ID in borrowed bits: `000`):**
                *   Last Octet (binary): `000 00000` (Subnet ID `000`, Host ID `00000`)
                *   **Network Address:** `192.168.1.0`
                *   **First Usable Host:** `192.168.1.1` (Host ID `00001`)
                *   **Last Usable Host:** `192.168.1.30` (Host ID `11110`)
                *   **Broadcast Address:** `192.168.1.31` (Host ID `11111`)

            *   **Subnet 2 (Subnet ID in borrowed bits: `001`):**
                *   Last Octet (binary): `001 00000`
                *   **Network Address:** `192.168.1.32`
                *   **First Usable Host:** `192.168.1.33`
                *   **Last Usable Host:** `192.168.1.62`
                *   **Broadcast Address:** `192.168.1.63`

            *   **Subnet 3 (Subnet ID in borrowed bits: `010`):**
                *   Last Octet (binary): `010 00000`
                *   **Network Address:** `192.168.1.64`
                *   ...and so on...

            *   **Subnet 8 (Subnet ID in borrowed bits: `111`):**
                *   Last Octet (binary): `111 00000`
                *   **Network Address:** `192.168.1.224`
                *   **First Usable Host:** `192.168.1.225`
                *   **Last Usable Host:** `192.168.1.254`
                *   **Broadcast Address:** `192.168.1.255`

*   **INTENSIVE PRACTICE & Key Formulas/Steps:**
    *   **Practice is Crucial:** Subnetting is a skill learned by doing. Work through numerous examples with different starting networks and requirements (e.g., "subnet for X networks" or "subnet for Y hosts per network").
    *   **Key Formulas to Memorize:**
        *   Number of Subnets = 2<sup>s</sup>
        *   Number of Usable Hosts per Subnet = 2<sup>h</sup> - 2
    *   **Steps for Solving Subnetting Problems:**
        1.  Identify the original network address and default mask (or given prefix).
        2.  Determine the number of bits to borrow (`s`) based on the number of required subnets OR the number of required hosts per subnet.
            *   If given #subnets: find smallest `s` such that 2<sup>s</sup> >= #required_subnets.
            *   If given #hosts/subnet: find smallest `h` such that 2<sup>h</sup>-2 >= #required_hosts/subnet. Then calculate `s` from the total available host bits.
        3.  Calculate the new subnet mask (dotted decimal and CIDR).
        4.  Calculate the number of subnets and usable hosts per subnet.
        5.  List the subnet ranges: Network Address, Usable Host Range, Broadcast Address for each subnet.
            *   Find the "increment" or "block size" for network addresses: 256 - (last non-255 octet value of the mask). For `/27` (255.255.255.224), increment is 256-224=32. So networks are .0, .32, .64, etc.

*   **Exam Angle (from pyqs.md):**
    *   **"What is subnet mask?"**: Explain its definition, structure (1s for network/subnet, 0s for host), and its primary role in dividing networks and identifying the network/subnet portion of an IP via the AND operation. Refer back to the "Subnet Mask Concept (Introduction)".
    *   **Numerical problems are VERY common:** Be prepared to perform all the calculations above:
        *   Given an IP and #subnets required, find the new mask, hosts/subnet, and subnet ranges.
        *   Given an IP and #hosts/subnet required, find the new mask, number of subnets, and subnet ranges.
        *   Given an IP and a custom subnet mask, determine the Network Address, Broadcast Address, and usable host range for that IP.
    *   Understand how to convert between binary, dotted decimal, and CIDR for masks and addresses.

### **CIDR (Classless Inter-Domain Routing)**

CIDR is a method for allocating IP addresses and routing Internet Protocol packets. It was introduced to replace the older classful addressing architecture (Class A, B, C) with the goal of slowing the exhaustion of IPv4 addresses and improving the scalability of routing on the internet.

*   **Concept: Eliminating Rigid Classes**
    *   CIDR discards the fixed /8, /16, and /24 network prefixes of Class A, B, and C addresses.
    *   Instead, it allows network prefixes to be of **any length**, from /0 to /32.
    *   This means an IP address block can be defined with a variable number of bits for the network portion and the host portion, precisely tailored to the needs of an organization, rather than forcing them into oversized or undersized classful blocks.
    *   An IP address in CIDR is represented as `a.b.c.d/x`, where `x` is the number of bits in the network prefix.

*   **/notation (Slash Notation or Prefix Length):**
    *   The `/x` in CIDR notation (e.g., `192.168.1.0/24`) specifies the **length of the network prefix** in bits.
    *   This directly corresponds to the number of leading `1`s in the subnet mask.
    *   **Examples:**
        *   `/8` means the first 8 bits are the network prefix (Subnet Mask: `255.0.0.0`).
        *   `/16` means the first 16 bits are the network prefix (Subnet Mask: `255.255.0.0`).
        *   `/24` means the first 24 bits are the network prefix (Subnet Mask: `255.255.255.0`).
        *   `/27` means the first 27 bits are the network prefix (Subnet Mask: `255.255.255.224`).
        *   `/30` means the first 30 bits are the network prefix (Subnet Mask: `255.255.255.252`), often used for point-to-point links with 2 usable host addresses.

*   **Advantages of CIDR:**
    1.  **Address Space Efficiency (Reduced Waste):**
        *   ISPs and organizations can be allocated IP address blocks that closely match their actual needs, rather than being forced into large, wasteful classful blocks.
        *   For example, an organization needing 500 IP addresses would have required a Class B address (65,534 hosts) in the classful system, wasting most of them. With CIDR, they could be assigned a `/23` block (2<sup>(32-23)</sup> = 2<sup>9</sup> = 512 addresses), which is a much better fit.
    2.  **Route Summarization (Supernetting):**
        *   CIDR allows multiple contiguous network prefixes to be aggregated or summarized into a single, shorter prefix (a **supernet**).
        *   **Example:** An ISP might have 16 contiguous /24 networks (e.g., `200.10.0.0/24` through `200.10.15.0/24`). Instead of advertising 16 individual routes to the rest of the internet, the ISP can advertise a single summarized route: `200.10.0.0/20`.
            *   A /20 prefix means 20 bits for the network. The remaining 12 bits are for hosts.
            *   The first 20 bits of `200.10.0.0` through `200.10.15.0` are the same: `11001000.00001010.0000` (200.10.0.xxxx)
        *   **Benefit:** This significantly reduces the size of global routing tables in internet routers, making routing more efficient and scalable.

*   **Relation to Subnetting:**
    *   CIDR notation is essentially a modern way to represent subnet masks.
    *   When you subnet a network, you are effectively creating smaller network blocks, each with a longer prefix length (a larger `/x` number).
    *   **Example:** If you take a `/24` network (e.g., `192.168.1.0/24`) and subnet it by borrowing 3 bits (as in the subnetting example), each subnet becomes a `/27` network (e.g., `192.168.1.0/27`, `192.168.1.32/27`, etc.).
    *   The CIDR prefix directly tells you how many bits are in the combined network and subnet portion of the address.

*   **How to Convert CIDR Prefix to Dotted Decimal Mask:**
    1.  Write out `x` number of `1`s from left to right.
    2.  Fill the remaining (32 - `x`) bits with `0`s.
    3.  Group these 32 bits into four 8-bit octets.
    4.  Convert each octet to its decimal equivalent.

    *   **Example: `/26`**
        1.  26 ones: `11111111 11111111 11111111 11`
        2.  Add (32-26) = 6 zeros: `11111111 11111111 11111111 11000000`
        3.  Grouped: `11111111.11111111.11111111.11000000`
        4.  Decimal: `255.255.255.192`

*   **Exam Angle (from pyqs.md):**
    *   **Concept:** Explain CIDR as a replacement for classful addressing, allowing variable-length network prefixes to improve address allocation efficiency and enable route summarization.
    *   **/notation:** Clearly explain that `/x` denotes the number of bits in the network prefix, which directly defines the subnet mask.
    *   **Advantages:** Focus on efficient address space use (minimizing waste) and route summarization (supernetting) to reduce routing table sizes.
    *   **Relation to Subnetting:** Explain that CIDR notation is the modern way to express a subnetted network's prefix length. Subnetting creates networks with longer CIDR prefixes. 