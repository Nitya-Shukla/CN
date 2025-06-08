# Day 3: Transport, Application Layers & Full Revision

## 1. Topic: Transport Layer Intro & UDP

### **Transport Layer Services**

*   **Primary Role:** The transport layer is responsible for providing logical communication between **application processes** running on different hosts. While the network layer provides host-to-host communication, the transport layer provides process-to-process communication.

*   **Key Services:**

    1.  **Process-to-Process Delivery (Port Numbers):**
        *   **Concept:** Enables data to be directed to the correct application process on the destination host.
        *   **Mechanism (Port Numbers):**
            *   The transport layer uses **port numbers** (16-bit numbers) to identify specific application processes.
            *   A source port number and a destination port number are included in the header of each transport layer segment.
            *   The combination of an IP address (from the network layer) and a port number creates a **socket address** (e.g., `192.168.1.100:80`), which uniquely identifies a specific process on a specific host.
            *   **Well-known ports (0-1023):** Reserved for common services (e.g., HTTP port 80, FTP port 21, DNS port 53).
            *   **Registered ports (1024-49151):** Can be registered by software vendors.
            *   **Dynamic/Private/Ephemeral ports (49152-65535):** Used by client applications as temporary source ports.
        *   **Example:** When you browse a website, your browser (client process) might use an ephemeral port (e.g., 51000) to send a request to the web server (server process) listening on port 80.

    2.  **Multiplexing and Demultiplexing:**
        *   **Multiplexing (at Sender):**
            *   The process of gathering data chunks from different application processes on the source host, adding transport layer headers (with source/destination port numbers, sequence numbers, etc.) to create **segments** (for TCP) or **datagrams** (for UDP), and then passing these segments/datagrams to the network layer.
            *   Allows multiple applications on a single host to use the network simultaneously over a single network interface.
        *   **Demultiplexing (at Receiver):**
            *   The process of receiving segments/datagrams from the network layer and delivering the data payload to the correct application process on the destination host.
            *   The transport layer examines the **destination port number** in the incoming segment/datagram to identify the appropriate socket (and thus the application process) to which the data should be delivered.
            *   TCP further uses the source IP, source port, destination IP, and destination port (a 4-tuple) to direct segments to the correct socket, as TCP is connection-oriented.
            *   UDP uses only the destination port to direct datagrams to the correct socket.

    3.  **Reliable Data Transfer (Primarily TCP):**
        *   **Concept:** Ensuring that data arrives at the destination process correctly, in order, and without loss or duplication.
        *   **Mechanisms (used by TCP):**
            *   **Acknowledgements (ACKs):** Receiver sends ACKs back to the sender to confirm receipt of data.
            *   **Sequence Numbers:** Sender numbers data segments; receiver uses them to reorder out-of-order segments and detect missing segments.
            *   **Retransmissions:** Sender retransmits lost or corrupted segments (detected via timeouts or duplicate ACKs).
            *   **Checksum:** Used for error detection within the segment header and data.
        *   **UDP:** Provides an unreliable, "best-effort" service. It includes a checksum for error detection but does not implement ACKs, sequence numbers for reordering, or retransmissions for lost data. Reliability, if needed, must be handled by the application layer when using UDP.

    4.  **Flow Control (Primarily TCP):**
        *   **Concept:** Prevents a fast sender from overwhelming a slow receiver with too much data too quickly.
        *   **Mechanism (Sliding Window Protocol - used by TCP):**
            *   The receiver advertises its available buffer space (receive window size) to the sender.
            *   The sender ensures that the amount of unacknowledged data it sends does not exceed this advertised window.
        *   **UDP:** Does not provide flow control. If an application sends UDP datagrams too quickly, the receiver might drop them if its input buffer overflows.

    5.  **Congestion Control (Primarily TCP):**
        *   **Concept:** Aims to prevent network congestion by regulating the rate at which senders inject data into the network, based on perceived network conditions.
        *   **Mechanism (used by TCP):** Senders adjust their sending rate based on feedback like packet loss (inferred from timeouts or duplicate ACKs) or explicit congestion notifications.
            *   Involves algorithms like Slow Start, AIMD (Additive Increase, Multiplicative Decrease), Fast Retransmit, Fast Recovery.
        *   **UDP:** Does not provide congestion control. UDP sources can potentially contribute to network congestion if they send data too aggressively without regard for network conditions.

*   **Exam Angle (from pyqs.md):**
    *   **"Explain transport layer multiplexing and demultiplexing":**
        *   Define multiplexing: gathering data from multiple app processes at the sender, adding headers (especially port numbers), and passing to the network layer.
        *   Define demultiplexing: receiving segments at the receiver, using the destination port number (and other info for TCP) to deliver data to the correct application process/socket.
        *   Highlight the role of port numbers.
    *   **"QoS parameters (reliability, delay, jitter, bandwidth)":**
        *   While these are general QoS parameters, the transport layer directly influences some:
            *   **Reliability:** TCP provides it; UDP doesn't. Explain how (ACKs, sequence numbers, retransmissions for TCP).
            *   **Delay/Jitter:** Transport layer protocols themselves add some processing delay. Congestion control in TCP indirectly tries to manage delay by reducing network load. UDP is often preferred for applications sensitive to delay/jitter (like VoIP, online gaming) *if* they can tolerate some loss, because UDP has lower overhead and no retransmission delays.
            *   **Bandwidth:** Flow control and congestion control in TCP manage how much bandwidth a connection attempts to use, aiming for fair sharing and avoiding network collapse.
        *   For the exam, define each parameter and then relate how transport layer services (or lack thereof in UDP for some) impact them. 

### **UDP (User Datagram Protocol)**

*   **Overview:** UDP is a minimalistic, connectionless transport layer protocol. It provides a way for applications to send messages, called datagrams, to other hosts on an IP network without prior communication to set up special transmission channels or data paths.

*   **Characteristics:**
    1.  **Connectionless:**
        *   No handshake process (like TCP's 3-way handshake) is required before sending data.
        *   Each UDP datagram is handled independently by the network.
        *   This reduces latency as there's no setup delay.
    2.  **Unreliable ("Best Effort" Delivery):**
        *   UDP does not guarantee delivery of datagrams. Datagrams can be lost, duplicated, or arrive out of order.
        *   It does not use acknowledgements (ACKs) or sequence numbers to ensure reliable, ordered delivery.
        *   It does not perform retransmissions of lost datagrams.
        *   Reliability, if required, must be implemented at the application layer.
    3.  **Fast and Low Overhead:**
        *   Minimal header size (8 bytes).
        *   No connection establishment/teardown overhead.
        *   No need to maintain connection state (sequence numbers, window sizes, etc.), which simplifies processing.
        *   These factors contribute to its speed and efficiency for certain types of applications.
    4.  **No Flow Control:**
        *   UDP does not have mechanisms to slow down the sender if the receiver is overwhelmed.
        *   If the sender transmits datagrams faster than the receiver can process them, datagrams may be dropped at the receiver's input buffer.
    5.  **No Congestion Control:**
        *   UDP does not react to network congestion (e.g., by reducing its sending rate).
        *   Applications using UDP can potentially contribute to network congestion if they send data too aggressively.
    6.  **Message-Oriented (Datagram-Oriented):**
        *   UDP preserves message boundaries. If an application sends a message, UDP places it in a datagram and sends it.
        *   The receiver gets the message exactly as it was sent (assuming it arrives and isn't corrupted beyond detection).
        *   This contrasts with TCP's byte-stream service, where message boundaries are not preserved by TCP itself.

*   **UDP Header Format:**

    ```
    0                   1                   2                   3
    0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1
    +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
    |          Source Port          |       Destination Port        |
    +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
    |            Length             |           Checksum            |
    +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
    |                             Data                              ...
    +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
    ```

    *   **Field Explanations (8-byte header):**
        1.  **Source Port (16 bits):**
            *   Identifies the port of the sending application process.
            *   This field is optional. If not used, it should be set to zero.
            *   It is primarily used by the receiver if it needs to send a reply back to the source.
        2.  **Destination Port (16 bits):**
            *   Identifies the port of the receiving application process on the destination host.
            *   This is a required field.
        3.  **Length (16 bits):**
            *   Specifies the length in bytes of the entire UDP datagram (UDP header + UDP data).
            *   The minimum value is 8 bytes (for a UDP datagram with no data, just the header).
        4.  **Checksum (16 bits):**
            *   Used for error detection. It covers the UDP header, the UDP data, and a "pseudo-header" constructed from fields in the IP header (source IP, destination IP, protocol, and UDP length).
            *   **Optional in IPv4:** If not used, it is set to all zeros. If an IPv4 checksum is computed to be all zeros, it is transmitted as all ones (the one's complement of 0).
            *   **Mandatory in IPv6:** The checksum must be calculated and used with IPv6. If an application running over IPv6 does not wish to compute a checksum, it should set the checksum field to all zeros. The IPv6 network stack on the source host will then compute it.
            *   If the checksum calculation results in a value of zero, it is transmitted as all ones (0xFFFF) to avoid confusion with an unused checksum.

*   **Common UDP Applications & Why UDP is Suitable:**

    1.  **DNS (Domain Name System - Port 53):**
        *   **Reason:** DNS queries are typically small request-response transactions. The overhead of TCP connection setup is unnecessary and would add latency. Lost queries can be re-queried by the application.
    2.  **DHCP (Dynamic Host Configuration Protocol - Ports 67, 68):**
        *   **Reason:** DHCP is used for initial IP address assignment. It often involves broadcast/multicast messages where TCP is not suitable. Speed is important during boot-up.
    3.  **SNMP (Simple Network Management Protocol - Port 161, 162):**
        *   **Reason:** Management queries and responses are often small. UDP's low overhead is beneficial. Traps (asynchronous notifications) also suit UDP.
    4.  **TFTP (Trivial File Transfer Protocol - Port 69):**
        *   **Reason:** Simple file transfer where the overhead of TCP is not desired. TFTP implements its own simple reliability and flow control if needed.
    5.  **Streaming Media (e.g., VoIP, online video/audio streaming):**
        *   **Reason:** Real-time applications are more sensitive to delay and jitter than to occasional loss of a small amount of data. TCP's retransmissions can cause significant delays, disrupting the stream. Applications often have their own mechanisms to handle minor losses (e.g., interpolation).
    6.  **Online Gaming:**
        *   **Reason:** Similar to streaming, low latency is critical for responsiveness. Lost game state updates might be tolerable or handled by the game logic (e.g., a missed position update might be quickly superseded by a new one).

*   **Exam Angle (from pyqs.md):**
    *   **"Draw and explain header format":**
        *   Draw the 4 fields (Source Port, Destination Port, Length, Checksum) and their bit lengths.
        *   Explain the purpose of each field clearly, including details like the optional nature of source port and checksum (in IPv4).
    *   **"Characteristics":**
        *   List and explain the key characteristics: Connectionless, Unreliable, Fast/Low Overhead, No Flow Control, No Congestion Control, Message-Oriented.
    *   **"Short Note":**
        *   A concise summary including: what UDP is (simple, connectionless transport), its key characteristics (unreliable, fast, low overhead), main header fields (mention port numbers, length, checksum), and 1-2 typical application examples (like DNS or streaming) explaining why UDP is a good fit for them. 