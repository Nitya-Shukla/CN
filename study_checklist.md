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
        *   [ ] **Parity (Simple, 2D):**
            *   [ ] Explain the concept (adding a bit to make the number of 1s even or odd).
            *   [ ] Provide a simple example for both.
            *   [ ] Discuss limitations (e.g., simple parity detects only odd number of bit errors).
        *   [ ] **Checksum:**
            *   [ ] Explain the concept (summing data segments, transmitting the sum/negative sum).
            *   [ ] Provide a simple conceptual example (no need for complex calculations unless specified in problems).
        *   [ ] **CRC (Cyclic Redundancy Check):**
            *   [ ] Explain the conceptual understanding (based on polynomial division, generating a checksum).
            *   [ ] Describe its advantages (stronger error detection).
            *   [ ] (Optional, if time permits or frequently asked) Understand a very simple example of polynomial division.
        *   [ ] **Comparison:** Briefly compare the effectiveness of these methods.
        *   [ ] **Exam Angle (from pyqs.md):** Be ready to "Explain different error detection and correction mechanisms with examples."

    *   **Flow Control Mechanisms (Conceptual):**
        *   [ ] **Stop-and-Wait:**
            *   [ ] Explain the basic idea (send one frame, wait for ACK before sending next).
            *   [ ] Diagram: Show a simple timeline with frame, ACK, and potential timeout.
            *   [ ] Discuss its simplicity and inefficiency (especially on long-delay links).
        *   [ ] **Sliding Window:**
            *   [ ] Explain the core concept (allowing multiple frames to be in transit).
            *   [ ] Explain the need for sequence numbers and acknowledgements (ACKs).
            *   [ ] Define sender and receiver window concepts at a high level.
        *   [ ] **Exam Angle (from pyqs.md):** Be ready to "Explain flow control mechanisms," "Describe stop and wait," and explain "sliding window flow control."

**3. Topic: DLL Protocols (Sliding Window Protocols)**

    *   **Stop-and-Wait ARQ (Automatic Repeat reQuest):**
        *   [ ] **Working Principle:** Explain how it handles errors (using ACKs and timeouts for retransmission).
        *   [ ] **Diagrams:**
            *   [ ] Successful transmission.
            *   [ ] Lost frame scenario.
            *   [ ] Lost ACK scenario.
            *   [ ] Premature timeout scenario.
        *   [ ] **Efficiency:** Discuss its limitations in terms of channel utilization.
        *   [ ] **Recall Cue:** "Send 1, Wait 1."
        *   [ ] **Exam Angle (from pyqs.md):** "Explain with diagrams," "Describe."

    *   **Go-Back-N ARQ:**
        *   [ ] **Working Principle:**
            *   [ ] Sender window size (Ws > 1), Receiver window size (Wr = 1).
            *   [ ] How it handles lost/damaged frames (sender retransmits the lost frame and all subsequent frames).
            *   [ ] Cumulative ACKs (ACK N means all frames up to N-1 received).
        *   [ ] **Diagram Scenarios:**
            *   [ ] Successful transmission.
            *   [ ] Frame loss (show retransmission of multiple frames).
            *   [ ] Lost ACK (show how timeout or subsequent ACKs recover).
        *   [ ] **Buffering:** Receiver doesn't buffer out-of-order frames.
        *   [ ] **Recall Cue:** "Go back and resend all from error."
        *   [ ] **Exam Angle (from pyqs.md):** "Explain working principle," "Diagram scenarios," "Compare with SR."

    *   **Selective Repeat ARQ:**
        *   [ ] **Working Principle:**
            *   [ ] Sender window size (Ws > 1), Receiver window size (Wr > 1, usually Ws = Wr).
            *   [ ] How it handles lost/damaged frames (sender retransmits only the specific lost/damaged frame).
            *   [ ] Individual ACKs (ACK N means frame N received).
            *   [ ] Buffering at the receiver for out-of-order frames.
        *   [ ] **Diagram Scenarios:**
            *   [ ] Successful transmission.
            *   [ ] Frame loss (show retransmission of only the lost frame and buffering at receiver).
            *   [ ] Lost ACK.
        *   [ ] **Recall Cue:** "Selectively repeat only the bad ones."
        *   [ ] **Exam Angle (from pyqs.md):** "Explain working principle," "Diagram scenarios," "Compare with GBN."

    *   **Comparison: Go-Back-N vs. Selective Repeat:**
        *   [ ] Create a table comparing them based on:
            *   [ ] Efficiency/Throughput (SR generally better).
            *   [ ] Complexity (SR more complex).
            *   [ ] Buffering requirements (SR needs more at receiver).
            *   [ ] Number of retransmissions.
            *   [ ] Window sizes.

**4. Topic: Framing & HDLC**

    *   **Framing Methods:**
        *   [ ] **Character Count:** Explain, discuss problems (error in count).
        *   [ ] **Character Stuffing (Byte Stuffing):**
            *   [ ] Explain (using STX, ETX, DLE).
            *   [ ] Provide an example of stuffing and destuffing.
        *   [ ] **Bit Stuffing:**
            *   [ ] Explain (inserting a 0 after a certain number of consecutive 1s).
            *   [ ] Provide an example of stuffing and destuffing (use the May 2018 CBGS/GS problem as a template).
        *   [ ] **Physical Layer Coding Violations:** Briefly explain the concept.
        *   [ ] **Focus:** Deeply understand Bit Stuffing with examples.
        *   [ ] **Exam Angle (from pyqs.md):** "Explain framing methods," "What is bit/byte stuffing? Explain with example," numerical bit stuffing problem.

    *   **HDLC (High-Level Data Link Control):**
        *   [ ] **Frame Format:**
            *   [ ] Draw the HDLC frame structure: `Flag | Address | Control | Information | FCS | Flag`.
            *   [ ] For each field, briefly explain its purpose (e.g., Flag: 01111110, Address: for secondary station, Control: type of frame - I, S, U, Information: data, FCS: error detection).
        *   [ ] (Optional, if time) Briefly mention the types of frames (Information, Supervisory, Unnumbered).
        *   [ ] **Exam Angle (from pyqs.md):** "Draw frame format," "Explain use of fields."

**5. Topic: Introduction to MAC Sublayer & ALOHA**

    *   **Function of MAC layer:**
        *   [ ] Explain its primary role: Controlling access to the shared medium.
        *   [ ] Mention relation to DLL.
        *   [ ] **Exam Angle (from pyqs.md):** "What is the function of MAC layer?"

    *   **ALOHA Protocols:**
        *   [ ] **Pure ALOHA:**
            *   [ ] **Principle:** Transmit whenever ready.
            *   [ ] **Vulnerable Period:** Explain (2 * Frame transmission time). Diagram this.
            *   [ ] **Maximum Throughput:** State it (18.4% or 0.184).
            *   [ ] **Numerical Problems:** Practice calculating throughput or max number of users given parameters (e.g., May 2023 Q4b).
        *   [ ] **Slotted ALOHA:**
            *   [ ] **Principle:** Transmit only at the beginning of a time slot.
            *   [ ] **Vulnerable Period:** Explain (1 * Frame transmission time). Diagram this.
            *   [ ] **Maximum Throughput:** State it (36.8% or 0.368).
            *   [ ] **Numerical Problems:** Practice (e.g., May 2022 Q2b, May 2018 CBGS Q5b).
        *   [ ] **Comparison:** Briefly compare Pure and Slotted ALOHA (throughput, complexity).
        *   [ ] **Exam Angle (from pyqs.md):** "Explain working principle," "vulnerable period," "max throughput," numerical problems.

**6. Review Day 1**

    *   [ ] **Active Recall:** Without looking at notes, try to:
        *   [ ] Draw OSI and TCP/IP models, label layers, and list one function for each.
        *   [ ] Explain GBN vs. SR to an imaginary student, including diagrams for frame loss.
        *   [ ] Explain pure vs. slotted ALOHA and their vulnerable periods.
    *   [ ] **Identify Weak Spots:** Note any topics you struggled to recall and revisit them.

---

### **Day 2: MAC cont., Network Layer**

**1. Topic: CSMA Protocols & Collision-Free**

    *   **CSMA (Carrier Sense Multiple Access):**
        *   [ ] **Core Idea:** "Listen before talk."
        *   [ ] **Persistent CSMA (1-persistent, p-persistent):**
            *   [ ] **1-persistent:** Explain (sense channel; if idle, transmit; if busy, keep sensing and transmit immediately when idle). Discuss propagation delay issues leading to collisions.
            *   [ ] **p-persistent:** Explain (sense channel; if idle, transmit with probability 'p', or defer with probability '1-p'; if busy, wait and restart).
            *   [ ] **Non-persistent:** Explain (sense channel; if idle, transmit; if busy, wait a random amount of time then restart).
            *   [ ] **Pros/Cons:** For each persistence method, briefly list pros and cons (e.g., channel utilization, collision rates).
        *   [ ] **Exam Angle (from pyqs.md):** "Explain Persistent and Non-persistent CSMA."

    *   **CSMA/CD (Collision Detection):**
        *   [ ] **Working Principle:**
            *   [ ] Explain the steps: Sense -> Transmit -> Detect Collision (while transmitting) -> Jam Signal -> Backoff.
            *   [ ] Emphasize "detect while transmitting."
            *   [ ] Where used: Ethernet.
        *   [ ] **Binary Exponential Backoff (BEB):**
            *   [ ] Explain how it works: After a collision, double the random waiting time range for each subsequent collision.
            *   [ ] Provide a simple example calculation of backoff time ranges for a few collisions.
        *   [ ] **Exam Angle (from pyqs.md):** "Explain working principle," "BEB with example," "Short Note."

    *   **CSMA/CA (Collision Avoidance):**
        *   [ ] **Working Principle:**
            *   [ ] Explain why it's needed (e.g., wireless, can't easily detect collisions while transmitting).
            *   [ ] Explain RTS/CTS (Request to Send/Clear to Send) mechanism for hidden station problem. Draw a simple diagram illustrating hidden stations and how RTS/CTS helps.
            *   [ ] Interframe Spacing (IFS), contention window.
        *   [ ] **Where used:** WiFi (IEEE 802.11).
        *   [ ] **Exam Angle (from pyqs.md):** "Explain working principle," "RTS/CTS," "Short Note."

    *   **Collision-Free Protocols (Briefly):**
        *   [ ] **Concept:** Protocols that aim to avoid collisions altogether, often through reservations or taking turns.
        *   [ ] **Binary Countdown:**
            *   [ ] Explain the concept (stations broadcast their addresses bit by bit; higher addresses drop out).
            *   [ ] Simple example.
        *   [ ] **Adaptive Tree Walk Protocol:**
            *   [ ] Explain the concept (nodes in a tree are probed to find ready stations).
            *   [ ] Simple example (like the May 2018 GS problem: "Sixteen stations...").
        *   [ ] **Exam Angle (from pyqs.md):** "Explain Binary Countdown," "Explain Adaptive Tree Walk," solve related problems.

**2. Topic: IP Addressing (Classes & Basics)**

    *   **IPv4 Address Structure:**
        *   [ ] Explain: 32-bit, hierarchical, dotted decimal notation.
        *   [ ] Show an example and identify network and host portions (conceptually, before classes).
    *   **Classes (A, B, C, D, E):** For each class:
        *   [ ] **Identifying Bits:** (e.g., Class A starts with 0, Class B with 10, Class C with 110).
        *   [ ] **Range of First Octet:** (e.g., Class A: 1-126).
        *   [ ] **Network/Host ID Bits:** How many bits for Network ID, how many for Host ID.
        *   [ ] **Default Subnet Mask:** Write it in dotted decimal and CIDR notation (e.g., Class A: 255.0.0.0 or /8).
        *   [ ] **Number of Networks/Hosts:** Formula and example calculation.
        *   [ ] **Practice:** Given an IP address, identify its class, default mask, network ID, and host ID.
        *   [ ] **Exam Angle (from pyqs.md):** "Explain various classes of IP addresses (network-ID bits, host-ID bits, etc.)," "Identify class."

    *   **Special Addresses:**
        *   [ ] **Loopback Address:** (e.g., 127.0.0.1) - Purpose.
        *   [ ] **Private IP Addresses:** Ranges for Class A, B, C (e.g., 10.0.0.0/8, 172.16.0.0/12, 192.168.0.0/16) - Purpose (NAT).
        *   [ ] **Broadcast Address:** Network broadcast (all host bits = 1), Limited broadcast (255.255.255.255) - Purpose.
        *   [ ] **Network Address:** (all host bits = 0) - Purpose.
    *   **Subnet Mask Concept (Introduction):**
        *   [ ] Explain its purpose: To divide the host portion of an IP address into a subnet portion and a (smaller) host portion.
        *   [ ] How it works (ANDing with IP address to get network address).

**3. Topic: Subnetting & CIDR**

    *   **Subnetting:**
        *   [ ] **Purpose:** Explain why subnetting is done (e.g., manageability, security, reduce broadcast domain size, overcome shortage of network IDs).
        *   [ ] **Process ("Borrowing Host Bits"):** Clearly explain how bits are borrowed from the host portion to create a subnet ID.
        *   [ ] **Calculations (Given an IP block and number of subnets OR hosts per subnet):**
            *   [ ] **New Subnet Mask:** Calculate it (in dotted decimal and CIDR).
            *   [ ] **Number of Subnets Created:** (2^s, where s is subnet bits).
            *   [ ] **Number of Usable Hosts per Subnet:** (2^h - 2, where h is remaining host bits).
            *   [ ] **For each Subnet:**
                *   [ ] **Network Address (Subnet ID):**
                *   [ ] **First Usable Host Address:**
                *   [ ] **Last Usable Host Address:**
                *   [ ] **Broadcast Address for that Subnet:**
        *   [ ] **INTENSIVE PRACTICE:** Work through MANY numerical examples, including May 2024 Q5. Create your own problems.
        *   [ ] **Key Formulas/Steps:** Write down a clear step-by-step guide for yourself to solve subnetting problems.
        *   [ ] **Exam Angle (from pyqs.md):** "What is subnet mask?", Numerical problems are VERY common.

    *   **CIDR (Classless Inter-Domain Routing):**
        *   [ ] **Concept:** Explain how it eliminates the concept of rigid classes (A, B, C).
        *   [ ] **/notation:** Explain what `/x` means (first x bits are the network prefix).
        *   [ ] **Advantages:** Address space efficiency, route summarization (supernetting).
        *   [ ] **Relation to Subnetting:** How subnet masks are represented in CIDR notation.

**4. Topic: ARP, RARP, ICMP & IPv6 Intro**

    *   **ARP (Address Resolution Protocol):**
        *   [ ] **Purpose:** Map IP address to MAC address on a local network.
        *   [ ] **Working Principle:**
            *   [ ] ARP Request (broadcast): "Who has IP address X, tell MAC address Y?"
            *   [ ] ARP Reply (unicast): "I have IP address X, my MAC address is Z."
            *   [ ] ARP Cache: How it's used to store mappings.
        *   [ ] **Scenario:** Explain when ARP is used (e.g., host A wants to send a packet to host B on the same LAN).
        *   [ ] **Exam Angle (from pyqs.md):** "Explain working of ARP," "Short Note."

    *   **RARP (Reverse ARP):**
        *   [ ] **Purpose:** Map MAC address to IP address (less common now, BOOTP/DHCP largely replaced it).
        *   [ ] **Basic Idea:** Client broadcasts its MAC, RARP server replies with IP.
        *   [ ] **Exam Angle (from pyqs.md):** "Explain working of RARP."

    *   **ICMP (Internet Control Message Protocol):**
        *   [ ] **Purpose:** Error reporting and diagnostic functions for IP. (IP itself is unreliable, ICMP provides feedback).
        *   [ ] **Common Message Types (Explain purpose and simple scenario for each):**
            *   [ ] Echo Request/Reply (used by `ping`).
            *   [ ] Destination Unreachable (host, network, port unreachable).
            *   [ ] Time Exceeded (TTL expired).
            *   [ ] Redirect (better route exists).
        *   [ ] **ICMP Frame Format (Briefly, if specified in pyqs.md):** Key fields like Type, Code, Checksum.
        *   [ ] **Encapsulation:** ICMP messages are encapsulated within IP packets.
        *   [ ] **Exam Angle (from pyqs.md):** "Brief note on ICMP using its frame formats," "Purpose," "Common message types," "Short Note."

    *   **IPv4 vs. IPv6:**
        *   [ ] **Key Differences (Create a comparison table):**
            *   [ ] **Address Space:** 32-bit vs. 128-bit (solve address exhaustion).
            *   [ ] **Header:** Simpler, more efficient header in IPv6 (fewer fields, fixed length options).
            *   [ ] **Security:** IPSec support mandatory/integrated in IPv6.
            *   [ ] **Autoconfiguration:** Stateless Address Autoconfiguration (SLAAC) in IPv6.
            *   [ ] **Fragmentation:** Handled by end hosts in IPv6, not routers.
            *   [ ] **Broadcast:** No broadcast in IPv6 (uses multicast).
            *   [ ] **Notation:** Dotted decimal vs. hexadecimal colon notation.
        *   [ ] **Salient Features of IPv6 (List and briefly explain):** Larger address space, simplified header, security, mobility, QoS.
        *   [ ] **Exam Angle (from pyqs.md):** "Comparative study of IPv4 and IPv6," "Differences," "Salient features of IPv6."

**5. Topic: Routing Algorithms**

    *   **Concept:**
        *   [ ] **Goal of Routing:** Find the "best" path for data packets from source to destination.
        *   [ ] **Metrics:** Common metrics used (hop count, bandwidth, delay, cost).
        *   [ ] **Static vs. Dynamic Routing:** Briefly explain the difference.
    *   **Distance Vector (e.g., RIP, Bellman-Ford):**
        *   [ ] **Working Principle:**
            *   [ ] Each router maintains a distance vector (table of distances to destinations).
            *   [ ] Routers periodically share their distance vectors ONLY with directly connected neighbors.
            *   [ ] Routers update their tables based on information from neighbors (Bellman-Ford equation concept: `MyCostToX = CostToNeighbor + NeighborCostToX`).
        *   [ ] **Bellman-Ford Algorithm:**
            *   [ ] Understand the iterative process (relaxing edges).
            *   [ ] **Practice with a small graph example:** Show how tables update step-by-step.
        *   [ ] **Count-to-Infinity Problem:**
            *   [ ] Explain what it is (slow convergence when a link goes down, routing loops).
            *   [ ] Briefly mention solutions (split horizon, poison reverse).
        *   [ ] **Exam Angle (from pyqs.md):** "Working principle," "Bellman-Ford with example," "Count-to-infinity."

    *   **Link State (e.g., OSPF, Dijkstra):**
        *   [ ] **Working Principle:**
            *   [ ] Each router discovers its neighbors and measures cost to them.
            *   [ ] Each router constructs a Link State Packet (LSP) containing this information.
            *   [ ] LSPs are flooded to ALL other routers in the network.
            *   [ ] Each router builds a complete map (topology) of the network.
            *   [ ] Each router uses Dijkstra's algorithm to compute the shortest path to all destinations.
        *   [ ] **Dijkstra's Algorithm:**
            *   [ ] Understand the steps (maintaining set of visited nodes, tentative distances).
            *   [ ] **Practice with a small graph example:** Show steps of the algorithm.
        *   [ ] **Comparison (DV vs LS):** Create a table comparing them (information shared, complexity, convergence speed, robustness, resource usage).
        *   [ ] **Exam Angle (from pyqs.md):** "Working principle," "Dijkstra's with example," "Compare with DV."

    *   **Briefly (Conceptual Understanding):**
        *   [ ] **Hierarchical Routing:** Purpose (scalability, dividing network into regions/AS).
        *   [ ] **Broadcast Routing:** Methods (flooding, spanning tree).
        *   [ ] **Multicast Routing:** Purpose (one-to-many), basic idea (multicast groups, trees).
        *   [ ] **Exam Angle (from pyqs.md):** "Explain Hierarchical, Broadcast, Multicast routing," "Difference between broadcast and multicast."

**6. Review Day 1 & Day 2**

    *   [ ] **Active Recall:**
        *   [ ] Explain the difference between CSMA/CD and CSMA/CA, including BEB and RTS/CTS.
        *   [ ] Take a sample IP address and subnet it for a given number of subnets. Calculate all relevant addresses.
        *   [ ] Verbally explain the steps of Distance Vector and Link State routing using a small mental graph.
    *   [ ] **Identify Weak Spots:** Revisit subnetting calculations and routing algorithm examples if needed.

---

### **Day 3: Transport, Application Layers & Full Revision**

**1. Topic: Transport Layer Intro & UDP**

    *   **Transport Layer Services:**
        *   [ ] **Process-to-Process Delivery:** Explain (using port numbers to deliver to specific applications).
        *   [ ] **Multiplexing/Demultiplexing:**
            *   [ ] Explain multiplexing at sender (gathering data from multiple app processes, adding headers, passing to network layer).
            *   [ ] Explain demultiplexing at receiver (delivering segments to correct app process based on port numbers).
        *   [ ] **(Optional) Reliability:** Provided by TCP, not UDP.
        *   [ ] **(Optional) Flow/Congestion Control:** Provided by TCP, not UDP.
        *   [ ] **Exam Angle (from pyqs.md):** "Explain transport layer multiplexing and demultiplexing," "QoS parameters (reliability, delay, jitter, bandwidth)."

    *   **UDP (User Datagram Protocol):**
        *   [ ] **Characteristics:**
            *   [ ] Connectionless (no handshake).
            *   [ ] Unreliable ("best effort" delivery, no ACKs, no retransmissions by UDP itself).
            *   [ ] Fast, low overhead.
            *   [ ] No flow control, no congestion control.
            *   [ ] Message-oriented.
        *   [ ] **UDP Header Format:**
            *   [ ] **Draw:** `Source Port | Destination Port | Length | Checksum`.
            *   [ ] **Explain Purpose of Each Field:**
                *   Source Port (optional, for reply).
                *   Destination Port (identifies application process on dest host).
                *   Length (length of UDP header + data in bytes).
                *   Checksum (optional in IPv4, mandatory in IPv6; for error detection in header & data).
        *   [ ] **Common UDP Applications:** DNS, DHCP, SNMP, TFTP, streaming media, online gaming. Explain *why* UDP is suitable for these.
        *   [ ] **Exam Angle (from pyqs.md):** "Draw and explain header format," "Characteristics," "Short Note."

**2. Topic: TCP (Transmission Control Protocol)**

    *   **TCP Characteristics:**
        *   [ ] Connection-oriented (requires handshake).
        *   [ ] Reliable (uses ACKs, sequence numbers, retransmissions).
        *   [ ] Byte-stream service (application data seen as a continuous stream of bytes).
        *   [ ] Full-duplex.
        *   [ ] Provides flow control and congestion control.
    *   **TCP Header Format:**
        *   [ ] **Draw:** A simplified version showing key fields. (Full header is complex, focus on what's usually asked).
        *   [ ] **Label and Explain Purpose of Key Fields:**
            *   [ ] Source Port, Destination Port.
            *   [ ] Sequence Number (byte stream number of first byte in segment).
            *   [ ] Acknowledgement Number (next sequence number expected from other side).
            *   [ ] Header Length.
            *   [ ] **Flags (Control Bits - VERY IMPORTANT):**
                *   [ ] `SYN` (Synchronize - connection request).
                *   [ ] `ACK` (Acknowledgement is valid).
                *   [ ] `FIN` (Finish - terminate connection).
                *   [ ] `RST` (Reset - abort connection).
                *   [ ] `PSH` (Push - send data immediately).
                *   [ ] `URG` (Urgent pointer field is valid).
            *   [ ] Window Size (for flow control - how many bytes receiver is willing to accept).
            *   [ ] Checksum (error detection for header & data).
            *   [ ] Urgent Pointer (if URG flag is set).
        *   [ ] **Exam Angle (from pyqs.md):** "Draw and explain header," "Compare with UDP header," "List fields in TCP not in UDP and why."

    *   **TCP Connection Establishment (3-way handshake):**
        *   [ ] **Diagram:** Show the three segments exchanged.
        *   [ ] **Flag Sequence:**
            *   [ ] Client -> Server: `SYN` (Client Seq=x)
            *   [ ] Server -> Client: `SYN` + `ACK` (Server Seq=y, Ack=x+1)
            *   [ ] Client -> Server: `ACK` (Client Seq=x+1, Ack=y+1)
        *   [ ] Explain purpose of each step (synchronize sequence numbers).
        *   [ ] **Exam Angle (from pyqs.md):** "Explain connection establishment," "Diagram."

    *   **TCP Connection Release (4-way or 3-way with piggybacking):**
        *   [ ] **Diagram (4-way):** Show the segments.
        *   [ ] **Flag Sequence (4-way):**
            *   [ ] Side 1 -> Side 2: `FIN` (Seq=x)
            *   [ ] Side 2 -> Side 1: `ACK` (Ack=x+1)
            *   [ ] Side 2 -> Side 1: `FIN` (Seq=y)
            *   [ ] Side 1 -> Side 2: `ACK` (Ack=y+1)
        *   [ ] Explain purpose (each side closes its end independently).
        *   [ ] Briefly mention 3-way with piggybacked FIN+ACK.
        *   [ ] **Exam Angle (from pyqs.md):** "Explain connection release," "Diagram."

    *   **TCP Flow Control (Sliding Window):**
        *   [ ] **Concept:** Prevent sender from overwhelming receiver.
        *   [ ] **How Window Size is Advertised:** Receiver includes its current `Window Size` (buffer space) in TCP header.
        *   [ ] **Sender Action:** Sender limits amount of unacknowledged data to receiver's advertised window size.
        *   [ ] **Zero Window:** What happens if receiver advertises window of 0.
        *   [ ] **Exam Angle (from pyqs.md):** "Discuss TCP flow control."

    *   **TCP Congestion Control:**
        *   [ ] **Purpose:** Prevent TCP from overwhelming the network itself.
        *   [ ] **Mechanisms (Conceptual Understanding):**
            *   [ ] **AIMD (Additive Increase, Multiplicative Decrease):**
                *   [ ] Additive Increase: Increase congestion window (cwnd) slowly when no congestion.
                *   [ ] Multiplicative Decrease: Decrease cwnd sharply when congestion detected (e.g., packet loss).
            *   [ ] **Slow Start:** Start with small cwnd, increase exponentially until threshold or loss.
            *   [ ] **Congestion Avoidance:** After Slow Start threshold, increase linearly (AIMD's additive part).
            *   [ ] **Fast Retransmit:** If 3 duplicate ACKs received, assume packet loss and retransmit without waiting for timeout.
            *   [ ] **Fast Recovery:** After Fast Retransmit, skip Slow Start and go to Congestion Avoidance.
        *   [ ] **Congestion Detection:** How TCP infers congestion (timeouts, duplicate ACKs).
        *   [ ] **Exam Angle (from pyqs.md):** "Discuss TCP congestion control," "Short Note on TCP Congestion Control," "AIMD, Slow Start."
        *   [ ] **TCP Throughput Problem (May 2024 Q6):** Understand how window size, RTT, and channel capacity affect throughput. Practice this problem.

**3. Topic: Application Layer Protocols**

    *   **DNS (Domain Name System):**
        *   [ ] **Purpose:** Translate human-readable hostnames (e.g., www.google.com) to IP addresses, and vice-versa.
        *   [ ] **Hierarchical Structure:** Explain (root, TLD, authoritative servers). Draw a simple hierarchy.
        *   [ ] **Resolver:** Role of client-side resolver.
        *   [ ] **Iterative vs. Recursive Queries (Concept & Diagram):**
            *   [ ] **Recursive:** Resolver asks server, server finds answer or asks others.
            *   [ ] **Iterative:** Resolver asks server, server refers to another server, resolver asks that one.
        *   [ ] **Record Types (Briefly explain purpose of each):**
            *   [ ] `A` (IPv4 address).
            *   [ ] `AAAA` (IPv6 address).
            *   [ ] `CNAME` (Canonical Name - alias).
            *   [ ] `MX` (Mail Exchange server).
            *   [ ] `NS` (Name Server).
        *   [ ] **Caching:** How DNS responses are cached.
        *   [ ] **Exam Angle (from pyqs.md):** "Explain services provided by DNS," "Short Note."

    *   **HTTP (HyperText Transfer Protocol):**
        *   [ ] **Purpose:** Protocol for fetching resources like HTML documents (foundation of data communication for WWW).
        *   [ ] **Client-Server Model:** Explain.
        *   [ ] **Persistent vs. Non-persistent Connections:**
            *   [ ] **Non-persistent:** One TCP connection per object. Explain overhead.
            *   [ ] **Persistent:** Multiple objects over one TCP connection (with/without pipelining). Explain benefits.
        *   [ ] **Request/Response Message Format (General Structure):**
            *   [ ] **Request Message:** Start Line (Method, URL, Version), Headers (Host, User-Agent, Accept, etc.), Body (optional).
            *   [ ] **Response Message:** Status Line (Version, Status Code, Status Phrase), Headers (Content-Type, Content-Length, Server, etc.), Body (requested resource).
        *   [ ] **Common HTTP Methods:** GET, POST, HEAD, PUT, DELETE (brief purpose).
        *   [ ] **Common Status Codes:** 200 OK, 301 Moved Permanently, 400 Bad Request, 404 Not Found, 500 Internal Server Error (brief meaning).
        *   [ ] **Exam Angle (from pyqs.md):** "Request/Response message format," "Persistent vs. Non-persistent," "Short Note."

    *   **SMTP (Simple Mail Transfer Protocol):**
        *   [ ] **Purpose:** Protocol for sending (pushing) email messages between mail servers and from MUA to MTA.
        *   [ ] **Basic Commands (Conceptual Understanding of interaction):**
            *   [ ] `HELO`/`EHLO` (Identify client).
            *   [ ] `MAIL FROM:` (Sender's email).
            *   [ ] `RCPT TO:` (Recipient's email).
            *   [ ] `DATA` (Start of message body).
            *   [ ] `QUIT`.
        *   [ ] **TCP Port:** Uses TCP port 25.
        *   [ ] **Not for retrieving mail** (POP3/IMAP do that).
        *   [ ] **Exam Angle (from pyqs.md):** "Basic commands," "Purpose," "Short Note."

    *   **SNMP (Simple Network Management Protocol) (Briefly):**
        *   [ ] **Purpose:** Monitoring and managing network devices (routers, switches, servers).
        *   [ ] **Components:**
            *   [ ] **Manager:** (NMS - Network Management Station) sends requests, receives traps.
            *   [ ] **Agent:** Runs on managed devices, responds to manager, sends traps.
            *   [ ] **MIB (Management Information Base):** Database of manageable objects on the agent.
        *   [ ] **Basic Operations:** GET, SET, TRAP.
        *   [ ] **Exam Angle (from pyqs.md):** "Explain SNMP and its functions."

    *   **Electronic Mail (General Components):**
        *   [ ] **MUA (Mail User Agent):** Client software (e.g., Outlook, Gmail web interface).
        *   [ ] **MTA (Mail Transfer Agent):** Mail server software (e.g., Postfix, Sendmail) - uses SMTP.
        *   [ ] **MDA (Mail Delivery Agent):** Delivers email to recipient's mailbox.
        *   [ ] Briefly explain the flow of an email using these components.
        *   [ ] **Exam Angle (from pyqs.md):** "What is Electronic Mailing? Components."

**4. Topic: Miscellaneous & Short Note Topics**

    *   **Finite State Machines (FSM):**
        *   [ ] **What they are:** A model of computation with states, transitions, inputs, and outputs.
        *   [ ] **How they model protocols:** Show a very simple example (e.g., Stop-and-Wait ARQ with states like "Ready to Send," "Waiting for ACK").
        *   [ ] **Benefits:** Precise specification, aids in implementation and verification.
        *   [ ] **Exam Angle (from pyqs.md):** "What is FSM model? How used in protocols? Explain."

    *   **Fragmentation & Reassembly:**
        *   [ ] **Why needed:** When a large packet from an upper layer needs to be sent over a network whose MTU (Maximum Transmission Unit) is smaller.
        *   [ ] **How it works (IP Fragmentation):**
            *   [ ] **IP Header Fields Involved:**
                *   Identification (ID): To identify fragments belonging to the same original datagram.
                *   Flags (DF - Don't Fragment, MF - More Fragments).
                *   Fragment Offset: Position of this fragment in the original datagram.
            *   [ ] Explain how routers fragment and how the destination host reassembles.
        *   [ ] **Example:** Show a large packet being broken into smaller fragments with correct ID, MF, and Offset values.
        *   [ ] **Exam Angle (from pyqs.md):** "Explain Fragmentation and reassembly using suitable example," "Short Note."

    *   **Queuing Theory (M/M/1, Little's Theorem - Very Basic):**
        *   [ ] **M/M/1:** Briefly explain what it represents (Poisson arrivals, Exponential service times, 1 server).
        *   [ ] **Little's Theorem:** `L = λW` (Average number of items in system = Arrival rate * Average time in system). Explain terms. Simple conceptual application.
        *   [ ] **Focus:** Conceptual understanding, not deep math unless specifically preparing for older CBGS type questions.
        *   [ ] **Exam Angle (from pyqs.md):** "Explain Little's theorem," "What is M/M/1?"

    *   **IEEE 802 series (General Awareness):**
        *   [ ] Know it's a family of standards for LANs/MANs.
        *   [ ] Mention a few key ones and what they cover:
            *   802.3 (Ethernet - CSMA/CD).
            *   802.11 (Wireless LAN - WiFi - CSMA/CA).
            *   802.15 (Wireless PAN - Bluetooth).
            *   802.16 (Broadband Wireless Access - WiMAX).
        *   [ ] **Exam Angle (from pyqs.md):** "Short Note on IEEE Standards 802 series."

    *   **FDDI (Fiber Distributed Data Interface) (Briefly for short note):**
        *   [ ] Token-passing, dual-ring LAN using fiber optic cable. High speed (for its time).
    *   **Service Primitives (Short note):**
        *   [ ] Define: Operations available to a layer from the layer below it (e.g., Request, Indication, Response, Confirm). Simple example of how they are used in layer interaction.
    *   **Frame Relay (Short Note):**
        *   [ ] WAN protocol, uses virtual circuits (PVCs, SVCs), connection-oriented at DLL.

**5. MEGA REVISION & Practice**

    *   [ ] Go through your prepared detailed answers for ALL topics.
    *   [ ] Use active recall: Cover your notes and try to explain the concept aloud or write down key points.
    *   [ ] **Prioritize:** Focus on the "Key Focus Areas" identified in `pyqs.md` analysis and your personal weak spots.
    *   [ ] **Rework Numerical Problems:** Subnetting, Aloha throughput, Shannon, TCP throughput. Ensure you can solve them quickly and accurately.
    *   [ ] **Draw Key Diagrams from Memory:** OSI/TCP-IP, TCP/UDP Headers, Sliding Window timelines, 3-way/4-way handshakes.
    *   [ ] **Practice Short Note Questions:** For common short note topics, jot down 3-4 key points for each.
    *   [ ] **Simulate Exam Conditions:** Try answering a few full questions from past papers within a time limit.

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
