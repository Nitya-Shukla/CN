# Day 2: MAC cont., Network Layer

## 1. Topic: CSMA Protocols & Collision-Free

### **CSMA (Carrier Sense Multiple Access)**

*   **Core Idea:** "Listen Before Talk" (LBT).
    *   **Concept:** Before a station transmits data on a shared medium, it first "listens" or "senses" the channel to check if it is busy (i.e., if another station is already transmitting).
    *   **Goal:** To reduce the probability of collisions by avoiding transmission if the channel is already in use.
    *   **Analogy:** Like politely waiting for a pause in a group conversation before speaking.
    *   **Improvement over ALOHA:** ALOHA protocols transmit whenever they have data, leading to high collision rates. CSMA improves on this by adding the sensing step.
    *   **Vulnerability:** Propagation delay. Even if a station senses the channel as idle, another station might have started transmitting, but its signal hasn't reached the sensing station yet. This can still lead to collisions, especially on links with long propagation delays.

*   **Persistent CSMA Methods:** These define how a station behaves once it senses the channel.

    *   **1-Persistent CSMA:**
        *   **Principle:**
            1.  Sense the channel.
            2.  If the channel is **idle**, transmit immediately (with probability 1).
            3.  If the channel is **busy**, continue to sense the channel continuously. As soon as it becomes idle, transmit immediately.
        *   **Behavior:** "Greedy" approach. Transmits as soon as the channel is free.
        *   **Propagation Delay Issue:** If two or more stations sense the channel busy, wait, and then sense it becomes idle at the same time (due to the previous transmission ending), they will both transmit immediately, leading to a guaranteed collision.
        *   **Advantages:**
            *   Reduces idle channel time if only one station is waiting.
        *   **Disadvantages:**
            *   High probability of collision if multiple stations are waiting, especially when the load is high.
            *   Can lead to poor throughput due to these collisions.

    *   **Non-Persistent CSMA:**
        *   **Principle:**
            1.  Sense the channel.
            2.  If the channel is **idle**, transmit immediately.
            3.  If the channel is **busy**, **do not** continuously sense. Instead, wait for a **random amount of time** and then go back to step 1 (sense the channel again).
        *   **Behavior:** Less aggressive than 1-persistent. If the channel is busy, it backs off and tries again later, rather than waiting to pounce.
        *   **Advantages:**
            *   Reduces the probability of collisions compared to 1-persistent because waiting stations are unlikely to sense the channel becoming free at the exact same moment and then transmit simultaneously.
            *   Generally offers better channel utilization than 1-persistent under higher loads.
        *   **Disadvantages:**
            *   Can lead to longer delays if the random backoff time is large, even if the channel becomes free sooner.
            *   Channel might remain idle even if there are stations ready to transmit (during their backoff period).

    *   **p-Persistent CSMA:**
        *   **Principle:** This method is typically used with slotted channels (like Slotted ALOHA).
            1.  Sense the channel.
            2.  If the channel is **idle**:
                *   Transmit with probability **p**.
                *   Defer transmission to the next time slot with probability **(1-p)**. If deferred, when the next slot begins, go back to step 1.
            3.  If the channel is **busy**, continue to sense the channel until it becomes idle. Once idle, go to step 2.
            4.  If transmission is deferred (due to probability 1-p), and the channel is sensed as busy in the next slot, it continues to sense until idle and then re-applies step 2.
        *   **Behavior:** A compromise between 1-persistent and non-persistent. It tries to avoid collisions by randomizing access among stations that find the channel idle, while also attempting to avoid wasting channel capacity.
        *   **Tuning 'p':**
            *   If 'p' is high (close to 1), it behaves like 1-persistent CSMA.
            *   If 'p' is low, it can lead to long delays.
            *   The value of 'p' needs to be chosen carefully based on the number of stations and traffic conditions to optimize performance.
        *   **Advantages:**
            *   Reduces collisions compared to 1-persistent by making stations defer with some probability.
            *   Can achieve better throughput than 1-persistent if 'p' is tuned well.
        *   **Disadvantages:**
            *   More complex than 1-persistent or non-persistent.
            *   Performance is sensitive to the choice of 'p'.
            *   Still doesn't eliminate collisions entirely.

*   **Pros/Cons Summary for CSMA Persistence Methods:**

    | Method           | Pros                                                              | Cons                                                                                             |
    |------------------|-------------------------------------------------------------------|--------------------------------------------------------------------------------------------------|
    | **1-Persistent** | - Attempts to minimize idle channel time when only one station is waiting. | - High collision probability if multiple stations are waiting (greedy).                           |
    |                  |                                                                   | - Can have low throughput under heavy load.                                                      |
    | **Non-Persistent**| - Reduces collision probability significantly compared to 1-persistent. | - Can lead to channel idle time even if stations have data (during random backoff).           |
    |                  | - Generally better channel utilization under higher loads.           | - Introduces potentially longer delays.                                                          |
    | **p-Persistent**  | - Offers a tunable trade-off between 1-persistent and non-persistent. | - Performance highly dependent on the choice of 'p'.                                          |
    |                  | - Can reduce collisions and improve throughput over 1-persistent. | - More complex to implement.                                                                       |
    |                  |                                                                   | - Still susceptible to collisions, especially if 'p' is not optimal for the current load.     |

*   **Exam Angle (from pyqs.md):**
    *   Be ready to **"Explain Persistent and Non-persistent CSMA."** This involves detailing their working principles, how they react to idle and busy channels, and their respective advantages and disadvantages, especially concerning collision probability and channel utilization. Understanding the role of propagation delay is also crucial for explaining why CSMA doesn't eliminate collisions.
    *   While p-persistent is not explicitly asked for as "Persistent CSMA" in the pyqs reference, it's a key part of the CSMA family and good to know for a complete understanding. 

### **CSMA/CD (Carrier Sense Multiple Access with Collision Detection)**

*   **Building on CSMA:** CSMA/CD enhances basic CSMA by adding a mechanism for stations to detect collisions *while they are transmitting* and then take action.
*   **Primary Use:** Classic Ethernet (IEEE 802.3 standards for wired LANs, specifically shared media like coaxial cable or hubs; less relevant in modern switched Ethernet where each device often has a dedicated link).

*   **Working Principle:**
    1.  **Sense (Carrier Sense):** Station listens to the channel. If idle, proceed to transmit. If busy, follow a persistence rule (e.g., 1-persistent: wait until idle then transmit; non-persistent: wait random time then re-sense).
    2.  **Transmit:** Begin transmitting the frame.
    3.  **Detect Collision (Collision Detection - Key Feature):**
        *   **Crucial Point:** The station *simultaneously* monitors the channel while transmitting its own data.
        *   How detection works: A station can detect a collision if the signal on the channel does not match the signal it is transmitting. If it's sending a '0' (e.g., low voltage) but senses a '1' (e.g., high voltage) or a garbled signal, it means another station's transmission is interfering.
        *   **Minimum Frame Size:** For collision detection to work reliably, the frame transmission time must be at least twice the maximum propagation delay between any two stations on the network (2 * Tp). This ensures that if a collision occurs at the farthest end of the network, the signal indicating the collision has enough time to travel back to the sender before the sender finishes transmitting its frame. This is why Ethernet has a minimum frame size (64 bytes).
    4.  **Jam Signal:** If a collision is detected:
        *   The station immediately stops transmitting its data frame.
        *   It then transmits a brief "jam signal" (e.g., 32-48 bits). The purpose of the jam signal is to ensure that all other stations involved in the collision also become aware that a collision has occurred and that the channel is noisy.
    5.  **Backoff:** After sending the jam signal, the station waits for a random amount of time before attempting to retransmit the frame (go back to step 1). The backoff algorithm used is typically Binary Exponential Backoff (BEB).

*   **Binary Exponential Backoff (BEB):**
    *   **Purpose:** To reduce the probability of repeated collisions between the same stations after an initial collision.
    *   **How it works:**
        1.  After the *n*-th collision for a particular frame, the station chooses a random integer *k* from the range `0 <= k < 2^n` (up to a maximum limit, typically `n=10`).
        2.  The station then waits for *k* "slot times" before attempting to retransmit. A slot time is usually defined as 2 * Tp (twice the maximum propagation delay), which is related to the minimum frame size and ensures that a station attempting retransmission will likely find the channel clear if no new collisions happen.
        3.  The range of possible waiting times doubles with each collision, spreading out retransmission attempts.
        4.  After a certain number of failed attempts (e.g., 16 collisions), the station typically gives up and reports an error to higher network layers.
    *   **Example Calculation:**
        *   **1st Collision (n=1):** Choose *k* from {0, 1}. Wait *k* slot times.
        *   **2nd Collision (n=2):** Choose *k* from {0, 1, 2, 3}. Wait *k* slot times.
        *   **3rd Collision (n=3):** Choose *k* from {0, 1, 2, 3, 4, 5, 6, 7}. Wait *k* slot times.
        *   ... and so on.
        *   **10th Collision (n=10):** Choose *k* from {0, 1, ..., 1023}. Wait *k* slot times.
        *   **11th to 16th Collision:** The range often stays capped at {0, 1, ..., 1023}.

*   **Advantages of CSMA/CD:**
    *   Relatively simple to implement for shared media.
    *   Performs reasonably well under light to moderate network loads.
    *   Collision detection stops wasteful transmission of entire frames once a collision is identified.

*   **Disadvantages of CSMA/CD:**
    *   Performance degrades significantly under heavy load due to increased collisions and backoff periods.
    *   Not suitable for very long-distance networks due to the propagation delay constraint for collision detection (the 2*Tp rule).
    *   Only works for half-duplex operations (cannot send and receive simultaneously on the same channel).
    *   Ineffective in wireless networks where collision detection is difficult (due to hidden station and exposed station problems, and because a station cannot easily listen while transmitting at full power).

*   **Exam Angle (from pyqs.md):**
    *   **"Explain working principle"**: Detail the steps: Sense, Transmit, Detect Collision (emphasize *while transmitting*), Jam Signal, and Backoff.
    *   **"BEB with example"**: Explain how the backoff range increases exponentially and provide a step-by-step calculation for a few collision instances as shown above.
    *   **"Short Note"**: Summarize the key aspects: carrier sensing, collision detection, jam signal, BEB, and its primary application in Ethernet (shared media). Mention pros and cons briefly. 

### **CSMA/CA (Carrier Sense Multiple Access with Collision Avoidance)**

*   **Motivation for Collision Avoidance:** In environments where collision *detection* is impractical or impossible, the focus shifts to *avoiding* collisions in the first place. This is particularly true for:
    *   **Wireless Networks (e.g., WiFi/IEEE 802.11):**
        *   **Hidden Station Problem:** Station A can hear an Access Point (AP), and Station C can hear the AP, but A and C cannot hear each other due to range limitations or obstructions. If A and C transmit to the AP simultaneously, a collision occurs at the AP, but A and C are unaware of each other's transmission.
        *   **Exposed Station Problem:** Station B is transmitting to A. Station C wants to transmit to D. If C senses B's transmission, it might defer, even though its transmission to D would not interfere with B's reception at A. This reduces channel efficiency.
        *   **Cost/Complexity:** Implementing reliable collision detection on wireless radios (which are typically half-duplex and transmit at much higher power than they receive) is difficult and expensive.

*   **Working Principle - "Listen Before Talk, Then Try to Avoid Collision":**
    CSMA/CA employs several strategies to reduce collision probability:
    1.  **Sense (Carrier Sense):** Before transmitting, the station listens to the channel (both physical carrier sense and virtual carrier sense using NAV - Network Allocation Vector, explained later).
    2.  **InterFrame Spacing (IFS):** If the channel is sensed as idle, the station doesn't transmit immediately. It waits for a specific period called an InterFrame Space. IFS times are used to prioritize access and ensure the channel is clear.
        *   Different types of IFS exist (e.g., SIFS - Short IFS, DIFS - Distributed IFS, EIFS - Extended IFS). SIFS is the shortest, used for ACKs, CTS, etc. A station typically waits for DIFS after sensing the channel idle before starting its random backoff procedure or transmitting.
    3.  **Contention Window & Random Backoff:** If the channel remains idle after DIFS, the station selects a random backoff time from a range called the "Contention Window" (CW).
        *   The station decrements its backoff timer as long as the channel remains idle. If the channel becomes busy during the backoff, the timer is frozen and resumes when the channel is idle again for a DIFS period.
        *   When the backoff timer reaches zero, the station transmits its frame.
        *   Similar to BEB in CSMA/CD, the CW size often increases if a transmission attempt fails (e.g., no ACK received), to reduce subsequent collision probability.
    4.  **Optional RTS/CTS Exchange (Collision Avoidance Mechanism):** For larger data frames, or in environments prone to hidden stations, a four-way handshake can be used:
        *   **RTS (Request To Send):** The sending station (A) transmits a short RTS frame to the intended receiver (B). The RTS includes the duration of the upcoming data transmission and ACK.
        *   **CTS (Clear To Send):** If B receives the RTS and is ready, it responds with a short CTS frame. The CTS also includes the duration information. All stations hearing the CTS (even those that didn't hear the RTS, like a hidden station C) will set their Network Allocation Vector (NAV) for the specified duration, effectively reserving the medium and refraining from transmission.
        *   **DATA:** A then transmits its data frame to B.
        *   **ACK (Acknowledgement):** If B receives the data correctly, it sends an ACK back to A. All stations hearing the ACK will also update their NAV if necessary.
        *   **Benefit:** Collisions are more likely to occur only with the short RTS frames. If an RTS collides, the sender won't receive a CTS and will know to backoff and retry, without wasting time sending a large data frame.
    5.  **Acknowledgement (ACK):** Transmissions are typically followed by an ACK from the receiver to confirm successful reception. If an ACK is not received within a timeout period, the sender assumes the frame was lost (possibly due to collision or corruption) and retransmits it after performing a backoff.

*   **Network Allocation Vector (NAV):**
    *   A timer maintained by each station that indicates the predicted time the medium will be busy due to transmissions from other stations.
    *   Stations hearing an RTS or CTS frame update their NAV based on the duration field in those frames.
    *   A station considers the medium busy if either its physical carrier sense detects activity OR its NAV is non-zero (virtual carrier sense).

*   **Conceptual Diagram for RTS/CTS and Hidden Station:**
    *   Imagine three stations: A, AP (Access Point), C.
    *   A can communicate with AP. C can communicate with AP. A and C cannot hear each other (hidden stations relative to each other).
    *   **Without RTS/CTS:** If A and C transmit to AP simultaneously, collision occurs at AP. A and C are unaware until (possibly) they don't receive an ACK.
    *   **With RTS/CTS:**
        1.  A sends RTS to AP.
        2.  AP sends CTS. This CTS is heard by C (even if C didn't hear A's RTS).
        3.  C now knows the medium will be busy for the duration specified in CTS and sets its NAV, deferring any potential transmission and avoiding a collision with A's data to AP.

*   **Where Used:** Predominantly in IEEE 802.11 (WiFi) networks.

*   **Advantages of CSMA/CA:**
    *   Effectively reduces collisions in wireless environments where detection is not feasible.
    *   RTS/CTS mechanism helps mitigate the hidden station problem.
    *   ACKs provide link-layer reliability.

*   **Disadvantages of CSMA/CA:**
    *   Higher overhead compared to CSMA/CD due to IFS, backoff, ACKs, and optional RTS/CTS frames.
    *   Still does not completely eliminate collisions (e.g., RTS frames can collide, or two stations might finish backoff at the same time).
    *   Performance can suffer in dense environments with many contending stations.

*   **Exam Angle (from pyqs.md):**
    *   **"Explain working principle"**: Describe the sequence: sense, IFS, contention window/backoff, optional RTS/CTS, Data, ACK. Emphasize the "avoidance" aspect.
    *   **"RTS/CTS"**: Explain the four-way handshake (RTS, CTS, Data, ACK), its purpose (especially for hidden stations), and how NAV is used. A simple diagram description of the hidden station problem and how RTS/CTS solves it would be beneficial.
    *   **"Short Note"**: Summarize CSMA/CA: its use in wireless (802.11), the need for avoidance over detection, key mechanisms like IFS, backoff, RTS/CTS, and ACKs. Briefly mention pros/cons. 

### **Collision-Free Protocols (Briefly)**

*   **Concept:** These protocols aim to eliminate collisions entirely, rather than just reducing their probability (like CSMA methods) or managing them after detection. They achieve this by ensuring that only one station can transmit at any given time, often through explicit permission, reservations, or turn-taking mechanisms. These are also known as "controlled access" or "deterministic" MAC protocols, especially under high load conditions where they can offer more predictable performance.

*   **General Characteristics:**
    *   **Efficiency at High Load:** Can be more efficient than contention-based protocols (like ALOHA or CSMA) when many stations have data to send, as channel time is not wasted on collisions or recovery.
    *   **Potential for Higher Delay at Low Load:** If only one station has data, it might still have to wait for its turn or permission, leading to higher delay compared to contention schemes where it might transmit immediately.
    *   **Complexity:** Can be more complex to implement due to the coordination required.

*   **1. Binary Countdown (Bit-Map Protocol Variant):**
    *   **Concept:** A method where stations with data to send broadcast their addresses (or a unique station identifier) one bit at a time, from most significant to least significant. If a station sending a '0' bit sees that another station has broadcast a '1' bit in the same position, it drops out of the contention for that round. The station with the highest address wins and gets to transmit its data frame.
    *   **Working Principle:**
        1.  **Reservation Period:** The protocol starts with a reservation period consisting of N bit-slots, where N is the number of bits in the station addresses.
        2.  **Broadcasting Address:** Each station ready to send data broadcasts its address, bit by bit, into these slots.
        3.  **Arbitration:**
            *   All stations monitor the channel.
            *   If a station has a '0' in the current bit position of its address but detects a '1' on the channel (from another station with a '1' in that position), it knows it has lost the arbitration and stops broadcasting its address bits. It must wait for the next contention period.
            *   Stations with a '1' in that bit position continue if they also broadcasted a '1'.
        4.  **Winner:** After N bit-slots, only one station (the one with the numerically highest address among those contending) will still be broadcasting. This station has won the right to transmit.
        5.  **Data Transmission:** The winning station transmits its data frame.
        6.  The process repeats for the next frame.
    *   **Simple Example:** Assume 3 stations want to send, with addresses (binary):
        *   Station A: `101`
        *   Station B: `011`
        *   Station C: `110`
        *   **Bit-slot 1 (MSB):**
            *   A broadcasts '1'.
            *   B broadcasts '0'. B sees '1' (from A or C) on the channel, so B drops out.
            *   C broadcasts '1'.
            *   Stations A and C continue.
        *   **Bit-slot 2:**
            *   A broadcasts '0'.
            *   C broadcasts '1'. A sees '1' (from C) on the channel, so A drops out.
            *   Station C continues.
        *   **Bit-slot 3 (LSB):**
            *   C broadcasts '0'.
            *   Station C is the only one left.
        *   **Result:** Station C (address `110`) wins and transmits its data frame.
    *   **Advantages:** Efficient in terms of channel use for arbitration; priority can be based on address.
    *   **Disadvantages:** Overhead of N bit-slots for each frame transmission, which can be significant if frames are small or addresses are long.

*   **2. Adaptive Tree Walk Protocol:**
    *   **Concept:** This protocol views all stations as leaf nodes in a binary tree. To find a station ready to transmit, the protocol polls nodes in the tree. If a node is polled and it or any station in the subtree below it is ready, the polling continues down that branch. If not, that branch is pruned from the current search.
    *   **Working Principle:**
        1.  **Tree Structure:** All stations are considered leaves of a conceptual binary tree.
        2.  **Polling/Probing:** The central controller (or a distributed mechanism) starts by polling the root of the tree (enabling all stations).
        3.  **Response:**
            *   If no station in the polled subtree responds (i.e., no one wants to transmit), that subtree is considered silent for this round.
            *   If one or more stations in the polled subtree respond (this might cause a collision if multiple stations respond simultaneously), the protocol then polls the children of that node.
        4.  **Recursive Search:** The process is applied recursively. If a poll to a node results in a collision (meaning multiple stations under that node are ready), the protocol separately polls its left child and then its right child.
        5.  **Leaf Node:** When a poll reaches a leaf node (a single station) and that station is ready, it transmits its frame.
        6.  The search can then continue from where it left off or restart from the root (or a higher unpruned node) to find the next ready station.
    *   **Simple Example (as per May 2018 GS problem reference for "Sixteen stations..."):**
        *   Imagine 8 stations (0 to 7) for simplicity, arranged in a binary tree.
            ```
                  Root (0-7)
                 /        \
            N1(0-3)       N2(4-7)
           /   \         /   \
        N3(0-1) N4(2-3) N5(4-5) N6(6-7)
        / \     / \     / \     / \
       S0 S1   S2 S3   S4 S5   S6 S7
            ```
        *   **Scenario:** Suppose S2 and S5 want to transmit.
        *   1. Poll Root (0-7): Collision (S2 and S5 respond).
        *   2. Poll N1(0-3): S2 responds (no collision here if only S2 is ready in this branch). If N1 was polled and S2 transmitted, the search would then need to address S5. Alternatively, if both branches from Root are explored systematically:
        *   2a. Poll N1(0-3): S2 responds. (Potentially collision if another station in 0-3 was also ready).
            *   Poll N3(0-1): Silence.
            *   Poll N4(2-3): S2 responds. (If only S2 is ready in 2-3 branch, then S2 transmits).
        *   2b. Poll N2(4-7): S5 responds.
            *   Poll N5(4-5): S5 responds. (If only S5 is ready in 4-5 branch, then S5 transmits).
            *   Poll N6(6-7): Silence.
        *   The protocol efficiently narrows down to the ready stations.
    *   **Advantages:** Adapts to the number of ready stations. Can be efficient if few stations are ready.
    *   **Disadvantages:** Can be slow if many stations are ready due to the depth of the tree search. Overhead of polling messages.

*   **Exam Angle (from pyqs.md):**
    *   **"Explain Binary Countdown"**: Describe the bit-by-bit address broadcasting and arbitration process. A simple example is key.
    *   **"Explain Adaptive Tree Walk"**: Describe the tree traversal polling mechanism to find ready stations. Illustrate with a small tree and a scenario of a few ready stations.
    *   **Solve related problems**: Be prepared for problems that might ask you to trace the steps of these protocols given a set of ready stations (e.g., for Adaptive Tree Walk, how many polling steps or which nodes are polled).
    *   Understand these as examples of **collision-free** or **controlled access** methods, distinct from contention-based CSMA protocols. 