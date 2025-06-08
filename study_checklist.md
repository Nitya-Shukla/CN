## Study Plan Checklist for Detailed Answers

This checklist will guide you in preparing detailed answers for each topic in your 3-day study plan. For each item, ensure your notes and understanding allow you to explain the concept clearly, as if to someone unfamiliar with it, and enable you to reconstruct the answer in your own words during an exam.

---

### **Day 1: Foundations, Data Link Layer & Intro to MAC**

**1. Topic: Network Models & Fundamentals**

    *   **OSI vs. TCP/IP:**
        *   [x] **Diagram:** Draw both models side-by-side, clearly labeling all layers.
        *   [x] **Layer Functions:** For each layer in both models:
            *   [x] List its primary function(s) (1-2 key functions per layer).
            *   [x] Name 1-2 common protocols operating at that layer (e.g., IP at Network, TCP/HTTP at Transport/Application).
        *   [x] **Key Differences:** Create a table or bullet list highlighting:
            *   [x] Number of layers.
            *   [x] When protocols were defined relative to the model.
            *   [x] Reliability (OSI as a model vs. TCP/IP as a protocol suite).
            *   [x] Any other significant distinctions (e.g., OSI's session/presentation vs. TCP/IP's application layer encompassing these).
        *   [x] **Recall Cue:** Create a mnemonic or simple sentence to remember the layers in order for both models.
        *   [x] **Exam Angle (from pyqs.md):** Be ready to "Compare and contrast," "Explain functions of each layer," and "Name protocols."

    *   **Connection-Oriented vs. Connectionless Services:**
        *   [x] **Definitions:** Clearly define connection-oriented and connectionless services.
        *   [x] **Examples:** Provide TCP as a prime example for connection-oriented and UDP for connectionless.
        *   [x] **Key Characteristics (Table/List):** Compare them based on:
            *   [x] Reliability (guaranteed delivery vs. best-effort).
            *   [x] Setup (connection establishment phase vs. no setup).
            *   [x] Overhead (acknowledgements, state maintenance vs. lower overhead).
            *   [x] Sequencing (in-order delivery vs. out-of-order possibility).
            *   [x] Use cases (when to use which).
        *   [x] **Analogy:** Think of an analogy (e.g., phone call vs. postal mail) to explain the difference.
        *   [x] **Exam Angle (from pyqs.md):** Be ready to "Differentiate," explain "principal difference," or give examples.

    *   **Network Topologies & Media:**
        *   [x] **Topologies (Bus, Star, Ring, Mesh, Hybrid):** For each:
            *   [x] **Sketch:** Draw a simple diagram representing the topology.
            *   [x] **Working:** Briefly explain how data flows.
            *   [x] **Advantages (1-2):** e.g., cost, ease of installation, fault tolerance.
            *   [x] **Disadvantages (1-2):** e.g., single point of failure, scalability, cabling complexity.
        *   [x] **Transmission Media (Guided vs. Unguided):**
            *   [x] **Guided (Twisted Pair, Coax, Fiber):** For each, list basic properties (e.g., bandwidth, susceptibility to interference, cost) and common use cases.
            *   [x] **Unguided (Radio, Microwave, Infrared):** For each, list basic properties and common use cases.
            *   [x] **Comparison:** Briefly highlight the main differences between guided and unguided media.
        *   [x] **Exam Angle (from pyqs.md):** Be ready to "Explain advantages/disadvantages," "Draw," and "Discuss."

    *   **Data Rate & Bandwidth:**
        *   [x] **Definitions:** Clearly define Data Rate (bits per second) and Bandwidth (range of frequencies a channel can carry, or sometimes used synonymously with data rate in digital contexts). Clarify the distinction if relevant.
        *   [x] **Shannon-Hartley Theorem:**
            *   [x] Write down the formula: `C = B * log2(1 + S/N)`.
            *   [x] Define each term: C (Channel Capacity), B (Bandwidth in Hz), S (Average received signal power), N (Average noise power), S/N (Signal-to-Noise Ratio).
            *   [x] Explain what the theorem implies (theoretical maximum data rate).
            *   [x] **Numerical Problem Practice:** Work through the Dec 2024 (Q3a) example and create a similar one. Ensure you can manipulate the formula if one variable is unknown.
        *   [x] **Exam Angle (from pyqs.md):** Be ready to "Define," and solve numerical problems using Shannon-Hartley.

**2. Topic: Data Link Layer (DLL) - Services & Error/Flow Control Concepts**

    *   **DLL Services:**
        *   [x] List and briefly explain the main services:
            *   [x] **Framing:** How data is divided into manageable units.
            *   [x] **Error Control:** Detecting and/or correcting errors.
            *   [x] **Flow Control:** Managing the rate of data transmission.
            *   [x] **Access Control:** Determining which device can use the channel (if shared).
        *   [x] **Recall Cue:** Think "FEFA" (Framing, Error, Flow, Access).
        *   [x] **Exam Angle (from pyqs.md):** Be ready to "Explain DLL services."

    *   **Error Detection & Correction Mechanisms:**
        *   [x] **Parity (Simple, 2D):**
            *   [x] Explain the concept (adding a bit to make the number of 1s even or odd).
            *   [x] Provide a simple example for both.
            *   [x] Discuss limitations (e.g., simple parity detects only odd number of bit errors).
        *   [x] **Checksum:**
            *   [x] Explain the concept (summing data segments, transmitting the sum/negative sum).
            *   [x] Provide a simple conceptual example (no need for complex calculations unless specified in problems).
        *   [x] **CRC (Cyclic Redundancy Check):**
            *   [x] Explain the conceptual understanding (based on polynomial division, generating a checksum).
            *   [x] Describe its advantages (stronger error detection).
            *   [x] (Optional, if time permits or frequently asked) Understand a very simple example of polynomial division.
        *   [x] **Comparison:** Briefly compare the effectiveness of these methods.
        *   [x] **Exam Angle (from pyqs.md):** Be ready to "Explain different error detection and correction mechanisms with examples."

    *   **Flow Control Mechanisms (Conceptual):**
        *   [x] **Stop-and-Wait:**
            *   [x] Explain the basic idea (send one frame, wait for ACK before sending next).
            *   [x] Diagram: Show a simple timeline with frame, ACK, and potential timeout.
            *   [x] Discuss its simplicity and inefficiency (especially on long-delay links).
        *   [x] **Sliding Window:**
            *   [x] Explain the core concept (allowing multiple frames to be in transit).
            *   [x] Explain the need for sequence numbers and acknowledgements (ACKs).
            *   [x] Define sender and receiver window concepts at a high level.
        *   [x] **Exam Angle (from pyqs.md):** Be ready to "Explain flow control mechanisms," "Describe stop and wait," and explain "sliding window flow control."

**3. Topic: DLL Protocols (Sliding Window Protocols)**

    *   **Stop-and-Wait ARQ:**
        *   [x] **Working Principle:** Explain how it handles errors (using ACKs and timeouts for retransmission).
        *   [x] **Diagrams:**
            *   [x] Successful transmission.
            *   [x] Lost frame scenario.
            *   [x] Lost ACK scenario.
            *   [x] Premature timeout scenario.
        *   [x] **Efficiency:** Discuss its limitations in terms of channel utilization.
        *   [x] **Recall Cue:** "Send 1, Wait 1."
        *   [x] **Exam Angle (from pyqs.md):** "Explain with diagrams," "Describe."

    *   **Go-Back-N ARQ:**
        *   [x] **Working Principle:** Explain sender/receiver window size (receiver window = 1), cumulative ACKs.
        *   [x] **Retransmission Strategy:** How it retransmits the erroneous frame and all subsequent frames.
        *   [x] **Diagram Scenarios:**
            *   [x] Frame loss.
            *   [x] ACK loss (and how cumulative ACK can help).
        *   [x] **Advantages/Disadvantages:** (e.g., more efficient than S&W, but can be wasteful on error-prone links).
        *   [x] **Exam Angle (from pyqs.md):** "Explain working principle," "Explain with diagrams."

    *   **Selective Repeat ARQ:**
        *   [x] **Working Principle:** Explain sender/receiver window sizes (both > 1), individual ACKs.
        *   [x] **Retransmission Strategy:** How it retransmits only the specific lost/damaged frame.
        *   [x] **Buffering:** Receiver buffers correctly received but out-of-order frames.
        *   [x] **Diagram Scenarios:**
            *   [x] Frame loss (show buffering of subsequent frames and later delivery).
            *   [x] Lost ACK (show individual timer for frame causing retransmission).
        *   [x] **Window Size Constraint:** `Ws + Wr <= 2^m` (or `Ws = Wr = 2^(m-1)`).
        *   [x] **Recall Cue:** "Selectively resend only what's missing."
        *   [x] **Exam Angle (from pyqs.md):** "Explain working principle," "Diagram scenarios," "Compare with GBN."

    *   **Comparison: GBN vs. SR:**
        *   [x] **Table:** Create a table summarizing key differences (Retransmission strategy, Receiver window size, Buffering at receiver, ACKs, Complexity, Efficiency on noisy links).
        *   [x] **Explanation:** Elaborate on each point in the table.
        *   [x] **Use Cases:** When to prefer one over the other.
        *   [x] **Exam Angle (from pyqs.md):** This is a frequent comparison question. "Compare GBN and SR."

**4. Topic: Framing & HDLC**

    *   **Framing Methods:**
        *   [x] **Character Count:** Explain, diagram, pros/cons (esp. error vulnerability).
        *   [x] **Character Stuffing (Byte Stuffing):** Explain FLAG and ESC, diagram an example of stuffing/destuffing.
        *   [x] **Bit Stuffing:** Explain flag pattern (e.g., 01111110), how 0 is inserted after five 1s, diagram an example. **(Practice this as per May 2018 CBGS/GS)**.
        *   [x] **Physical Layer Coding Violations:** Explain the concept.
        *   [x] **Comparison:** Briefly compare methods.
        *   [x] **Exam Angle (from pyqs.md):** "Explain different methods," "List and explain," "Explain bit stuffing with example."

    *   **HDLC (High-Level Data Link Control):**
        *   [x] **Frame Format:** Draw and label: Flag, Address, Control, Information, FCS, Flag.
        *   [x] **Function of Each Field:** Briefly explain what each field is for.
            *   [x] **Flag:** `01111110` (and bit stuffing context).
            *   [x] **Address:** Identifies secondary station.
            *   [x] **Control:** Key field! Explain I-frames (Information), S-frames (Supervisory - RR, RNR, REJ, SREJ), U-frames (Unnumbered - link setup/teardown). Mention N(S), N(R), P/F bit.
            *   [x] **Information:** User data (in I-frames).
            *   [x] **FCS:** Error detection (CRC).
        *   [x] **Recall Cue:** "Flags Always Control Information For Sure, Flags."
        *   [x] **Exam Angle (from pyqs.md):** "Draw frame format and state function of each field," "Explain types of frames in HDLC."

**5. Topic: Introduction to MAC Sublayer & ALOHA**

    *   **Function of MAC Sublayer:**
        *   [x] **Primary Role:** Explain that MAC is primarily about controlling access to a shared medium (preventing/managing collisions).
        *   [x] **Other Functions:** Briefly mention its role in MAC addressing and framing (adding MAC headers).
        *   [x] **Exam Angle (from pyqs.md):** "What is MAC?" (This is the core of this question).

    *   **ALOHA Protocols:**
        *   [x] **Pure ALOHA:**
            *   [x] **Principle:** Transmit whenever ready.
            *   [x] **Vulnerable Period:** Explain (2 * Frame transmission time). Diagram this.
            *   [x] **Maximum Throughput:** State it (18.4% or 0.184).
            *   [x] **Numerical Problems:** Practice calculating throughput or max number of users given parameters (e.g., May 2023 Q4b).
        *   [x] **Slotted ALOHA:**
            *   [x] **Principle:** Transmit only at the beginning of a time slot.
            *   [x] **Vulnerable Period:** Explain (1 * Frame transmission time). Diagram this.
            *   [x] **Maximum Throughput:** State it (36.8% or 0.368).
            *   [x] **Numerical Problems:** Practice (e.g., May 2022 Q2b, May 2018 CBGS Q5b).
        *   [x] **Comparison:** Briefly compare Pure and Slotted ALOHA (throughput, complexity).
        *   [x] **Exam Angle (from pyqs.md):** "Explain working principle," "vulnerable period," "max throughput," numerical problems.

**6. Review Day 1 & Day 2**

    *   [x] **Active Recall:**
        *   [x] Explain the difference between CSMA/CD and CSMA/CA, including BEB and RTS/CTS.
        *   [x] Take a sample IP address and subnet it for a given number of subnets. Calculate all relevant addresses.
        *   [x] Verbally explain the steps of Distance Vector and Link State routing using a small mental graph.
    *   [x] **Identify Weak Spots:** Revisit subnetting calculations and routing algorithm examples if needed.

---

### **Day 2: MAC cont., Network Layer**

**1. Topic: CSMA Protocols & Collision-Free**

    *   **CSMA (Carrier Sense Multiple Access):**
        *   [x] **Core Idea:** "Listen before talk."
        *   [x] **Persistent CSMA (1-persistent, p-persistent):**
            *   [x] **1-persistent:** Explain (sense channel; if idle, transmit; if busy, keep sensing and transmit immediately when idle). Discuss propagation delay issues leading to collisions.
            *   [x] **p-persistent:** Explain (sense channel; if idle, transmit with probability 'p', or defer with probability '1-p'; if busy, wait and restart).
            *   [x] **Non-persistent:** Explain (sense channel; if idle, transmit; if busy, wait a random amount of time then restart).
            *   [x] **Pros/Cons:** For each persistence method, briefly list pros and cons (e.g., channel utilization, collision rates).
        *   [x] **Exam Angle (from pyqs.md):** "Explain Persistent and Non-persistent CSMA."

    *   **CSMA/CD (Collision Detection):**
        *   [x] **Working Principle:**
            *   [x] Explain the steps: Sense -> Transmit -> Detect Collision (while transmitting) -> Jam Signal -> Backoff.
            *   [x] Emphasize "detect while transmitting."
            *   [x] Where used: Ethernet.
        *   [x] **Binary Exponential Backoff (BEB):**
            *   [x] Explain how it works: After a collision, double the random waiting time range for each subsequent collision.
            *   [x] Provide a simple example calculation of backoff time ranges for a few collisions.
        *   [x] **Exam Angle (from pyqs.md):** "Explain working principle," "BEB with example," "Short Note."

    *   **CSMA/CA (Collision Avoidance):**
        *   [x] **Working Principle:**
            *   [x] Explain why it's needed (e.g., wireless, can't easily detect collisions while transmitting).
            *   [x] Explain RTS/CTS (Request to Send/Clear to Send) mechanism for hidden station problem. Draw a simple diagram illustrating hidden stations and how RTS/CTS helps.
            *   [x] Interframe Spacing (IFS), contention window.
        *   [x] **Where used:** WiFi (IEEE 802.11).
        *   [x] **Exam Angle (from pyqs.md):** "Explain working principle," "RTS/CTS," "Short Note."

    *   **Collision-Free Protocols (Briefly):**
        *   [x] **Concept:** Protocols that aim to avoid collisions altogether, often through reservations or taking turns.
        *   [x] **Binary Countdown:**
            *   [x] Explain the concept (stations broadcast their addresses bit by bit; higher addresses drop out).
            *   [x] Simple example.
        *   [x] **Adaptive Tree Walk Protocol:**
            *   [x] Explain the concept (nodes in a tree are probed to find ready stations).
            *   [x] Simple example (like the May 2018 GS problem: "Sixteen stations...").
        *   [x] **Exam Angle (from pyqs.md):** "Explain Binary Countdown," "Explain Adaptive Tree Walk," solve related problems.

**2. Topic: IP Addressing (Classes & Basics)**

    *   **IPv4 Address Structure:**
        *   [x] Explain: 32-bit, hierarchical, dotted decimal notation.
        *   [x] Show an example and identify network and host portions (conceptually, before classes).
    *   **Classes (A, B, C, D, E):** For each class:
        *   [x] **Identifying Bits:** (e.g., Class A starts with 0, Class B with 10, Class C with 110).
        *   [x] **Range of First Octet:** (e.g., Class A: 1-126).
        *   [x] **Network/Host ID Bits:** How many bits for Network ID, how many for Host ID.
        *   [x] **Default Subnet Mask:** Write it in dotted decimal and CIDR notation (e.g., Class A: 255.0.0.0 or /8).
        *   [x] **Number of Networks/Hosts:** Formula and example calculation.
        *   [x] **Practice:** Given an IP address, identify its class, default mask, network ID, and host ID.
        *   [x] **Exam Angle (from pyqs.md):** "Explain various classes of IP addresses (network-ID bits, host-ID bits, etc.)," "Identify class."

    *   **Special Addresses:**
        *   [x] **Loopback Address:** (e.g., 127.0.0.1) - Purpose.
        *   [x] **Private IP Addresses:** Ranges for Class A, B, C (e.g., 10.0.0.0/8, 172.16.0.0/12, 192.168.0.0/16) - Purpose (NAT).
        *   [x] **Broadcast Address:** Network broadcast (all host bits = 1), Limited broadcast (255.255.255.255) - Purpose.
        *   [x] **Network Address:** (all host bits = 0) - Purpose.
    *   **Subnet Mask Concept (Introduction):**
        *   [x] Explain its purpose: To divide the host portion of an IP address into a subnet portion and a (smaller) host portion.
        *   [x] How it works (ANDing with IP address to get network address).

**3. Topic: Subnetting & CIDR**

    *   **Subnetting:**
        *   [x] **Purpose:** Explain why subnetting is done (e.g., manageability, security, reduce broadcast domain size, overcome shortage of network IDs).
        *   [x] **Process ("Borrowing Host Bits"):** Clearly explain how bits are borrowed from the host portion to create a subnet ID.
        *   [x] **Calculations (Given an IP block and number of subnets OR hosts per subnet):**
            *   [x] **New Subnet Mask:** Calculate it (in dotted decimal and CIDR).
            *   [x] **Number of Subnets Created:** (2^s, where s is subnet bits).
            *   [x] **Number of Usable Hosts per Subnet:** (2^h - 2, where h is remaining host bits).
            *   [x] **For each Subnet:**
                *   [x] **Network Address (Subnet ID):**
                *   [x] **First Usable Host Address:**
                *   [x] **Last Usable Host Address:**
                *   [x] **Broadcast Address for that Subnet:**
        *   [x] **INTENSIVE PRACTICE:** Work through MANY numerical examples, including May 2024 Q5. Create your own problems.
        *   [x] **Key Formulas/Steps:** Write down a clear step-by-step guide for yourself to solve subnetting problems.
        *   [x] **Exam Angle (from pyqs.md):** "What is subnet mask?", Numerical problems are VERY common.

    *   **CIDR (Classless Inter-Domain Routing):**
        *   [x] **Concept:** Explain how it eliminates the concept of rigid classes (A, B, C).
        *   [x] **/notation:** Explain what `/x` means (first x bits are the network prefix).
        *   [x] **Advantages:** Address space efficiency, route summarization (supernetting).
        *   [x] **Relation to Subnetting:** How subnet masks are represented in CIDR notation.

**4. Topic: ARP, RARP, ICMP & IPv6 Intro**

    *   **ARP (Address Resolution Protocol):**
        *   [x] **Purpose:** Map IP address to MAC address on a local network.
        *   [x] **Working Principle:**
            *   [x] ARP Request (broadcast): "Who has IP address X, tell MAC address Y?"
            *   [x] ARP Reply (unicast): "I have IP address X, my MAC address is Z."
            *   [x] ARP Cache: How it's used to store mappings.
        *   [x] **Scenario:** Explain when ARP is used (e.g., host A wants to send a packet to host B on the same LAN).
        *   [x] **Exam Angle (from pyqs.md):** "Explain working of ARP," "Short Note."

    *   **RARP (Reverse ARP):**
        *   [x] **Purpose:** Map MAC address to IP address (less common now, BOOTP/DHCP largely replaced it).
        *   [x] **Basic Idea:** Client broadcasts its MAC, RARP server replies with IP.
        *   [x] **Exam Angle (from pyqs.md):** "Explain working of RARP."

    *   **ICMP (Internet Control Message Protocol):**
        *   [x] **Purpose:** Error reporting and diagnostic functions for IP. (IP itself is unreliable, ICMP provides feedback).
        *   [x] **Common Message Types (Explain purpose and simple scenario for each):**
            *   [x] Echo Request/Reply (used by `ping`).
            *   [x] Destination Unreachable (host, network, port unreachable).
            *   [x] Time Exceeded (TTL expired).
            *   [x] Redirect (better route exists).
        *   [x] **ICMP Frame Format (Briefly, if specified in pyqs.md):** Key fields like Type, Code, Checksum.
        *   [x] **Encapsulation:** ICMP messages are encapsulated within IP packets.
        *   [x] **Exam Angle (from pyqs.md):** "Brief note on ICMP using its frame formats," "Purpose," "Common message types," "Short Note."

    *   **IPv4 vs. IPv6:**
        *   [x] **Key Differences (Create a comparison table):**
            *   [x] **Address Space:** 32-bit vs. 128-bit (solve address exhaustion).
            *   [x] **Header:** Simpler, more efficient header in IPv6 (fewer fields, fixed length options).
            *   [x] **Security:** IPSec support mandatory/integrated in IPv6.
            *   [x] **Autoconfiguration:** Stateless Address Autoconfiguration (SLAAC) in IPv6.
            *   [x] **Fragmentation:** Handled by end hosts in IPv6, not routers.
            *   [x] **Broadcast:** No broadcast in IPv6 (uses multicast).
            *   [x] **Notation:** Dotted decimal vs. hexadecimal colon notation.
        *   [x] **Salient Features of IPv6 (List and briefly explain):** Larger address space, simplified header, security, mobility, QoS.
        *   [x] **Exam Angle (from pyqs.md):** "Comparative study of IPv4 and IPv6," "Differences," "Salient features of IPv6."

**5. Topic: Routing Algorithms**

    *   **Concept:**
        *   [x] **Goal of Routing:** Find the "best" path for data packets from source to destination.
        *   [x] **Metrics:** Common metrics used (hop count, bandwidth, delay, cost).
        *   [x] **Static vs. Dynamic Routing:** Briefly explain the difference.
    *   **Distance Vector (e.g., RIP, Bellman-Ford):**
        *   [x] **Working Principle:**
            *   [x] Each router maintains a distance vector (table of distances to destinations).
            *   [x] Routers periodically share their distance vectors ONLY with directly connected neighbors.
            *   [x] Routers update their tables based on information from neighbors (Bellman-Ford equation concept: `MyCostToX = CostToNeighbor + NeighborCostToX`).
        *   [x] **Bellman-Ford Algorithm:**
            *   [x] Understand the iterative process (relaxing edges).
            *   [x] **Practice with a small graph example:** Show how tables update step-by-step.
        *   [x] **Count-to-Infinity Problem:**
            *   [x] Explain what it is (slow convergence when a link goes down, routing loops).
            *   [x] Briefly mention solutions (split horizon, poison reverse).
        *   [x] **Exam Angle (from pyqs.md):** "Working principle," "Bellman-Ford with example," "Count-to-infinity."

    *   **Link State (e.g., OSPF, Dijkstra):**
        *   [x] **Working Principle:**
            *   [x] Each router discovers its neighbors and measures cost to them.
            *   [x] Each router constructs a Link State Packet (LSP) containing this information.
            *   [x] LSPs are flooded to ALL other routers in the network.
            *   [x] Each router builds a complete map (topology) of the network.
            *   [x] Each router uses Dijkstra's algorithm to compute the shortest path to all destinations.
        *   [x] **Dijkstra's Algorithm:**
            *   [x] Understand the steps (maintaining set of visited nodes, tentative distances).
            *   [x] **Practice with a small graph example:** Show steps of the algorithm.
        *   [x] **Comparison (DV vs LS):** Create a table comparing them (information shared, complexity, convergence speed, robustness, resource usage).
        *   [x] **Exam Angle (from pyqs.md):** "Working principle," "Dijkstra's with example," "Compare with DV."

    *   **Briefly (Conceptual Understanding):**
        *   [x] **Hierarchical Routing:** Purpose (scalability, dividing network into regions/AS).
        *   [x] **Broadcast Routing:** Methods (flooding, spanning tree).
        *   [x] **Multicast Routing:** Purpose (one-to-many), basic idea (multicast groups, trees).
        *   [x] **Exam Angle (from pyqs.md):** "Explain Hierarchical, Broadcast, Multicast routing," "Difference between broadcast and multicast."

**6. Review Day 1 & Day 2**

    *   [x] **Active Recall:**
        *   [x] Explain the difference between CSMA/CD and CSMA/CA, including BEB and RTS/CTS.
        *   [x] Take a sample IP address and subnet it for a given number of subnets. Calculate all relevant addresses.
        *   [x] Verbally explain the steps of Distance Vector and Link State routing using a small mental graph.
    *   [x] **Identify Weak Spots:** Revisit subnetting calculations and routing algorithm examples if needed.

---

### **Day 3: Transport, Application Layers & Full Revision**

**1. Topic: Transport Layer Intro & UDP**

    *   **Transport Layer Services:**
        *   [x] **Process-to-Process Delivery:** Explain (using port numbers to deliver to specific applications).
        *   [x] **Multiplexing/Demultiplexing:**
            *   [x] Explain multiplexing at sender (gathering data from multiple app processes, adding headers, passing to network layer).
            *   [x] Explain demultiplexing at receiver (delivering segments to correct app process based on port numbers).
        *   [x] **(Optional) Reliability:** Provided by TCP, not UDP.
        *   [x] **(Optional) Flow/Congestion Control:** Provided by TCP, not UDP.
        *   [x] **Exam Angle (from pyqs.md):** "Explain transport layer multiplexing and demultiplexing," "QoS parameters (reliability, delay, jitter, bandwidth)."

    *   **UDP (User Datagram Protocol):**
        *   [x] **Characteristics:**
            *   [x] Connectionless (no handshake).
            *   [x] Unreliable ("best effort" delivery, no ACKs, no retransmissions by UDP itself).
            *   [x] Fast, low overhead.
            *   [x] No flow control, no congestion control.
            *   [x] Message-oriented.
        *   [x] **UDP Header Format:**
            *   [x] **Draw:** `Source Port | Destination Port | Length | Checksum`.
            *   [x] **Explain Purpose of Each Field:**
                *   Source Port (optional, for reply).
                *   Destination Port (identifies application process on dest host).
                *   Length (length of UDP header + data in bytes).
                *   Checksum (optional in IPv4, mandatory in IPv6; for error detection in header & data).
        *   [x] **Common UDP Applications:** DNS, DHCP, SNMP, TFTP, streaming media, online gaming. Explain *why* UDP is suitable for these.
        *   [x] **Exam Angle (from pyqs.md):** "Draw and explain header format," "Characteristics," "Short Note."

**2. Topic: TCP (Transmission Control Protocol)**

    *   **TCP Characteristics:**
        *   [x] Connection-oriented (requires handshake).
        *   [x] Reliable (uses ACKs, sequence numbers, retransmissions).
        *   [x] Byte-stream service (application data seen as a continuous stream of bytes).
        *   [x] Full-duplex.
        *   [x] Provides flow control and congestion control.
    *   **TCP Header Format:**
        *   [x] **Draw:** A simplified version showing key fields. (Full header is complex, focus on what's usually asked).
        *   [x] **Label and Explain Purpose of Key Fields:**
            *   [x] Source Port, Destination Port.
            *   [x] Sequence Number (byte stream number of first byte in segment).
            *   [x] Acknowledgement Number (next sequence number expected from other side).
            *   [x] Header Length.
            *   [x] **Flags (Control Bits - VERY IMPORTANT):**
                *   [x] `SYN` (Synchronize - connection request).
                *   [x] `ACK` (Acknowledgement is valid).
                *   [x] `FIN` (Finish - terminate connection).
                *   [x] `RST` (Reset - abort connection).
                *   [x] `PSH` (Push - send data immediately).
                *   [x] `URG` (Urgent pointer field is valid).
            *   [x] **Window Size:** (for flow control - how many bytes receiver is willing to accept).
            *   [x] **Checksum:** (error detection for header & data).
            *   [x] **Urgent Pointer:** (if URG flag is set).
        *   [x] **Exam Angle (from pyqs.md):** "Draw and explain header," "Compare with UDP header," "List fields in TCP not in UDP and why."

    *   **TCP Connection Establishment (3-way handshake):**
        *   [x] **Diagram:** Show the three segments exchanged.
        *   [x] **Flag Sequence:**
            *   [x] Client -> Server: `SYN` (Client Seq=x)
            *   [x] Server -> Client: `SYN` + `ACK` (Server Seq=y, Ack=x+1)
            *   [x] Client -> Server: `ACK` (Client Seq=x+1, Ack=y+1)
        *   [x] Explain purpose of each step (synchronize sequence numbers).
        *   [x] **Exam Angle (from pyqs.md):** "Explain connection establishment," "Diagram."

    *   **TCP Connection Release (4-way or 3-way with piggybacking):**
        *   [x] **Diagram (4-way):** Show the segments.
        *   [x] **Flag Sequence (4-way):**
            *   [x] Side 1 -> Side 2: `FIN` (Seq=x)
            *   [x] Side 2 -> Side 1: `ACK` (Ack=x+1)
            *   [x] Side 2 -> Side 1: `FIN` (Seq=y)
            *   [x] Side 1 -> Side 2: `ACK` (Ack=y+1)
        *   [x] Explain purpose (each side closes its end independently).
        *   [x] Briefly mention 3-way with piggybacked FIN+ACK.
        *   [x] **Exam Angle (from pyqs.md):** "Explain connection release," "Diagram."

    *   **TCP Flow Control (Sliding Window):**
        *   [x] **Concept:** Prevent sender from overwhelming receiver.
        *   [x] **How Window Size is Advertised:** Receiver includes its current `Window Size` (buffer space) in TCP header.
        *   [x] **Sender Action:** Sender limits amount of unacknowledged data to receiver's advertised window size.
        *   [x] **Zero Window:** What happens if receiver advertises window of 0.
        *   [x] **Exam Angle (from pyqs.md):** "Discuss TCP flow control."

    *   **TCP Congestion Control:**
        *   [x] **Purpose:** Prevent TCP from overwhelming the network itself.
        *   [x] **Mechanisms (Conceptual Understanding):**
            *   [x] **AIMD (Additive Increase, Multiplicative Decrease):**
                *   [x] Additive Increase: Increase congestion window (cwnd) slowly when no congestion.
                *   [x] Multiplicative Decrease: Decrease cwnd sharply when congestion detected (e.g., packet loss).
            *   [x] **Slow Start:** Start with small cwnd, increase exponentially until threshold or loss.
            *   [x] **Congestion Avoidance:** After Slow Start threshold, increase linearly (AIMD's additive part).
            *   [x] **Fast Retransmit:** If 3 duplicate ACKs received, assume packet loss and retransmit without waiting for timeout.
            *   [x] **Fast Recovery:** After Fast Retransmit, skip Slow Start and go to Congestion Avoidance.
        *   [x] **Congestion Detection:** How TCP infers congestion (timeouts, duplicate ACKs).
        *   [x] **Exam Angle (from pyqs.md):** "Discuss TCP congestion control," "Short Note on TCP Congestion Control," "AIMD, Slow Start."
        *   [x] **TCP Throughput Problem (May 2024 Q6):** Understand how window size, RTT, and channel capacity affect throughput. Practice this problem.

**3. Topic: Application Layer Protocols**

    *   **DNS (Domain Name System):**
        *   [x] **Purpose:** Translate human-readable hostnames (e.g., www.google.com) to IP addresses, and vice-versa.
        *   [x] **Hierarchical Structure:** Explain (root, TLD, authoritative servers). Draw a simple hierarchy.
        *   [x] **Resolver:** Role of client-side resolver.
        *   [x] **Iterative vs. Recursive Queries (Concept & Diagram):**
            *   [x] **Recursive:** Resolver asks server, server finds answer or asks others.
            *   [x] **Iterative:** Resolver asks server, server refers to another server, resolver asks that one.
        *   [x] **Record Types (Briefly explain purpose of each):**
            *   [x] `A` (IPv4 address).
            *   [x] `AAAA` (IPv6 address).
            *   [x] `CNAME` (Canonical Name - alias).
            *   [x] `MX` (Mail Exchange server).
            *   [x] `NS` (Name Server).
        *   [x] **Caching:** How DNS responses are cached.
        *   [x] **Exam Angle (from pyqs.md):** "Explain services provided by DNS," "Short Note."

    *   **HTTP (HyperText Transfer Protocol):**
        *   [x] **Purpose:** Protocol for fetching resources like HTML documents (foundation of data communication for WWW).
        *   [x] **Client-Server Model:** Explain.
        *   [x] **Persistent vs. Non-persistent Connections:**
            *   [x] **Non-persistent:** One TCP connection per object. Explain overhead.
            *   [x] **Persistent:** Multiple objects over one TCP connection (with/without pipelining). Explain benefits.
        *   [x] **Request/Response Message Format (General Structure):**
            *   [x] **Request Message:** Start Line (Method, URL, Version), Headers (Host, User-Agent, Accept, etc.), Body (optional).
            *   [x] **Response Message:** Status Line (Version, Status Code, Status Phrase), Headers (Content-Type, Content-Length, Server, etc.), Body (requested resource).
        *   [x] **Common HTTP Methods:** GET, POST, HEAD, PUT, DELETE (brief purpose).
        *   [x] **Common Status Codes:** 200 OK, 301 Moved Permanently, 400 Bad Request, 404 Not Found, 500 Internal Server Error (brief meaning).
        *   [x] **Exam Angle (from pyqs.md):** "Request/Response message format," "Persistent vs. Non-persistent," "Short Note."

    *   **SMTP (Simple Mail Transfer Protocol):**
        *   [x] **Purpose:** Protocol for sending (pushing) email messages between mail servers and from MUA to MTA.
        *   [x] **Basic Commands (Conceptual Understanding of interaction):**
            *   [x] `HELO`/`EHLO` (Identify client).
            *   [x] `MAIL FROM:` (Sender's email).
            *   [x] `RCPT TO:` (Recipient's email).
            *   [x] `DATA` (Start of message body).
            *   [x] `QUIT`.
        *   [x] **TCP Port:** Uses TCP port 25.
        *   [x] **Not for retrieving mail** (POP3/IMAP do that).
        *   [x] **Exam Angle (from pyqs.md):** "Basic commands," "Purpose," "Short Note."

    *   **SNMP (Simple Network Management Protocol) (Briefly):**
        *   [x] **Purpose:** Monitoring and managing network devices (routers, switches, servers).
        *   [x] **Components:**
            *   [x] **Manager:** (NMS - Network Management Station) sends requests, receives traps.
            *   [x] **Agent:** Runs on managed devices, responds to manager, sends traps.
            *   [x] **MIB (Management Information Base):** Database of manageable objects on the agent.
        *   [x] **Basic Operations:** GET, SET, TRAP.
        *   [x] **Exam Angle (from pyqs.md):** "Explain SNMP and its functions."

    *   **Electronic Mail (General Components):**
        *   [x] **MUA (Mail User Agent):** Client software (e.g., Outlook, Gmail web interface).
        *   [x] **MTA (Mail Transfer Agent):** Mail server software (e.g., Postfix, Sendmail) - uses SMTP.
        *   [x] **MDA (Mail Delivery Agent):** Delivers email to recipient's mailbox.
        *   [x] Briefly explain the flow of an email using these components.
        *   [x] **Exam Angle (from pyqs.md):** "What is Electronic Mailing? Components."

**4. Topic: Miscellaneous & Short Note Topics**

    *   **Finite State Machines (FSM):**
        *   [x] **What they are:** A model of computation with states, transitions, inputs, and outputs.
        *   [x] **How they model protocols:** Show a very simple example (e.g., Stop-and-Wait ARQ with states like "Ready to Send," "Waiting for ACK").
        *   [x] **Benefits:** Precise specification, aids in implementation and verification.
        *   [x] **Exam Angle (from pyqs.md):** "What is FSM model? How used in protocols? Explain."

    *   **Fragmentation & Reassembly:**
        *   [x] **Why needed:** When a large packet from an upper layer needs to be sent over a network whose MTU (Maximum Transmission Unit) is smaller.
        *   [x] **How it works (IP Fragmentation):**
            *   [x] **IP Header Fields Involved:**
                *   Identification (ID): To identify fragments belonging to the same original datagram.
                *   Flags (DF - Don't Fragment, MF - More Fragments).
                *   Fragment Offset: Position of this fragment in the original datagram.
            *   [x] Explain how routers fragment and how the destination host reassembles.
        *   [x] **Example:** Show a large packet being broken into smaller fragments with correct ID, MF, and Offset values.
        *   [x] **Exam Angle (from pyqs.md):** "Explain Fragmentation and reassembly using suitable example," "Short Note."

    *   **Queuing Theory (M/M/1, Little's Theorem - Very Basic):**
        *   [x] **M/M/1:** Briefly explain what it represents (Poisson arrivals, Exponential service times, 1 server).
        *   [x] **Little's Theorem:** `L = λW` (Average number of items in system = Arrival rate * Average time in system). Explain terms. Simple conceptual application.
        *   [x] **Focus:** Conceptual understanding, not deep math unless specifically preparing for older CBGS type questions.
        *   [x] **Exam Angle (from pyqs.md):** "Explain Little's theorem," "What is M/M/1?"

    *   **IEEE 802 series (General Awareness):**
        *   [x] Know it's a family of standards for LANs/MANs.
        *   [x] Mention a few key ones and what they cover:
            *   802.3 (Ethernet - CSMA/CD).
            *   802.11 (Wireless LAN - WiFi - CSMA/CA).
            *   802.15 (Wireless PAN - Bluetooth).
            *   802.16 (Broadband Wireless Access - WiMAX).
        *   [x] **Exam Angle (from pyqs.md):** "Short Note on IEEE Standards 802 series."

    *   **FDDI (Fiber Distributed Data Interface) (Briefly for short note):**
        *   [x] Token-passing, dual-ring LAN using fiber optic cable. High speed (for its time).
    *   **Service Primitives (Short note):**
        *   [x] Define: Operations available to a layer from the layer below it (e.g., Request, Indication, Response, Confirm). Simple example of how they are used in layer interaction.
    *   **Frame Relay (Short Note):**
        *   [x] WAN protocol, uses virtual circuits (PVCs, SVCs), connection-oriented at DLL.

**5. MEGA REVISION & Practice**

    *   [x] Go through your prepared detailed answers for ALL topics.
    *   [x] Use active recall: Cover your notes and try to explain the concept aloud or write down key points.
    *   [x] **Prioritize:** Focus on the "Key Focus Areas" identified in `pyqs.md` analysis and your personal weak spots.
    *   [x] **Rework Numerical Problems:** Subnetting, Aloha throughput, Shannon, TCP throughput. Ensure you can solve them quickly and accurately.
    *   [x] **Draw Key Diagrams from Memory:** OSI/TCP-IP, TCP/UDP Headers, Sliding Window timelines, 3-way/4-way handshakes.
    *   [x] **Practice Short Note Questions:** For common short note topics, jot down 3-4 key points for each.
    *   [x] **Simulate Exam Conditions:** Try answering a few full questions from past papers within a time limit.

---

**General Tips for Each Answer (Instructions for Reconstruction):**

*   **Start with a Clear Definition:** Define the main concept in simple terms.
*   **Explain the "Why":** Why is this concept/protocol needed? What problem does it solve?
*   **Explain the "How":** Describe its working mechanism. Use steps if applicable.
*   **Use Diagrams:** Visuals are powerful. Draw simple, clear, and well-labeled diagrams for models, frame formats, timelines, handshakes, etc.
*   **Provide Examples:**
    *   Conceptual examples to illustrate a point.
    *   Numerical examples where calculations are involved (and practice them).
    *   Real-world analogies if they help clarify.
*   **Compare and Contrast:** If the topic involves comparing two or more things (e.g., OSI vs TCP/IP, GBN vs SR, TCP vs UDP), use a table or clear bullet points to highlight differences and similarities across key parameters.
*   **Advantages/Disadvantages:** If relevant, list them.
*   **Key Takeaways/Summary:** Conclude with 1-2 main points that are easy to remember.
*   **Use Your Own Words:** While studying, try to rephrase concepts in your own language. This helps solidify understanding and makes recall easier during exams. Avoid rote memorization where possible; focus on understanding the logic.
*   **Structure:** Organize your answer logically with headings or bullet points for readability.
