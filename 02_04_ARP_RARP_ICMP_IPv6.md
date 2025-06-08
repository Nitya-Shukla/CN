# Day 2: MAC cont., Network Layer

## 4. Topic: ARP, RARP, ICMP & IPv6 Intro

### **ARP (Address Resolution Protocol)**

*   **Purpose:**
    *   Address Resolution Protocol (ARP) is a communication protocol used for discovering the link layer address, such as a MAC (Media Access Control) address, associated with a given internet layer address, typically an IPv4 address.
    *   **Core Function:** To map a known IP address to its corresponding physical MAC address when devices are on the **same local network (LAN)**.
    *   **Why it's needed:** Data packets on a local network are ultimately delivered using hardware MAC addresses. When a host wants to send an IP packet to another host on the same LAN, it knows the destination IP address (e.g., from DNS or configuration). However, to actually send the Ethernet frame, it needs the destination host's MAC address.

*   **Working Principle:**

    1.  **Check ARP Cache:**
        *   When Host A wants to send data to Host B (on the same LAN) and knows Host B's IP address (IP_B), Host A first checks its own **ARP cache** (a table in memory).
        *   The ARP cache stores recent IP-to-MAC address mappings it has learned.
        *   If a valid entry for IP_B exists in the cache, Host A retrieves MAC_B and uses it to create and send the Ethernet frame directly to Host B.

    2.  **ARP Request (if not in cache):**
        *   If IP_B is not found in Host A's ARP cache (or the entry has expired), Host A initiates an ARP discovery process.
        *   Host A constructs an **ARP Request** packet. This packet essentially asks, "Who has IP address IP_B? Tell me your MAC address."
        *   **Key fields in ARP Request:**
            *   Sender MAC Address: MAC_A (Host A's MAC)
            *   Sender IP Address: IP_A (Host A's IP)
            *   Target MAC Address: `00:00:00:00:00:00` (unknown, set to all zeros or a broadcast MAC like FF:FF:FF:FF:FF:FF in the Ethernet header)
            *   Target IP Address: IP_B (the IP address Host A is looking for)
        *   The ARP Request is encapsulated in an Ethernet frame with:
            *   Source MAC: MAC_A
            *   Destination MAC: `FF:FF:FF:FF:FF:FF` (Ethernet broadcast address)
        *   This broadcast frame is sent to all devices on the local network segment.

    3.  **ARP Reply:**
        *   All devices on the LAN receive and process the broadcasted ARP Request.
        *   Devices whose IP address does not match IP_B will silently discard the ARP Request.
        *   The device that **owns** IP_B (i.e., Host B) recognizes its IP address in the Target IP Address field.
        *   Host B then constructs an **ARP Reply** packet.
        *   **Key fields in ARP Reply:**
            *   Sender MAC Address: MAC_B (Host B's MAC - this is the information Host A was seeking)
            *   Sender IP Address: IP_B (Host B's IP)
            *   Target MAC Address: MAC_A (Host A's MAC - taken from the original ARP Request)
            *   Target IP Address: IP_A (Host A's IP - taken from the original ARP Request)
        *   The ARP Reply is encapsulated in an Ethernet frame with:
            *   Source MAC: MAC_B
            *   Destination MAC: MAC_A (unicast, directly to Host A)
        *   Host B sends this unicast frame directly back to Host A.

    4.  **Update ARP Cache and Send Data:**
        *   Host A receives the ARP Reply from Host B.
        *   Host A extracts Host B's MAC address (MAC_B) from the ARP Reply.
        *   Host A **updates its ARP cache** with the new mapping: (IP_B -> MAC_B).
        *   Host A can now use MAC_B to encapsulate its original IP packet in an Ethernet frame and send it directly to Host B.
        *   Other hosts on the network that received the initial ARP Request might also cache Host A's IP-to-MAC mapping from the request (passive ARP learning).

*   **ARP Cache:**
    *   A temporary table maintained by each host and router.
    *   Stores IP-to-MAC address mappings for devices on the local network.
    *   Entries typically have a timeout period (e.g., a few minutes) after which they are removed if not refreshed. This is to handle cases where a device's MAC address might change (e.g., NIC replacement) or an IP address is reassigned.
    *   Improves efficiency by avoiding repeated ARP Request/Reply cycles for frequently accessed local devices.

*   **Scenario: When ARP is used**
    *   Host A (IP: 192.168.1.10, MAC: AA:AA:AA:AA:AA:AA) wants to send an IP packet to Host B (IP: 192.168.1.20) on the same Ethernet LAN.
    1.  Host A checks its ARP cache for an entry for 192.168.1.20.
    2.  **If not found:**
        *   Host A broadcasts an ARP Request: "Who has 192.168.1.20? Tell 192.168.1.10 (at AA:AA:AA:AA:AA:AA)."
        *   The Ethernet frame destination is FF:FF:FF:FF:FF:FF.
    3.  All hosts on the 192.168.1.0/24 network receive it.
    4.  Host B (MAC: BB:BB:BB:BB:BB:BB) recognizes its IP address.
    5.  Host B sends a unicast ARP Reply to Host A: "192.168.1.20 is at BB:BB:BB:BB:BB:BB."
        *   The Ethernet frame destination is AA:AA:AA:AA:AA:AA.
    6.  Host A receives the reply, caches (192.168.1.20 -> BB:BB:BB:BB:BB:BB).
    7.  Host A can now send its IP packet to Host B by putting it in an Ethernet frame with destination MAC BB:BB:BB:BB:BB:BB.
    *   **If Host A wants to send to an IP on a different network (e.g., 8.8.8.8):**
        *   Host A will ARP for the MAC address of its **default gateway (router)**, not the final destination IP.
        *   The router, once it receives the packet, will then handle forwarding it to the remote network (potentially ARPing on its other interfaces if needed).

*   **Exam Angle (from pyqs.md):**
    *   **"Explain working of ARP"**: This requires detailing the steps: cache check, ARP Request (broadcast, contents), ARP Reply (unicast, contents), and cache update. Mentioning the purpose (IP to MAC on local LAN) is crucial.
    *   **"Short Note"**: Summarize ARP's purpose, the request/reply mechanism, the role of the ARP cache, and its limitation to local network segments. Mention it operates between Layer 2 and Layer 3.

### **RARP (Reverse Address Resolution Protocol)**

*   **Purpose:**
    *   Reverse Address Resolution Protocol (RARP) is a network protocol used by a client machine to request its IPv4 address from a computer network, when it only knows its own MAC address.
    *   **Core Function:** To map a known MAC address to an IP address.
    *   **Historical Context:** RARP was primarily used by diskless workstations or thin clients when they booted up. These devices knew their own MAC address (burned into the NIC) but did not have permanent storage for an IP address.
    *   **Largely Obsolete:** RARP has been largely superseded by more robust and feature-rich protocols like BOOTP (Bootstrap Protocol) and DHCP (Dynamic Host Configuration Protocol), which can provide not only an IP address but also other configuration information (e.g., subnet mask, gateway, DNS server).

*   **Working Principle (Basic Idea):**

    1.  **RARP Request:**
        *   When a RARP client boots up, it needs to discover its IP address.
        *   It constructs a **RARP Request** packet. This packet essentially asks, "My MAC address is MAC_Client, can someone tell me my IP address?"
        *   **Key fields in RARP Request:**
            *   Sender MAC Address: MAC_Client (Client's own MAC)
            *   Target MAC Address: MAC_Client (Client's own MAC)
            *   The fields for IP addresses are usually left blank or set to `0.0.0.0` as the client doesn't know its IP yet.
        *   The RARP Request is encapsulated in an Ethernet frame with:
            *   Source MAC: MAC_Client
            *   Destination MAC: `FF:FF:FF:FF:FF:FF` (Ethernet broadcast address)
        *   This broadcast frame is sent to all devices on the local network segment.

    2.  **RARP Reply:**
        *   The RARP Request is received by a **RARP server** on the same local network.
        *   The RARP server maintains a pre-configured table that maps MAC addresses to IP addresses.
        *   The server looks up the client's MAC address (MAC_Client) from the RARP Request in its mapping table.
        *   If a mapping is found, the RARP server constructs a **RARP Reply** packet.
        *   **Key fields in RARP Reply:**
            *   Sender MAC Address: MAC_Server (RARP server's MAC)
            *   Sender IP Address: IP_Server (RARP server's IP)
            *   Target MAC Address: MAC_Client (Client's MAC)
            *   Target IP Address: IP_Client (The IP address assigned to the client - this is the information the client was seeking)
        *   The RARP Reply is encapsulated in an Ethernet frame and typically sent as a **unicast** directly back to the client's MAC address (MAC_Client).

    3.  **Client Configuration:**
        *   The RARP client receives the RARP Reply.
        *   It extracts its assigned IP address (IP_Client) from the reply.
        *   The client can now configure its network interface with this IP address and begin IP-based communication.

*   **Limitations of RARP:**
    *   **Provides Only IP Address:** RARP only provides the client with its IP address. It doesn't supply other essential network configuration details like subnet mask, default gateway address, or DNS server addresses.
    *   **Requires RARP Server on Same Segment:** RARP requests are typically broadcast at the link layer and are not forwarded by routers. Therefore, a RARP server must be present on each local network segment where RARP clients reside.
    *   **MAC Address Dependency:** Relies solely on MAC addresses for identification.

*   **Exam Angle (from pyqs.md):**
    *   **"Explain working of RARP"**: Describe the process:
        1.  Client boots and broadcasts a RARP Request containing its MAC address, asking for its IP.
        2.  RARP server on the same network receives the request.
        3.  Server looks up the MAC address in its configuration table.
        4.  Server sends a RARP Reply (unicast) back to the client with the assigned IP address.
        5.  Client configures itself with the received IP.
    *   Mention its purpose (MAC to IP for diskless clients) and that it's largely replaced by DHCP/BOOTP. Highlight its limitation of only providing an IP address.

### **ICMP (Internet Control Message Protocol)**

*   **Purpose:**
    *   ICMP is a supporting protocol in the Internet protocol suite. It is used by network devices, like routers, to send **error messages and operational information** indicating, for example, that a requested service is not available or that a host or router could not be reached.
    *   **Feedback Mechanism for IP:** The Internet Protocol (IP) itself is a best-effort, connectionless datagram delivery service. IP does not have built-in mechanisms for error reporting or control. ICMP provides this feedback.
    *   It is **not** designed to make IP reliable; TCP handles reliability at the transport layer. ICMP reports errors *about* IP packet processing.
    *   Used for **diagnostic and control purposes**.

*   **Encapsulation:**
    *   ICMP messages are encapsulated **directly within IP datagrams**.
    *   There is no intermediate transport layer protocol (like TCP or UDP) for ICMP. The IP header's "Protocol" field will have a value of `1` to indicate that the IP payload is an ICMP message.
    *   Structure: `[ Ethernet Header [ IP Header [ ICMP Header | ICMP Data ] Ethernet Trailer ] ]`

*   **ICMP Message Format (General Structure):**
    *   All ICMP messages have a common basic structure for the first few bytes:
        *   **Type (8 bits):** Identifies the ICMP message type (e.g., Echo Reply, Destination Unreachable).
        *   **Code (8 bits):** Provides further details about the message type (e.g., for Destination Unreachable, a code might specify "Network Unreachable" or "Port Unreachable").
        *   **Checksum (16 bits):** A checksum calculated over the entire ICMP message (header and data) for error detection within the ICMP message itself.
        *   **Rest of Header/Data (Variable):** The content and structure of the rest of the message depend on the `Type` and `Code` fields. For error messages, this part often includes the IP header and the first 8 bytes of the original IP datagram that caused the error (so the source of the original datagram can match the error to a specific communication).

*   **Common Message Types (Purpose and Simple Scenario):**

    1.  **Type 0: Echo Reply**
        *   **Code:** 0
        *   **Purpose:** Used to reply to an Echo Request message. This is the basis of the `ping` utility.
        *   **Scenario:** Host A pings Host B. Host A sends an ICMP Echo Request (Type 8) to Host B. If Host B is reachable and configured to respond, it sends back an ICMP Echo Reply (Type 0) to Host A.

    2.  **Type 3: Destination Unreachable**
        *   **Purpose:** Sent by a router or the destination host to inform the source host that a datagram cannot be delivered.
        *   **Common Codes:**
            *   **Code 0: Network Unreachable:** A router cannot find a path to the destination network.
                *   *Scenario:* A host tries to send a packet to an IP address on a network for which its default gateway has no route.
            *   **Code 1: Host Unreachable:** A router can reach the destination network, but cannot deliver the datagram to the specific host (e.g., ARP failure for the final destination host).
                *   *Scenario:* A router on the destination LAN ARPs for the destination IP, but gets no reply.
            *   **Code 2: Protocol Unreachable:** The transport protocol in the original datagram (e.g., UDP, TCP) is not active or supported on the destination host.
                *   *Scenario:* A host sends a UDP packet to a server, but no application is listening on that UDP port and the OS doesn't support the protocol for that operation.
            *   **Code 3: Port Unreachable:** The destination port (for UDP or TCP) is not available on the destination host (e.g., no service listening on that port).
                *   *Scenario:* A client tries to connect to a web server on TCP port 81, but the server is only listening on port 80.
            *   **Code 4: Fragmentation Needed and DF bit set:** A router needs to fragment a packet but the "Don't Fragment" (DF) bit in the IP header is set.
                *   *Scenario:* A packet is larger than the MTU of an outgoing link, and the sender disallowed fragmentation.

    3.  **Type 5: Redirect Message**
        *   **Purpose:** Sent by a router to inform a source host that there is a better (more direct) route to a particular destination.
        *   **Scenario:** Host A sends a packet to Host B via Router R1. R1 knows that Router R2 is a more direct path to Host B (perhaps Host A, R1, and R2 are on the same LAN). R1 forwards the packet to R2 and also sends an ICMP Redirect message to Host A, telling it to send future packets for Host B directly to R2.
        *   **Common Codes:**
            *   Code 0: Redirect datagrams for the Network.
            *   Code 1: Redirect datagrams for the Host.

    4.  **Type 8: Echo Request**
        *   **Code:** 0
        *   **Purpose:** Used to solicit an Echo Reply from a host or router to test reachability. This is the message sent by the `ping` utility.
        *   **Scenario:** (See Echo Reply)

    5.  **Type 11: Time Exceeded**
        *   **Purpose:** Sent by a router when it discards a datagram because its Time-To-Live (TTL) field in the IP header has reached zero. Also sent by a host if a reassembly timer expires before all fragments of a datagram arrive.
        *   **Common Codes:**
            *   **Code 0: Time to Live exceeded in transit:** A router decremented the TTL to 0.
                *   *Scenario:* A packet is caught in a routing loop, and its TTL eventually expires. This prevents packets from circulating indefinitely. The `traceroute` utility uses this.
            *   **Code 1: Fragment reassembly time exceeded:** A host did not receive all fragments of a datagram within its reassembly time limit.
                *   *Scenario:* Some fragments of a large IP datagram are lost in transit, and the destination host gives up waiting for them.

*   **Exam Angle (from pyqs.md):**
    *   **"Brief note on ICMP using its frame formats"**: Explain ICMP's purpose (error/control for IP). Describe the general format (Type, Code, Checksum, Data). Then, pick 2-3 common types (e.g., Echo Request/Reply, Destination Unreachable, Time Exceeded), state their Type/Code values, and what the 'Data' typically contains (especially the IP header of the offending packet for error messages).
    *   **"Purpose"**: Focus on ICMP being a companion to IP for error reporting and network diagnostics, not for making IP reliable.
    *   **"Common message types"**: List and explain the purpose of Echo Request/Reply, Destination Unreachable (with a few codes like network/host/port unreachable), Time Exceeded (TTL), and Redirect. Provide simple scenarios for each.
    *   **"Short Note"**: A concise summary of purpose, its role with IP, key message types (ping, destination unreachable), and its encapsulation within IP.

### **IPv6**

*   **Purpose:**
    *   IPv6 is the latest version of the Internet Protocol, designed to replace IPv4. It is used to identify devices on the network and to route data packets between them.
    *   **Key Features:**
        *   **Expanded Address Space:** IPv6 supports a much larger address space compared to IPv4.
        *   **Simplified Header:** IPv6 has a simpler header format than IPv4.
        *   **Enhanced Security:** IPv6 includes built-in security features.
        *   **Automatic Configuration:** IPv6 devices can configure their addresses automatically.
    *   **Why it's needed:** As the number of devices connected to the internet continues to grow, IPv4 addresses are becoming scarce. IPv6 provides a solution to this problem by offering a much larger address space.

*   **Working Principle:**

    1.  **Address Format:**
        *   IPv6 addresses are 128 bits long and are written in eight groups of four hexadecimal digits, separated by colons.
        *   For example, 2001:0db8:85a3:0000:0000:8a2e:0370:7334 is a valid IPv6 address.

    2.  **Routing:**
        *   IPv6 uses a different routing algorithm than IPv4.
        *   Routers in the IPv6 network are called "gateways" and they are responsible for forwarding packets between different networks.

    3.  **Security:**
        *   IPv6 includes built-in security features, such as authentication and encryption.

*   **Exam Angle (from pyqs.md):**
    *   **"Explain working of IPv6"**: Describe the address format, routing, and security features of IPv6.
    *   **"Purpose"**: Focus on IPv6 being a solution to the scarcity of IPv4 addresses and its advanced features.
    *   **"Short Note"**: A concise summary of IPv6's purpose, address format, and key features.

### **IPv4 vs. IPv6 Comparison**

*   **Primary Motivation for IPv6:** Exhaustion of the IPv4 address space.

*   **Comparison Table:**

    | Feature                 | IPv4                                     | IPv6                                                     |
    |-------------------------|------------------------------------------|----------------------------------------------------------|
    | **Address Space**       | 32-bit (approx. 4.3 billion addresses)   | 128-bit (approx. 3.4 x 10^38 addresses)                  |
    | **Address Notation**    | Dotted Decimal (e.g., `192.168.1.1`)      | Hexadecimal Colon-Separated (e.g., `2001:0db8::1`)         |
    | **Address Configuration** | Manual, DHCP                             | Stateless Address Autoconfiguration (SLAAC), DHCPv6        |
    | **Header Size**         | 20 bytes (variable with options)         | 40 bytes (fixed size for base header)                    |
    | **Header Complexity**   | More fields, some optional               | Simpler, streamlined, fewer base header fields           |
    | **Checksum**            | In IPv4 header (covers only the header)  | Removed from IP header (relies on Link Layer & Transport Layer checksums) |
    | **Fragmentation**       | Done by sending host and routers         | Done only by the sending host; Path MTU Discovery is used |
    | **Broadcast**           | Yes (e.g., `255.255.255.255`)             | No (Replaced by Multicast and Anycast)                   |
    | **Security (IPSec)**    | Optional, external addition              | Integrated, mandatory support (though not always used)     |
    | **Quality of Service (QoS)** | `Type of Service` (ToS) field; often ignored | `Traffic Class` and `Flow Label` fields for better QoS handling |
    | **Configuration**       | Often manual or requires DHCP server     | Supports stateless autoconfiguration (SLAAC) and DHCPv6    |
    | **Mobility**            | Mobile IP (complex)                      | Mobile IPv6 (more efficient and integrated)                |
    | **Packet Size**         | Min 576 bytes required, typical 1500 bytes MTU | Min 1280 bytes required, supports jumbograms             |

*   **Salient Features of IPv6 (Summary & Expansion):**

    1.  **Massive Address Space:** The most significant advantage, addressing the exhaustion of IPv4 addresses and allowing for a vast number of devices and services.
    2.  **Simplified Header:**
        *   The IPv6 header is fixed at 40 bytes, whereas the IPv4 header is 20 bytes plus options (variable).
        *   Fewer fields in the main header (8 fields vs. 14 in IPv4). This speeds up router processing as routers don't need to parse many optional fields.
        *   Options are handled through "Extension Headers" placed between the IPv6 header and the upper-layer payload. This makes processing more efficient as routers typically only look at the main header.
    3.  **End-to-End Connectivity & Autoconfiguration:**
        *   Every device can potentially have a unique global IP address, restoring the end-to-end principle that NAT in IPv4 somewhat broke.
        *   **Stateless Address Autoconfiguration (SLAAC):** Allows devices to automatically configure their own IPv6 address using their MAC address and router advertisements, without needing a DHCP server for basic connectivity.
    4.  **Enhanced Security (IPSec Integration):**
        *   IPSec (Authentication Header - AH, and Encapsulating Security Payload - ESP) support is a mandatory part of the IPv6 protocol suite.
        *   While mandatory to implement, its use is optional.
        *   Provides a framework for network-layer encryption and authentication.
    5.  **Improved QoS Support:**
        *   **Traffic Class (8 bits):** Similar to IPv4's DiffServ field, used for differentiating classes or priorities of traffic.
        *   **Flow Label (20 bits):** A new field designed to identify packets belonging to the same "flow." This allows routers to apply specific handling to a sequence of packets (e.g., for real-time video or voice) without deep packet inspection.
    6.  **No Broadcasts, More Efficient Multicast:**
        *   IPv6 does not use broadcast messages. Their functionality (sending to all nodes on a segment) is replaced by sending to the link-local all-nodes multicast address (`ff02::1`).
        *   Extensive use of multicast for various functions (e.g., neighbor discovery uses multicast).
        *   Introduces **Anycast** addressing: a packet sent to an anycast address is delivered to only one of the interfaces (typically the "nearest" one) configured with that address.
    7.  **No Router Fragmentation:**
        *   In IPv6, only the source host can fragment a packet.
        *   If a router receives an IPv6 packet larger than the MTU of the next hop, it discards the packet and sends an ICMPv6 "Packet Too Big" message back to the source.
        *   The source then uses Path MTU Discovery (PMTUD) to determine the appropriate size and sends pre-fragmented packets.
        *   This reduces processing load on routers.
    8.  **Extensibility:** The use of Extension Headers allows for new features and options to be added more easily in the future without changing the base IPv6 header.
    9.  **Mobility:** Mobile IPv6 (MIPv6) is more efficient and robust than Mobile IPv4, allowing devices to maintain the same IP address while moving between networks.

*   **Exam Angle (from pyqs.md):**
    *   **"Comparative study of IPv4 and IPv6" / "Differences":** The table above is a good starting point. Be ready to elaborate on key points like address space, header, security, and autoconfiguration.
    *   **"Salient features of IPv6":** List and explain the key benefits and changes introduced by IPv6 as detailed above (Large Address Space, Simplified Header, SLAAC, IPSec, QoS, No Broadcast, No Router Fragmentation).
    *   Focus on *why* these changes were made (e.g., IPv4 address exhaustion, router performance, security needs). 