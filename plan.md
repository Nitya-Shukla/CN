**Core Principles for this 3-Day Plan:**

*   **Active Learning:** Don't just read. Explain concepts aloud, draw diagrams, try to solve problems before looking at solutions.
*   **Prioritize:** Focus on topics that appear most frequently and form the foundation of others.
*   **Time Blocking:** Allocate specific time slots for topics. Be disciplined.
*   **Breaks:** Short breaks every 1-1.5 hours are crucial to maintain focus.
*   **Sleep:** Don't sacrifice sleep, especially the night before the exam.
*   **Hydration & Nutrition:** Keep your brain fueled.

---

**Day 1: Foundations, Data Link Layer & Intro to MAC**

**(Approx. 8-10 hours of focused study + breaks)**

**Morning Session (Approx. 4 hours)**

1.  **Topic: Network Models & Fundamentals (2 hours)**
    *   **OSI vs. TCP/IP:**
        *   Draw both models side-by-side.
        *   List layers and their primary functions (1-2 key functions per layer).
        *   Key differences (e.g., number of layers, when protocols were defined).
        *   Protocols at each layer (focus on common ones like IP, TCP, UDP, HTTP, Ethernet).
    *   **Connection-Oriented vs. Connectionless Services (30 mins)**
        *   Definitions, examples (TCP vs. UDP).
        *   Key characteristics (reliability, setup, overhead).
    *   **Network Topologies & Media (1 hour)**
        *   Bus, Star, Ring, Mesh, Hybrid: Sketch, 1-2 Adv/Disadv for each.
        *   Guided (Twisted Pair, Coax, Fiber) vs. Unguided (Radio, Microwave, Infrared): Basic properties, use cases.
    *   **Data Rate & Bandwidth (30 mins)**
        *   Definitions.
        *   Shannon-Hartley Theorem: Understand the formula (C = B log2(1 + S/N)). Attempt the numerical problem from Dec 2024 (Q3a).

2.  **Topic: Data Link Layer (DLL) - Services & Error/Flow Control Concepts (2 hours)**
    *   **DLL Services:** Framing, Error Control, Flow Control, Access Control.
    *   **Error Detection & Correction Mechanisms:**
        *   Parity (Simple, 2D): Concept, limitations.
        *   Checksum: Concept.
        *   CRC (Cyclic Redundancy Check): Conceptual understanding of how it works (polynomial division idea), not deep math.
    *   **Flow Control Mechanisms (Conceptual):**
        *   Stop-and-Wait: Basic idea.
        *   Sliding Window: Concept, need for sequence numbers, ACKs.

**Afternoon Session (Approx. 4 hours)**

3.  **Topic: DLL Protocols (Sliding Window Protocols) (2.5 hours)**
    *   **Stop-and-Wait ARQ:**
        *   Working principle, diagram (timeline for frame, ACK, timeout).
        *   Efficiency (simple concept).
    *   **Go-Back-N ARQ:**
        *   Working principle, sender/receiver window size, cumulative ACKs.
        *   Diagram scenarios: Frame loss, ACK loss.
    *   **Selective Repeat ARQ:**
        *   Working principle, sender/receiver window size, individual ACKs, buffering.
        *   Diagram scenarios: Frame loss.
        *   Compare GBN and SR (efficiency, complexity, buffering).
    *   *(Practice drawing timelines for each for successful transmission, lost frame, lost ACK).*

4.  **Topic: Framing & HDLC (1.5 hours)**
    *   **Framing Methods:** Character count, Character stuffing, Bit stuffing, Physical layer coding violations. Focus on Bit Stuffing (understand the mechanism, practice the problem from May 2018 CBGS/GS).
    *   **HDLC:** Frame format (Flag, Address, Control, Information, FCS). Briefly, what each field is for. (Don't get bogged down in control field subtypes unless you have extra time).

**Evening Session (Approx. 2 hours)**

5.  **Topic: Introduction to MAC Sublayer & ALOHA (1 hour)**
    *   Function of MAC layer.
    *   **ALOHA Protocols:**
        *   Pure ALOHA: Principle, vulnerable period, max throughput (18.4%).
        *   Slotted ALOHA: Principle, vulnerable period, max throughput (36.8%).
        *   Practice the numerical problems (May 2023 Q4b, May 2022 Q2b, May 2018 CBGS Q5b).
6.  **Review Day 1 (1 hour)**
    *   Quickly skim through all topics covered.
    *   Redraw OSI/TCP-IP models from memory.
    *   Explain GBN vs SR to yourself.

---

**Day 2: MAC cont., Network Layer**

**(Approx. 8-10 hours of focused study + breaks)**

**Morning Session (Approx. 4 hours)**

1.  **Topic: CSMA Protocols & Collision-Free (2 hours)**
    *   **CSMA (Carrier Sense Multiple Access):**
        *   Persistent (1-persistent, p-persistent), Non-persistent: Working principle, pros/cons.
    *   **CSMA/CD (Collision Detection):**
        *   Working principle (sense, transmit, detect, jam, backoff).
        *   Binary Exponential Backoff (BEB): How it works, example.
    *   **CSMA/CA (Collision Avoidance):**
        *   Working principle (RTS/CTS concept for hidden station).
        *   Where it's used (WiFi).
    *   **Collision-Free Protocols (Briefly - 30 mins):**
        *   Binary Countdown: Concept.
        *   Adaptive Tree Walk: Concept. (Don't spend too much time unless you feel strong elsewhere).

2.  **Topic: IP Addressing (Classes & Basics) (2 hours)**
    *   IPv4 Address structure (32-bit, dotted decimal).
    *   Classes (A, B, C, D, E): Range, Network/Host ID bits, Default Mask, Number of Networks/Hosts.
        *   *Practice identifying class and default mask from an IP address.*
    *   Special Addresses: Loopback, Private IPs, Broadcast.
    *   Subnet Mask concept.

**Afternoon Session (Approx. 4 hours)**

3.  **Topic: Subnetting & CIDR (2 hours)**
    *   **Subnetting:**
        *   Purpose (borrowing host bits for subnet ID).
        *   Calculating: New subnet mask, number of subnets, number of hosts per subnet.
        *   Finding: Network address, first usable host, last usable host, broadcast address for a given IP and subnet mask/subnet bits.
        *   **PRACTICE NUMERICALS INTENSIVELY** (e.g., May 2024 Q5).
    *   **CIDR (Classless Inter-Domain Routing):** /notation (e.g., /24).

4.  **Topic: ARP, RARP, ICMP & IPv6 Intro (2 hours)**
    *   **ARP (Address Resolution Protocol):**
        *   Purpose (IP to MAC), working principle (broadcast request, unicast reply), ARP cache.
    *   **RARP (Reverse ARP):** Purpose (MAC to IP), basic idea.
    *   **ICMP (Internet Control Message Protocol):**
        *   Purpose (error reporting, diagnostics - e.g., ping, destination unreachable).
        *   Common message types (Echo request/reply, Dest Unreachable). Frame format (briefly, if time).
    *   **IPv4 vs. IPv6:** Key differences (address space, header simplicity, security, autoconfiguration). Salient features of IPv6.

**Evening Session (Approx. 2-3 hours)**

5.  **Topic: Routing Algorithms (2 hours)**
    *   **Concept:** Goal of routing, metrics, static vs. dynamic.
    *   **Distance Vector (e.g., RIP, Bellman-Ford):**
        *   Working principle (sharing distance vectors with neighbors).
        *   Bellman-Ford algorithm: Understand the iterative process. Practice with a small graph example.
        *   Count-to-infinity problem (brief concept).
    *   **Link State (e.g., OSPF, Dijkstra):**
        *   Working principle (LSPs, building full map, Dijkstra).
        *   Dijkstra's algorithm: Understand the steps. Practice with a small graph example.
    *   **Briefly:** Hierarchical, Broadcast, Multicast routing concepts.
6.  **Review Day 1 & Day 2 (1 hour)**
    *   Key concepts from DLL and MAC.
    *   Subnetting rules.
    *   DV vs LS routing.

---

**Day 3: Transport, Application Layers & Full Revision**

**(Approx. 8-10 hours of focused study + breaks)**

**Morning Session (Approx. 4 hours)**

1.  **Topic: Transport Layer Intro & UDP (1.5 hours)**
    *   Transport Layer Services: Process-to-process delivery, multiplexing/demultiplexing, (optional) reliability, flow/congestion control.
    *   **UDP (User Datagram Protocol):**
        *   Characteristics (connectionless, unreliable, fast, low overhead).
        *   UDP Header Format: Draw and label fields (Source Port, Dest Port, Length, Checksum). Explain purpose of each.
        *   Common UDP applications.

2.  **Topic: TCP (Transmission Control Protocol) (2.5 hours)**
    *   **TCP Characteristics:** Connection-oriented, reliable, byte-stream.
    *   **TCP Header Format:** Draw and label key fields (Source/Dest Port, Seq Num, Ack Num, Flags (SYN, ACK, FIN, RST, PSH, URG), Window Size, Checksum, Urgent Pointer). Explain purpose of key fields.
    *   **TCP Connection Establishment (3-way handshake):** Diagram, flag sequence.
    *   **TCP Connection Release (4-way or 3-way with piggybacking):** Diagram, flag sequence.
    *   **TCP Flow Control (Sliding Window):** How window size is advertised.
    *   **TCP Congestion Control:**
        *   AIMD (Additive Increase, Multiplicative Decrease).
        *   Slow Start, Congestion Avoidance, Fast Retransmit/Fast Recovery (conceptual understanding).
        *   (If time) TCP throughput problem (May 2024 Q6).

**Afternoon Session (Approx. 3 hours)**

3.  **Topic: Application Layer Protocols (2 hours)**
    *   **DNS (Domain Name System):**
        *   Purpose (hostname to IP mapping).
        *   Hierarchical structure, resolver, iterative/recursive queries (concept).
        *   Record types (A, AAAA, CNAME, MX, NS - brief).
    *   **HTTP (HyperText Transfer Protocol):**
        *   Purpose, client-server model.
        *   Persistent vs. Non-persistent.
        *   Request/Response message format (general structure, common headers like Host, User-Agent, Content-Type, Status codes like 200, 404, 500).
    *   **SMTP (Simple Mail Transfer Protocol):** Purpose, basic commands (HELO, MAIL FROM, RCPT TO, DATA - conceptual).
    *   **SNMP (Simple Network Management Protocol) (Briefly - 30 mins):** Purpose, manager/agent, MIB concept.
    *   **Electronic Mail (General):** Components (MUA, MTA, MDA).

4.  **Topic: Miscellaneous & Short Note Topics (1 hour)**
    *   Finite State Machines (FSM): What they are, how they can model protocols (simple example).
    *   Fragmentation & Reassembly: Why needed, how it works (IP header fields involved - ID, flags, offset).
    *   Queuing Theory (M/M/1, Little's Theorem): Very basic concept if you see it in short notes often. Don't go deep.
    *   IEEE 802 series (general awareness), FDDI (briefly, if short note).
    *   Review other short note topics from the list (e.g., Service Primitives, Frame Relay).

**Evening Session (Remaining Time - Critical)**

5.  **MEGA REVISION & Practice (2-3+ hours)**
    *   **Go through the ENTIRE deduplicated list of questions.** For each, mentally (or by jotting down key points) outline how you would answer it.
    *   **Prioritize areas you feel weakest in.**
    *   **Rework numerical problems** (Subnetting, Aloha throughput, Shannon, TCP throughput).
    *   **Draw key diagrams from memory:** OSI/TCP-IP, TCP/UDP Headers, Sliding Window timelines.
    *   **Practice Short Note Questions:** Write 2-3 key points for each common short note topic.
    *   Look at the structure of the actual question papers: how many choices, marks per question.

---

**General Tips for Success:**

*   **Previous Papers:** Use them as a guide for question types and important topics. After studying a topic, try to answer related questions from the papers.
*   **Explain to Yourself:** If you can explain a concept simply, you understand it.
*   **Don't Panic:** If a topic seems too complex, get the main idea and move on if time is short. You can't master everything in 3 days. Aim for broad understanding and depth in core areas.
*   **Exam Strategy:** On exam day, read all questions first. Start with those you are most confident about. Manage your time.