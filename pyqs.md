**Analysis of Patterns:**

1.  **Core Topics are Consistent:**
    *   **OSI and TCP/IP Models:** Comparison, functions of layers, protocols at each layer. This is almost guaranteed.
    *   **Data Link Layer:**
        *   Error control and Flow control mechanisms (general concepts).
        *   Specific protocols: Stop-and-Wait, Go-Back-N, Selective Repeat (working, diagrams, comparison).
        *   Framing, Bit/Byte stuffing.
    *   **MAC Sublayer:**
        *   ALOHA (Pure, Slotted) and CSMA (Persistent, Non-persistent, CSMA/CD, CSMA/CA) are extremely common. Expect working principles, comparisons, and sometimes simple throughput calculations for Aloha.
        *   Collision-free protocols (Binary Countdown, Adaptive Tree Walk) appear occasionally.
        *   IEEE 802.x standards (Token Bus, general mention of 802 series, 802.11 vs 802.16).
    *   **Network Layer:**
        *   IP Addressing: Classes, Subnetting (numerical problems are common), Network ID, Host ID.
        *   IPv4 vs. IPv6 comparison.
        *   Routing Algorithms: Distance Vector (RIP), Link State (OSPF concept, though Dijkstra is more common directly), Bellman-Ford, Dijkstra's algorithm (expect examples). Hierarchical, Broadcast, Multicast routing concepts.
        *   ARP/RARP.
        *   ICMP (purpose, frame format).
        *   Fragmentation and Reassembly.
        *   Congestion Control (general principles, leaky bucket, choke packets).
    *   **Transport Layer:**
        *   TCP vs. UDP (comparison, header formats, when to use which).
        *   TCP: Connection establishment/release, Flow control, Congestion control (BEB appears here too, but also in MAC).
        *   UDP: Header format.
        *   Multiplexing/Demultiplexing.
        *   Quality of Service (QoS) parameters.
    *   **Application Layer:**
        *   HTTP (request/response, general).
        *   SMTP.
        *   DNS.
        *   SNMP (less frequent but appears).
        *   Electronic Mail (general concept).
    *   **General Concepts:**
        *   Connection-oriented vs. Connectionless services.
        *   Network Topologies (advantages/disadvantages).
        *   Guided/Unguided Media.
        *   Finite State Machines (FSM) in protocol design.
        *   Queuing Theory (M/M/1, Little's Theorem) – appears in older CBGS papers more.

2.  **Question Types:**
    *   **"Explain/Describe":** The most common type, asking for working principles, often with examples or diagrams.
    *   **"Compare/Differentiate":** OSI vs. TCP/IP, TCP vs. UDP, IPv4 vs. IPv6, different MAC protocols, etc.
    *   **"Short Notes":** A list of topics, usually have to pick 2 or 3. These are good for breadth.
    *   **Numerical Problems:** Subnetting, Shannon's capacity/data rate, Aloha throughput, TCP window throughput/efficiency, Token Ring scan time.
    *   **"Draw and Explain":** Header formats (TCP, UDP, IP, ICMP, HDLC), OSI model layers.

3.  **Variations between GS (Grading System) and CBGS (Choice Based Grading System):**
    *   CBGS papers (older ones like 2018-2020) seem to have a slightly higher emphasis on theoretical/mathematical aspects like Queuing Theory (M/M/1, Little's Theorem), Petri Nets.
    *   GS papers (more recent) are very focused on the practical aspects of protocols and their mechanisms.
    *   However, the core networking topics are largely the same.

4.  **Recurring "Short Note" Topics:**
    *   TCP Congestion Control
    *   Aloha (and Slotted Aloha)
    *   ARP
    *   CSMA/CD or CSMA/CA
    *   DNS
    *   HTTP
    *   ICMP
    *   UDP (often packet format)
    *   SMTP
    *   Fragmentation and Reassembly
    *   FSM

5.  **"MLMA" (Dec 2024 Q8d):** This is an outlier. It's likely a typo or a very specific term not commonly found in standard textbooks. Given it only appears once, it's low priority unless your instructor emphasized it. It might have been intended as "MAC Layer" or something similar.

---

**Deduplicated and Combined Question List (Organized by Topic):**

**I. Network Models & Fundamentals**

1.  **OSI and TCP/IP Models:**
    *   Give the comparative analysis of OSI and TCP/IP Model. (Dec 2024)
    *   Draw and name all seven layers of ISO-OSI reference model. Explain the function of each layer and name two protocols of each layer. (May 2024)
    *   Explain the working principle of ISO-OSI reference model with functionality of each layer. (May 2023)
    *   Explain the OSI model stating its importance in network architectures. (May 2022)
    *   Discuss issues and its functionality of OSI models. (Dec 2020, June 2020)
    *   What are the advantages of Internet TCP/IP model over OSI model? (May 2018 GS)
    *   Explain the principles that were applied at the time of seven layer architecture design of ISO-OSI reference model. (May 2018 CBGS)
    *   State the major functions performed by the presentation layer of the ISO OSI model. (May 2019)
2.  **Connection-Oriented vs. Connectionless Services:**
    *   Give the difference between connection-oriented and connectionless services. (Dec 2024)
    *   What is the principal difference between connectionless and connection-oriented communication? (May 2022)
    *   (Short Note) Connection Oriented (May 2024)
    *   Two networks each provide reliable connection-oriented service. One offers a reliable byte stream and the other offers a reliable message stream. Are these identical? If so, why is distinction made? If not, give an example of how they are different? (May 2018 CBGS)
3.  **Network Topologies:**
    *   Give explanation of different type of topologies in terms of its advantages and disadvantages. (May 2024)
    *   Give Ring and BUS Topology? Write its advantages and disadvantages. (Dec 2020, June 2020)
4.  **Computer Network Basics:**
    *   What is Computer Network? Discuss the importance of computer network. Give its merits and demerits. (May 2023)
    *   Explain Network Architecture in detail. (May 2019)
    *   Write the goals of computer networking with its classification. (May 2018 CBGS)
5.  **Transmission Media:**
    *   Discuss about guided and unguided transmission media with suitable sketch. (May 2023)
6.  **Data Rate and Bandwidth:**
    *   Define the term data rate, bandwidth with taking a suitable example. (May 2024)
    *   (Problem) We measure the performance of a line (4 kHz of Bandwidth) when the signal is 20V, the noise is 6mV. Calculate the Maximum data rate supported by this line. (Dec 2024 - *Hint: Shannon-Hartley Theorem*)
7.  **Service Primitives:**
    *   (Short Note) Service Primitives (May 2023)

**II. Data Link Layer (DLL)**

1.  **DLL Services & Functions:**
    *   What is the data link layer and what are services does it provides? (May 2023)
    *   Explain data link layer address with examples. (May 2022)
2.  **Error Control & Flow Control:**
    *   Explain in detail about the error and flow control mechanisms employed at data link layer. (Dec 2024)
    *   What is the mechanism of sliding window flow control? Explain with an example. (May 2023, May 2018 CBGS)
    *   Describe the stop and wait flow control technique. (May 2022)
    *   Explain different flow control mechanisms used in brief. (May 2019)
    *   Explain briefly: ii) Error control (Dec 2020)
    *   Explain different error detection and correction mechanisms with examples. (May 2019)
    *   Write the different types of ARQ techniques. (May 2018 CBGS)
3.  **DLL Protocols (Sliding Window Protocols):**
    *   Explain the working principle of Go-Back-N and Selective repeat protocols. (Dec 2024)
    *   With the help of suitable diagrams explain: Stop and wait, Go back N, Selective repeat. (May 2024)
    *   Describe Go Back N and Selective Repeat protocol. (May 2022)
    *   Describe the working of a selective repeat protocol. (Dec 2020)
    *   Explain sliding window protocol in details. (June 2020)
    *   Compare the performance of stop and wait protocol and sliding window protocol. (Dec 2020)
4.  **Framing & Stuffing:**
    *   What is bit and byte stuffing? Explain with example. (May 2022)
    *   Explain briefly: i) Framing (Dec 2020)
    *   What is framing? Explain different types of framing protocols with their format. (May 2019)
    *   (Problem) If the start and end header is `100001` and the data stream `100110001100001110000011` is to be stuffed. What will be the frame after bit stuffing? (May 2018 CBGS, May 2018 GS - slightly different stream)
5.  **HDLC/SDLC:**
    *   Draw the frame format of HDLC protocol. Explain the use of control, data checksum, and address fields of HDLC protocol. (May 2018 GS)
    *   Compare and contrast SDLC with HDLC? (May 2018 CBGS)
6.  **(Problem) Protocol Interaction & Channel Capacity:**
    *   Three stations A, B, C. A-B (3000km, T1 trunk, Go-Back-N), B-C (1000km, Stop-and-Wait, short ACK). Frame size 64 byte, propagation 6 μsec/Km. What channel capacity B-C so B doesn't overflow? (May 2018 GS)

**III. Medium Access Control (MAC) Sublayer**

1.  **MAC Layer Functions:**
    *   What is the function of MAC layer? (May 2024)
2.  **ALOHA Protocols:**
    *   Explain working principle of slotted ALOHA with suitable sketch. (May 2023)
    *   Explain: Pure and slotted ALOHA (May 2024)
    *   Explain: ALOHA (June 2020)
    *   (Short Note) Aloha and Slotted Aloha (Dec 2024)
    *   (Problem) A group of N stations share a 56 Kbps pure ALOHA channel. Each station outputs a 1000 bits frame on an average of once every 100 sec. What is the max value of N? (May 2023)
    *   (Problem) A radio station is using 14.44 kbps channel for message transmission and the sending message packets are 100 bits long. Calculate the maximum throughput for the channel using slotted ALOHA. (May 2022)
    *   (Problem) A large population of ALOHA users generate 50 requests/sec. Time is slotted in units of 40 msec. i) Chance of success in first attempt? ii) Probability of exactly k collisions then success? (May 2018 CBGS)
    *   Compare pure ALOHA, slotted ALOHA and CSMA/CD. (May 2018 CBGS)
3.  **CSMA Protocols:**
    *   Explain Binary Exponential Back-off (BEB) with suitable example. (Dec 2024)
    *   Explain briefly about the Persistent and Non-persistent CSMA protocols. (Dec 2024)
    *   Explain: CSMA (May 2024)
    *   Explain following term: i) CSMA/CD, ii) CSMA/CA (June 2020)
    *   (Short Note) CSMA/CA (May 2024)
    *   (Short Note) CSMA/CD (May 2023)
    *   Explain CSMA and protocols with Collision detection and Avoidance. (May 2019)
4.  **Collision-Free Protocols:**
    *   What do you understand by collision free protocol? Explain the Binary Count Down Protocol with suitable example. (Dec 2024)
    *   What is Limited-Contention Protocols? Explain the working principle of Adaptive Tree Walk Protocol with suitable example. (May 2023)
    *   (Problem) Sixteen stations contending using adaptive tree walk. If all stations whose addresses are prime numbers become ready, how many bit slots to resolve contention? (May 2018 GS)
5.  **IEEE 802 Standards:**
    *   (Short Note) IEEE Standards 802 series (May 2023)
    *   Explain the frame format of IEEE 802.4 (token bus) protocol. (May 2018 CBGS)
    *   Write a comparison between 802.11 and 802.16. (May 2018 GS)
    *   Explain the functioning of wireless LAN in detail. (May 2019)
6.  **Token Ring Problem:**
    *   In a token ring LAN, 256 stations, distance 10m. Data rate 1 Mbps, velocity 2.5×10⁵ Km/sec. 116 stations off, 0 stations active (assume this means remaining are active), each active holds token for 10 msec. Determine current scan time and channel efficiency. (May 2018 GS - *check wording carefully, "0 stations active" and "each active station" is contradictory. Assume it means some are active*)

**IV. Network Layer**

1.  **IP Addressing & Subnetting:**
    *   Explain various classes of IP addresses (network-ID bits, host-ID bits, possible hosts, possible networks, netgate). (May 2024)
    *   (Problem) An organization is granted block 211.17.180.0/24. Administrator wants 32 subnets. Find: a) subnet mask, b) addresses per subnet, c) first/last address in first subnet, d) first/last address in last subnet. (May 2024)
    *   What is subnet mask? Explain different classes of IP address. (May 2022)
    *   What is IP Address? Discuss its various type of classes. (June 2020)
2.  **ARP & RARP:**
    *   Explain the working of ARP and RARP. (Dec 2024)
    *   (Short Note) ARP (May 2024, May 2022)
3.  **IPv4 vs. IPv6:**
    *   Give the comparative study of IPv4 and IPv6. (May 2023)
    *   Write down the differences between IPv4 and IPv6. (Dec 2020)
    *   Explain the salient features of IPv6. (May 2019)
4.  **Routing Algorithms & Principles:**
    *   Describe the working principle of Bellman Ford algorithm using suitable example. (Dec 2024, May 2018 CBGS)
    *   Describe the working principle of Least Cost Routing using suitable example. (May 2023 - *Often refers to Dijkstra's*)
    *   Explain Dijkstra’s algorithm with the help of example. (Dec 2020)
    *   Explain Distance Vector routing with example. (May 2022)
    *   What is Routing Algorithm? Discuss its design issues. (June 2020)
    *   Explain Hierarchical Routing Algorithms. (June 2020)
    *   Explain Hierarchical, Broadcast and Multicast routing. (May 2019)
    *   Give the basic difference between broadcast and multicast routing. (Dec 2024)
    *   (Short Note) Multicast Routing (June 2020)
5.  **ICMP (Internet Control Message Protocol):**
    *   Write a brief note on ICMP using its frame formats. (May 2023)
    *   (Short Note) ICMP (May 2022)
6.  **Fragmentation and Reassembly:**
    *   Explain Fragmentation and reassembly using suitable example. (Dec 2024)
    *   (Short Note) Fragmentation and reassembly (May 2023)
7.  **Congestion Control (Network Layer Perspective):**
    *   Explain routing and congestion control. (May 2022 - routing part is general)
    *   What is Congestion control? And how it is implemented in network layer? What is the role of choke packet in managing congestion. (Dec 2020)
    *   Write the general principles of congestion control. (May 2018 CBGS)
    *   What is congestion? How is it avoided? (May 2018 GS)
    *   What is leaky Bucket algorithm? (Problem) Computer on 6 Mbps network regulated by token bucket. Filled at 1 Mbps. Initially 8 megabits capacity. How long can computer transmit at full 6 Mbps? (May 2018 GS)
8.  **Mobile IP:**
    *   Give a brief note on mobile IP. (Dec 2020)
9.  **Internetworking:**
    *   What is internetworking? Explain its service model, global address and packet format. (May 2019)

**V. Transport Layer**

1.  **TCP vs. UDP:**
    *   Compare the TCP header and the UDP header. List fields in TCP header not in UDP header. Give reason for each missing field. (May 2024)
    *   What are TCP and UDP? Explain how you will choose between TCP and UDP? (May 2018 GS)
2.  **TCP (Transmission Control Protocol):**
    *   (Short Note) TCP Congestion Control (Dec 2024)
    *   Discuss the concept of TCP flow control and TCP congestion control. (May 2023, June 2020)
    *   Explain connection establishment and connection release in Transport protocols. (May 2022)
    *   Explain TCP Congestion management. (May 2019)
    *   (Short Note) TCP Header Format (June 2020)
    *   (Problem) A TCP machine is sending windows of 65535 B over a 1 Gbps channel that has a 10 msec one way delay. a) Max throughput? b) Line efficiency? (May 2024)
3.  **UDP (User Datagram Protocol):**
    *   Draw and explain the header format of a User Datagram Protocol (UDP). (Dec 2024)
    *   Give and explain UDP Header Format. (June 2020)
    *   (Short Note) Packet format of UDP (Dec 2020, May 2018 CBGS)
    *   (Short Note) UDP (May 2022)
4.  **Transport Layer Services:**
    *   What are the various parameters of quality of service offered by the transport layer? Discuss their significance. (May 2024)
    *   Explain transport layer multiplexing and demultiplexing. (Dec 2020, May 2019)
    *   Explain connection management issue at transport layer. (May 2018 GS)
5.  **Frame Relay:**
    *   Describe the Frame Relay Protocol Architecture and explain the functions of each one. (May 2022)

**VI. Application Layer & Other Topics**

1.  **HTTP (HyperText Transfer Protocol):**
    *   Write the HTTP request and HTTP response message format. (Dec 2020, May 2019)
    *   (Short Note) HTTP (Dec 2020, May 2018 CBGS)
    *   (Short Note) HTTP and FTP (June 2020)
2.  **SMTP (Simple Mail Transfer Protocol):**
    *   (Short Note) SMTP (Dec 2020, May 2018 CBGS)
3.  **DNS (Domain Name System):**
    *   (Short Note) DNS (May 2024)
    *   Explain the services provided by DNS. (May 2019)
4.  **SNMP (Simple Network Management Protocol):**
    *   Explain Simple Network Management Protocol (SNMP) and its functions. (Dec 2024)
5.  **Electronic Mail:**
    *   What is Electronic Mailing? What are different ways of sending electronic mail (E-mail)? Discuss in detail. (May 2023)
6.  **Finite State Machines (FSM) & Petri Nets:**
    *   What is finite state machine model? How finite state machines are used in the study of network protocols? Explain. (May 2023)
    *   What is finite state models and Petri Net models? (Dec 2020, June 2020)
    *   (Short Note) Finite State Machine (Dec 2024)
7.  **Queuing Theory:**
    *   Explain Little’s theorem. Give one example in support of this theorem. (Dec 2020)
    *   What is Queuing system M/M/1 and M/M/Q system? (June 2020)
    *   Discuss the M/M/1 queuing system with infinity capacity and obtain its steady state probability and mean number of customer in the system. (May 2018 GS)
    *   What is queuing theory? Explain the queuing system M/M/1? (May 2018 CBGS)
8.  **Encryption & Decryption:**
    *   Explain Encryption and Decryption. Why it need? (June 2020)
9.  **Internet Services Standards & Standardization:**
    *   Discuss Internet services standards for TCP/IP and the process of standardization for RFCS and TCP/IP. (May 2022)
10. **Broadband Network Local Loop Technologies:**
    *   Differentiate various broadband network local loop technologies stating their properties. (May 2022)
11. **Miscellaneous Short Notes/Terms:**
    *   (Short Note) FDDI (May 2022)
    *   (Short Note) High Speed LAN (June 2020)
    *   (Short Note) MLMA (Dec 2024 - *low priority, likely typo*)
    *   Explain Networking Protocol (General). (May 2018 GS)
    *   What are the performance measuring metrics for a computer network? Define each of them. (May 2018 CBGS)
    *   Enlist the protocols of presentation layer. Explain the working of any one protocol. (May 2019)

---

**Key Focus Areas for Your 3-Day Preparation:**

1.  **OSI/TCP-IP Models:** Layers, functions, comparison.
2.  **DLL Protocols:** Stop-and-Wait, GBN, SR – know their mechanisms, pros/cons, and be able to draw diagrams.
3.  **MAC Protocols:** Aloha (Pure/Slotted), CSMA (Types, CD/CA) – principles, basic efficiency.
4.  **IP Addressing:** Classes, Subnetting (PRACTICE NUMERICALS).
5.  **Routing Algorithms:** Distance Vector (Bellman-Ford) and Link State (Dijkstra) – conceptual understanding and applying to simple graph examples.
6.  **TCP & UDP:** Comparison, Header fields (especially key ones), TCP flow/congestion control concepts.
7.  **ARP, ICMP, DNS, HTTP:** Purpose and basic working.

**Strategy for 3 Days:**

*   **Day 1: Layers & DLL.** Focus on OSI/TCP-IP, fundamentals, and all Data Link Layer topics including flow/error control and specific protocols (Stop-and-Wait, GBN, SR). Practice framing/stuffing.
*   **Day 2: MAC & Network Layer.** Cover ALOHA, CSMA variants. Then dive deep into IP addressing (practice subnetting!), routing algorithms (conceptual + examples), IPv4/v6, ARP, ICMP.
*   **Day 3: Transport, Application & Revision.** TCP/UDP differences, headers, TCP control mechanisms. Key application protocols (HTTP, DNS, SMTP). Spend the rest of the day revising ALL topics, especially your weak areas and practicing more numericals. Quickly go over short note topics.