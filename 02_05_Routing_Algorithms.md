# Day 2: MAC cont., Network Layer

## 5. Topic: Routing Algorithms

### **Routing Concepts**

*   **Goal of Routing:**
    *   The primary goal of routing is to **determine the "best" path** for data packets to travel from a source device (e.g., a computer) to a destination device (e.g., a server) across an internetwork (a collection of interconnected networks).
    *   Routers are the devices responsible for making these path determination decisions.
    *   "Best" path is not always the shortest path; it depends on the **routing metrics** used. It could be the fastest, least congested, most reliable, or most economical path.
    *   Routing involves creating and maintaining **routing tables** on routers. These tables store information about network topology and the paths to various destinations.
    *   **Forwarding vs. Routing:**
        *   **Routing:** The process of building the routing tables (the "control plane"). This involves running routing algorithms to learn about the network.
        *   **Forwarding:** The process of taking an incoming packet, looking up its destination address in the routing table, and sending it out on the appropriate outgoing interface (the "data plane").

*   **Routing Metrics:**
    *   Metrics are values used by routing algorithms to measure the "cost" or "desirability" of a particular path to a destination network. The path with the lowest metric is generally considered the best path.
    *   **Common Metrics:**
        1.  **Hop Count:**
            *   The number of routers (hops) a packet must pass through to reach the destination.
            *   Simple to calculate but doesn't consider link speed, congestion, or reliability. A path with fewer hops might be slower if those hops involve slow links.
            *   Used by protocols like RIP (Routing Information Protocol).
        2.  **Bandwidth:**
            *   The data capacity of a link (e.g., in Mbps or Gbps).
            *   Algorithms can choose paths with higher bandwidth. The metric might be inversely proportional to the bandwidth of the slowest link in the path (bottleneck bandwidth).
            *   Used by protocols like OSPF (Open Shortest Path First) and EIGRP (Enhanced Interior Gateway Routing Protocol).
        3.  **Delay (Latency):**
            *   The time it takes for a packet to travel from source to destination over a path.
            *   Includes propagation delay, transmission delay, queuing delay, and processing delay.
            *   Important for real-time applications (e.g., voice, video).
            *   Used by protocols like EIGRP.
        4.  **Cost:**
            *   An arbitrary value, often assigned by a network administrator, to a link. It can be based on bandwidth (e.g., cost = 10^8 / bandwidth in bps for OSPF), monetary cost, or other policy considerations.
            *   Lower cost indicates a more preferred path.
            *   Used by OSPF.
        5.  **Load (Utilization):**
            *   The amount of traffic currently using a link, typically expressed as a percentage of its capacity.
            *   Routing algorithms might try to avoid heavily loaded links.
            *   Can change frequently, making it a dynamic metric.
            *   Used by EIGRP.
        6.  **Reliability:**
            *   A measure of the link's dependability (e.g., based on error rates or link uptime).
            *   Paths with higher reliability are preferred.
            *   Used by EIGRP.
        7.  **MTU (Maximum Transmission Unit):**
            *   The largest packet size that a link can carry. While not a direct cost metric, path MTU discovery is important to avoid fragmentation.

*   **Static vs. Dynamic Routing:**

    1.  **Static Routing:**
        *   **Definition:** Routes are manually configured in the routing table by a network administrator.
        *   **Process:** The administrator explicitly defines the destination network, subnet mask, and the next-hop IP address or exit interface for each route.
        *   **Characteristics:**
            *   **Simplicity:** Easy to configure in very small, stable networks.
            *   **Security:** Can be more secure as routes are explicitly defined and don't change unless manually altered. No routing protocol overhead broadcasting network information.
            *   **Control:** Provides precise control over routing paths.
            *   **Low Overhead:** No CPU cycles are used for routing calculations, and no bandwidth is consumed by routing protocol updates.
        *   **Disadvantages:**
            *   **Scalability:** Becomes unmanageable in large networks. Every change requires manual updates on all relevant routers.
            *   **Fault Intolerance:** Does not automatically adapt to network changes (e.g., link failures or new links). If a static route's path becomes unavailable, traffic to that destination will be dropped unless a backup static route is also configured.
            *   **Administrative Burden:** Time-consuming to maintain and prone to human error.
        *   **Use Cases:**
            *   Small, simple networks that rarely change.
            *   Stub networks (networks with only one way out).
            *   Defining a default route (gateway of last resort).
            *   Connecting to a specific network for security reasons where dynamic routing is not desired.

    2.  **Dynamic Routing:**
        *   **Definition:** Routers automatically learn about remote networks and update their routing tables by exchanging information with other routers using routing protocols.
        *   **Process:** Routers running a dynamic routing protocol advertise the networks they know about, listen to advertisements from other routers, and use a routing algorithm to calculate the best paths.
        *   **Characteristics:**
            *   **Scalability:** Suitable for networks of all sizes, from small to very large.
            *   **Adaptability/Fault Tolerance:** Automatically detects network topology changes (e.g., link failures, new routers/links) and reroutes traffic accordingly.
            *   **Ease of Administration (in large networks):** Once configured, it requires less ongoing manual intervention for route management compared to static routing for all routes.
        *   **Disadvantages:**
            *   **Complexity:** More complex to initially configure and troubleshoot than static routing.
            *   **Resource Overhead:** Consumes router CPU resources for routing calculations and network bandwidth for routing protocol messages.
            *   **Security:** Can be less secure if not properly configured (e.g., routers might learn incorrect routes from unauthorized sources). Routing protocols often have authentication mechanisms to mitigate this.
        *   **Types of Dynamic Routing Protocols:**
            *   **Interior Gateway Protocols (IGPs):** Used for routing within a single autonomous system (AS). Examples: RIP, EIGRP, OSPF, IS-IS.
            *   **Exterior Gateway Protocols (EGPs):** Used for routing between different autonomous systems. Example: BGP (Border Gateway Protocol).
        *   **Use Cases:** Most networks of any significant size or networks that are expected to change or grow.

*   **Exam Angle (from pyqs.md):**
    *   **"Goal of Routing":** Explain finding the best path, role of routers, routing tables.
    *   **"Metrics":** List and briefly explain common metrics (hop count, bandwidth, delay, cost, load, reliability).
    *   **"Static vs. Dynamic Routing":** Compare them based on configuration, scalability, fault tolerance, overhead, and typical use cases. Provide clear advantages and disadvantages of each. 

### **Distance Vector Routing Algorithms (e.g., RIP, Bellman-Ford)**

*   **Concept:** "Routing by Rumor" - routers learn about the network from their immediate neighbors.

*   **Working Principle:**
    1.  **Initialization:** Each router knows the distance (cost) to its directly connected neighbors. For all other destinations, the distance is initially considered infinite.
    2.  **Distance Vector (Routing Table):** Each router maintains a **distance vector** (effectively its routing table). This table lists:
        *   All known destination networks.
        *   The current best-known distance (cost) to reach each destination.
        *   The next-hop router (the neighbor through which packets to that destination should be sent).
    3.  **Periodic Sharing:** At regular intervals (e.g., every 30 seconds for RIP), each router broadcasts its **entire distance vector (routing table)** to all of its **directly connected neighbors only**.
    4.  **Information Processing (Bellman-Ford Algorithm Core Idea):**
        *   When a router (say, Router R) receives a distance vector from its neighbor (Neighbor N):
            *   For each destination (Dest) in Neighbor N's received vector, Router R calculates its own potential distance to Dest *through* Neighbor N. This is: `Cost_R_to_N + Cost_N_to_Dest` (where `Cost_R_to_N` is the known cost from R to N, and `Cost_N_to_Dest` is the distance advertised by N to reach Dest).
            *   Router R compares this calculated distance with the distance it currently has in its own routing table for Dest.
            *   **If the calculated distance via Neighbor N is shorter:** Router R updates its own routing table for Dest, setting the new shorter distance and listing Neighbor N as the next hop.
            *   **If the received information indicates a path through the advertising neighbor itself has changed cost (e.g., N used to route to X via R, but now N has a new path to X):** R updates its table if N offers a better path or if its current path through N is no longer valid/has a higher cost.
            *   **If the advertisement is from the current next-hop router for a destination, and the cost has increased:** Router R updates its cost to that destination.
    5.  **Convergence:** This process repeats. Eventually, all routers will converge on the shortest paths to all destinations, according to the chosen metric (typically hop count for simple DV protocols like RIP).

*   **Bellman-Ford Algorithm:**
    *   **Mathematical Basis:** Distance Vector protocols are based on the Bellman-Ford algorithm (or sometimes the Ford-Fulkerson algorithm, which Bellman-Ford is a case of).
    *   **Iterative Process:** The algorithm iteratively relaxes edges in a graph. In each iteration, it finds shorter paths. For a graph with N nodes, it guarantees finding the shortest paths (if no negative cycles) in at most N-1 iterations.
    *   **How it relates to DV:** Each router performs a part of the Bellman-Ford calculation. The periodic exchange of routing tables and updates is essentially a distributed, iterative application of the Bellman-Ford principle.
    *   **Practice with a small graph example:**
        1.  Draw a simple network topology (3-4 routers, links with costs/hops).
        2.  Create initial distance vector tables for each router (0 to self, direct cost to neighbors, infinity to others).
        3.  Simulate rounds of updates: For each round, assume routers exchange tables. Show how one router updates its table based on its neighbors' tables from the *previous* round. Explicitly write out the calculation: `MyCostToDest = CostToNeighbor + NeighborAdvertisedCostToDest`.
        4.  Continue until tables stabilize (no more changes after an exchange).

*   **Count-to-Infinity Problem:**
    *   **The Problem:** A significant issue with simple Distance Vector protocols. When a link fails or a router goes down, the information about this failure propagates slowly through the network. This can lead to:
        *   **Routing Loops:** Router A might think the path to network X is via B, and B might think it's via A, creating a loop for packets destined to X.
        *   **Slow Convergence:** It can take a long time for all routers to agree on the new topology after a failure. During this time, packets can be looped or dropped.
        *   **How it happens (Good news / Bad news):** "Good news travels fast" (a new, shorter path is quickly adopted). "Bad news travels slow" (when a path becomes unavailable, routers might hear outdated good news from neighbors who haven't yet learned of the failure, leading to metric values for the unreachable network incrementally increasing towards "infinity").
    *   **Example of Counting to Infinity:**
        *   A -> B -> C -> X (Network X)
        *   Link C-X fails. C knows X is unreachable (distance infinity).
        *   Before C tells B, B might still advertise to A that it can reach X via C (e.g., at cost 2).
        *   C tells B that X is unreachable.
        *   B receives an update from A (who hasn't heard from B yet about C-X failure) that A can reach X via B (A thinks B can reach X in 2, so A thinks it can reach X in 3 via B).
        *   B might incorrectly think it can reach X via A now (cost 3 + cost B-A = 4). B tells C it can reach X via A (cost 4).
        *   C might think it can reach X via B (cost 1 + cost B-X = 5).
        *   This process continues, with the metric for X incrementing slowly until it reaches the protocol's defined "infinity" (e.g., 16 hops for RIP).
    *   **Solutions (Mitigation Techniques):**
        1.  **Defining Infinity (Maximum Metric):** Setting a maximum value for a metric (e.g., 16 for RIP). If a metric exceeds this, the destination is considered unreachable. This stops the counting but doesn't eliminate loops during the counting phase.
        2.  **Split Horizon:**
            *   Principle: A router does not advertise a route back to the neighbor from which it learned the route.
            *   If Router A learns about Network X from Router B, Router A will not advertise its route to Network X back to Router B.
            *   Helps prevent 2-node routing loops.
        3.  **Poison Reverse (Split Horizon with Poison Reverse):**
            *   Stronger version of split horizon.
            *   A router advertises a route back to the neighbor from which it learned the route, but with an infinite metric (poisoning the route).
            *   If Router A learns about Network X from Router B, Router A will advertise Network X back to Router B with a metric of 16 (infinity for RIP).
            *   This explicitly tells Router B that A's path to X is through B, so B should not use A for X.
            *   More effective in breaking larger loops than simple split horizon.
        4.  **Hold-Down Timers:**
            *   When a router learns that a route has become unavailable, it places the route in "hold-down" for a period.
            *   During hold-down, the router will not accept any new information about that route from other routers unless it has a better metric than the original (now invalid) route.
            *   Prevents the router from accepting outdated good news about a failed route while the bad news is still propagating.
        5.  **Triggered Updates (Flash Updates):**
            *   When a router detects a change in its routing table (e.g., a link goes down, metric changes), it immediately sends an update to its neighbors, rather than waiting for the next periodic update interval.
            *   Speeds up convergence.

*   **Examples of Distance Vector Protocols:**
    *   **RIP (Routing Information Protocol):** Uses hop count as the metric. Max hops 15 (16 is infinity). Updates every 30 seconds. Prone to count-to-infinity. Uses split horizon, poison reverse.
    *   **IGRP (Interior Gateway Routing Protocol):** Cisco proprietary. Uses a composite metric (bandwidth, delay, load, reliability, MTU). More complex than RIP. (Largely superseded by EIGRP).

*   **Advantages of Distance Vector:**
    *   Simple to understand and implement.
    *   Less CPU and memory intensive than Link State protocols initially.

*   **Disadvantages of Distance Vector:**
    *   Slow convergence (especially after failures).
    *   Prone to routing loops and count-to-infinity (though mitigated by techniques).
    *   Routers only have a "local" view of the network (they only know what their neighbors tell them, not the full topology).
    *   Periodic full table updates can consume bandwidth, especially on slow links.

*   **Exam Angle (from pyqs.md):**
    *   **"Working principle":** Detail the steps: initialization, maintaining distance vector, periodic sharing with neighbors, and how updates are processed using the Bellman-Ford idea (comparing costs via neighbors).
    *   **"Bellman-Ford with example":** Explain its iterative nature and how it forms the basis. Work through a small network graph showing table updates over a few rounds until stabilization.
    *   **"Count-to-infinity":** Clearly explain what the problem is, why it occurs (bad news travels slow), and give a simple scenario. List and briefly explain common solutions (max metric, split horizon, poison reverse, hold-down timers, triggered updates). 

### **Link State Routing Algorithms (e.g., OSPF, Dijkstra)**

*   **Concept:** Each router strives to build a complete map (topology graph) of the entire network. Path calculations are made locally based on this complete map.

*   **Working Principle (General Steps):**
    1.  **Discover Neighbors and Learn Link Costs:**
        *   Each router first discovers its directly connected neighbors (e.g., by sending "Hello" packets).
        *   It then determines the cost (metric) to reach each neighbor (e.g., based on link bandwidth for OSPF, or a configured cost).
    2.  **Construct Link State Packet (LSP) or Link State Advertisement (LSA):**
        *   Each router creates a small packet called a Link State Packet (LSP) or Link State Advertisement (LSA).
        *   The LSP contains:
            *   The router's ID.
            *   A list of its directly connected neighbors and the cost to reach each one.
            *   Sequence numbers and aging information to manage LSP versions and expiration.
    3.  **Flood LSPs to All Other Routers:**
        *   The router reliably floods its LSP to **all other routers** within the same routing domain or area (not just immediate neighbors).
        *   Flooding ensures that every router receives LSPs from every other router.
        *   Reliable flooding mechanisms (e.g., acknowledgments, retransmissions) are used to ensure all routers get all LSPs.
        *   Typically, LSPs are flooded when a router boots up, when a link status changes (up/down), or when a link cost changes.
    4.  **Build Link-State Database (LSDB) and Network Map:**
        *   Each router stores the most recently received LSPs from all other routers in its **Link-State Database (LSDB)**.
        *   Since each LSP describes a piece of the network (a router and its direct connections), the collection of all LSPs in the LSDB allows each router to form an identical, complete topological map of the network.
    5.  **Compute Shortest Paths (Dijkstra's Algorithm):**
        *   With the complete network map (from the LSDB), each router **independently** calculates the shortest path from itself to every other destination network/router.
        *   This is done using the **Dijkstra (Shortest Path First - SPF) algorithm**.
        *   The result of the SPF calculation is a shortest-path tree, with the calculating router as the root.
        *   The best paths (next hop, total cost) are then installed into the router's routing table.

*   **Dijkstra's Algorithm (Shortest Path First - SPF):**
    *   **Goal:** To find the shortest paths from a single source node (the calculating router) to all other nodes in a graph with non-negative edge weights (costs).
    *   **Conceptual Steps:**
        1.  **Initialization:**
            *   Assign a tentative distance value to every node: zero for the initial node (our router), and infinity for all other initial nodes.
            *   Mark all nodes as unvisited. Set the initial node as current.
        2.  **Iteration:**
            *   For the current node, consider all of its unvisited neighbors and calculate their tentative distances through the current node.
            *   Compare the newly calculated tentative distance to the current assigned value and assign the smaller one. For example, if the current node A is at distance 5, and the link from A to B has cost 2, then the tentative distance to B via A is 5+2=7. If B's current tentative distance was 10, update it to 7.
            *   When all of the neighbors of the current node have been considered, mark the current node as visited.
            *   Select the unvisited node that is marked with the smallest tentative distance, and set it as the new "current node." Repeat from step 2a.
        3.  **Termination:** The algorithm terminates when all reachable nodes have been visited, or when the smallest tentative distance among the unvisited nodes is infinity (indicating remaining nodes are unreachable from the source).
    *   **Practice with a small graph example:**
        1.  Draw a simple network topology (4-5 routers, links with costs). Pick one router as the source.
        2.  Create a table to track: Node, Visited (Y/N), Tentative Distance, Previous Node (to reconstruct path).
        3.  Step-by-step, apply Dijkstra's: Initialize distances. In each step, select the unvisited node with the smallest tentative distance, mark it visited, and update distances of its neighbors. Show the table changing with each iteration.
        4.  Once complete, reconstruct the shortest path tree from the source router.

*   **Comparison: Distance Vector (DV) vs. Link State (LS):**

    | Feature               | Distance Vector (e.g., RIP)                                  | Link State (e.g., OSPF)                                       |
    |-----------------------|--------------------------------------------------------------|---------------------------------------------------------------|
    | **Information Shared** | Entire routing table (or parts of it)                          | Information about directly connected links (LSPs/LSAs)        |
    | **To Whom Shared**    | Only directly connected neighbors                            | All routers in the routing domain/area (via flooding)         |
    | **Network View**      | "Local" view; routers only know what neighbors tell them     | Each router has a complete map (topology) of the network    |
    | **Complexity**        | Algorithmically simpler                                      | More complex algorithm (Dijkstra), more complex data structures |
    | **Convergence Speed** | Generally slower, prone to count-to-infinity                  | Faster convergence, as changes are flooded quickly            |
    | **Routing Loops**     | More susceptible (mitigation techniques needed)              | Less susceptible once converged (loops transiently possible during flooding) |
    | **Resource Usage**    | Less CPU/memory initially, but periodic full table updates can be bandwidth-intensive | More CPU/memory for SPF calculation and LSDB storage; updates are small & event-driven (less bandwidth overall after initial flood) |
    | **Scalability**       | Limited scalability, especially due to convergence issues    | More scalable for larger networks                             |
    | **Example Protocols** | RIP, IGRP                                                    | OSPF, IS-IS                                                   |

*   **Examples of Link State Protocols:**
    *   **OSPF (Open Shortest Path First):** Widely used IGP. Uses cost as a metric (often based on bandwidth). Hierarchical design with Areas. Complex but scalable and robust.
    *   **IS-IS (Intermediate System to Intermediate System):** Another IGP, similar in complexity and scalability to OSPF. Popular in ISP networks.

*   **Advantages of Link State:**
    *   **Fast Convergence:** Each router calculates its own paths based on a complete map. When a change occurs, LSPs are flooded quickly, allowing routers to update their maps and recalculate paths promptly.
    *   **Robustness against Routing Loops:** Since each router has a consistent view of the network topology, routing loops are less likely once converged.
    *   **Scalability:** Generally scales better to larger networks than DV protocols.
    *   **Event-Driven Updates:** LSPs are typically sent only when there's a change in topology (or periodically with long intervals for refresh), reducing bandwidth consumption compared to periodic full table updates of DV.

*   **Disadvantages of Link State:**
    *   **Higher CPU and Memory Requirements:** Storing the LSDB and running the SPF algorithm (Dijkstra) can be more CPU and memory intensive, especially in very large networks or areas.
    *   **Initial Complexity:** More complex to understand and configure than simpler DV protocols.
    *   **Initial LSP Flooding:** The initial flooding of LSPs from all routers can cause a burst of traffic. Subsequent updates are usually small.

*   **Exam Angle (from pyqs.md):**
    *   **"Working principle":** Detail the steps: neighbor discovery, LSP creation, LSP flooding, LSDB construction, and SPF (Dijkstra) calculation to build the routing table.
    *   **"Dijkstra's with example":** Explain its purpose and conceptual steps. Work through a small network graph showing how the algorithm determines shortest paths from a source node, tracking tentative distances and visited nodes.
    *   **"Compare with DV":** Use a table or bullet points to highlight key differences in information shared, network view, convergence, loop susceptibility, resource usage, and scalability. 

### **Other Routing Concepts (Briefly)**

*   **Hierarchical Routing:**
    *   **Purpose:** To manage routing in very large networks (like the internet) by dividing the network into a hierarchy of smaller, manageable domains or regions, often called **Autonomous Systems (AS)**.
    *   **How it works:**
        *   **Intra-AS Routing (Interior Gateway Protocols - IGPs):** Routers within the same AS use an IGP (like OSPF, RIP, EIGRP) to exchange routing information and calculate paths *within* that AS. Routers in one AS do not need to know the detailed topology of other ASes.
        *   **Inter-AS Routing (Exterior Gateway Protocols - EGPs):** Special routers called **gateway routers** or **border routers** sit at the edge of an AS and connect to other ASes. These routers use an EGP (primarily **BGP - Border Gateway Protocol**) to exchange reachability information *between* ASes. BGP exchanges information about which networks are reachable through which ASes, often based on policies rather than just shortest paths.
        *   **Abstraction:** Routers inside an AS see their own AS in detail but view other ASes as abstract "clouds" reachable via their gateway routers.
    *   **Advantages:**
        *   **Scalability:** Significantly reduces the amount of routing information that needs to be stored and processed by individual routers. Routing table sizes are smaller.
        *   **Administrative Autonomy:** Each AS can choose its own IGP and manage its internal routing policies independently.
        *   **Reduced Update Traffic:** Routing updates from within one AS do not necessarily need to be propagated to other ASes.
    *   **Example:** The Internet is the prime example of hierarchical routing, composed of many interconnected ASes.

*   **Broadcast Routing:**
    *   **Purpose:** To deliver a packet from a source node to **all other nodes** in the network (or a specific broadcast domain).
    *   **Methods:**
        1.  **Flooding (Uncontrolled):**
            *   The source node sends the packet to all its neighbors.
            *   Each router that receives the packet forwards it on all outgoing links *except* the one it arrived on.
            *   **Problem:** Generates a vast number of duplicate packets, can lead to "broadcast storms," and is highly inefficient and resource-intensive.
        2.  **Controlled Flooding (Sequence Number-Controlled Flooding):**
            *   To avoid processing the same broadcast packet multiple times, each broadcast packet is given a unique sequence number (Source ID + Sequence #).
            *   Routers keep track of (source, sequence #) of broadcast packets they have already seen and forwarded. If a duplicate arrives, it's discarded.
        3.  **Spanning Tree Routing (Reverse Path Forwarding - RPF check often used as part of this concept in practice for multicast, but idea applies to broadcast source-based tree):**
            *   A **spanning tree** is a subgraph that connects all routers with no cycles.
            *   To broadcast, the source sends the packet along the spanning tree branches.
            *   Each router forwards the packet only on links that are part of the spanning tree (excluding the one it arrived on if it's part of the tree path from source).
            *   More efficient than simple flooding as it avoids loops and minimizes redundant transmissions.
            *   **Reverse Path Forwarding (RPF) Check:** A common technique. A router only forwards a broadcast packet if it arrives on the link that the router would use to send unicast packets *back* to the source of the broadcast. This implicitly creates a source-based tree and helps prevent loops and duplicates from simple flooding.

*   **Multicast Routing:**
    *   **Purpose:** To deliver a packet from a source node to a **specific group of destination nodes** that have explicitly joined a "multicast group." This is a one-to-many or many-to-many communication.
    *   **Key Concepts:**
        *   **Multicast Group Address:** A special IP address (Class D range in IPv4: 224.0.0.0 to 239.255.255.255) that identifies a multicast group.
        *   **Group Membership (IGMP):** Hosts use a protocol like **IGMP (Internet Group Management Protocol)** to inform their local routers that they want to join or leave a specific multicast group.
        *   **Multicast Routing Protocols:** Routers use multicast routing protocols (e.g., PIM, DVMRP, MOSPF) to build distribution trees that efficiently deliver multicast traffic from sources to all members of a group.
            *   These protocols aim to create trees that span only to network segments where there are active members of the group.
            *   They prune branches of the tree that do not lead to any group members.
    *   **Difference from Broadcast:**
        *   **Broadcast:** Goes to *all* nodes in a broadcast domain, whether they want the packet or not.
        *   **Multicast:** Goes *only* to nodes that have explicitly joined the specific multicast group. More efficient for targeted group communication.
    *   **Applications:** Streaming video/audio (IPTV, video conferencing), online gaming, stock ticker updates, distributing software updates.

*   **Exam Angle (from pyqs.md):**
    *   **"Explain Hierarchical, Broadcast, Multicast routing":**
        *   **Hierarchical:** Purpose (scalability), AS concept, IGP vs. EGP (BGP), gateway routers.
        *   **Broadcast:** Purpose (to all), methods (flooding pitfalls, controlled flooding/sequence numbers, spanning tree/RPF concept for efficiency).
        *   **Multicast:** Purpose (to a specific group), group addresses, IGMP for membership, general idea of multicast routing protocols building distribution trees.
    *   **"Difference between broadcast and multicast":** Broadcast is to all nodes in the domain; multicast is to a self-selected group of nodes. Multicast is more targeted and efficient for group applications. 