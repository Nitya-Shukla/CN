# Day 3: Transport, Application Layers & Full Revision

## 3. Topic: Application Layer Protocols

### **DNS (Domain Name System)**

*   **Purpose:**
    *   The Domain Name System (DNS) is a hierarchical and decentralized naming system for computers, services, or other resources connected to the Internet or a private network.
    *   Its primary function is to **translate human-readable domain names** (e.g., `www.google.com`) into machine-readable **IP addresses** (e.g., `172.217.160.142` for IPv4 or `2607:f8b0:4004:804::200e` for IPv6).
    *   It can also perform reverse lookups: translating IP addresses back into domain names.
    *   DNS makes the internet more user-friendly, as users can remember names instead of numerical IP addresses.
    *   It also provides a level of indirection, allowing the IP address associated with a service to change without users needing to know the new IP address (they just continue using the domain name).

*   **Hierarchical Structure:**
    *   DNS has a tree-like structure. The hierarchy consists of several levels:
        1.  **Root Level:** At the very top, denoted by a `.` (dot), though often implicit. Managed by ICANN (Internet Corporation for Assigned Names and Numbers).
        2.  **Top-Level Domains (TLDs):** These are the suffixes like `.com`, `.org`, `.net`, `.gov`, `.edu`, and country-code TLDs (ccTLDs) like `.uk`, `.in`, `.de`.
        3.  **Second-Level Domains (SLDs):** These are the names registered by organizations or individuals directly under a TLD (e.g., `google` in `google.com`, `wikipedia` in `wikipedia.org`).
        4.  **Subdomains:** Further subdivisions of SLDs (e.g., `mail.google.com`, `en.wikipedia.org`, where `mail` and `en` are subdomains).
        5.  **Hostnames:** The actual name of a specific machine (e.g., `www` in `www.example.com`).

    *   **Simple Hierarchy Diagram:**
        ```
                      . (Root)
                     /|\
                    / | \
                   /  |  \
レベル1 (TLD)   .com .org .edu ...
                 |    |    |
レベル2 (SLD) google wikipedia stanford ...
                 |      |       |
レベル3 (Subdomain) www    en      cs  ...
        ```

    *   **Authoritative Name Servers:** For each **zone** (a portion of the domain namespace), there are authoritative name servers that hold the definitive DNS records for that zone. For example, Google runs the authoritative name servers for the `google.com` zone.

*   **Resolver (DNS Client):**
    *   A DNS resolver is a client-side program or library (often part of the operating system) that initiates DNS queries on behalf of an application (e.g., a web browser, email client).
    *   When an application needs to resolve a domain name, it passes the request to the local resolver.
    *   The resolver typically queries one or more configured DNS servers (e.g., provided by an ISP, a public DNS service like Google Public DNS `8.8.8.8` or Cloudflare `1.1.1.1`, or a local network router).

*   **Iterative vs. Recursive Queries:**

    1.  **Recursive Query:**
        *   **Concept:** The resolver asks its configured DNS server (e.g., ISP DNS server) to perform the full resolution process.
        *   **Mechanism:**
            1.  Client resolver sends a recursive query for `www.example.com` to its local DNS server.
            2.  The local DNS server takes responsibility. If it doesn't know the answer (not cached), it queries a root server.
            3.  The root server might refer it to a TLD server for `.com`.
            4.  The local DNS server then queries the `.com` TLD server.
            5.  The TLD server might refer it to the authoritative name server for `example.com`.
            6.  The local DNS server queries the `example.com` authoritative server, which returns the IP address for `www.example.com`.
            7.  The local DNS server sends the final IP address back to the client resolver.
        *   **Diagram (Conceptual Flow for Recursive to Local DNS, then Local DNS Iterates):**
            ```
            Client Resolver  --- Recursive Query (www.example.com) ---> Local DNS Server
                                                                          |
                                                                          | (if not cached)
                                                                          v
                                                                        Root Server --(refers to .com server)--> Local DNS Server
                                                                                                                  |
                                                                                                                  v
                                                                                                                .com TLD Server --(refers to example.com server)--> Local DNS Server
                                                                                                                                                            |
                                                                                                                                                            v
                                                                                                                                                          example.com Auth Server --(IP for www)--> Local DNS Server
            Client Resolver  <--- IP Address ------------------------- Local DNS Server
            ```
        *   The burden of resolution is on the queried DNS server.

    2.  **Iterative Query:**
        *   **Concept:** The resolver asks a DNS server for the answer. If the server doesn't know, it provides a referral to another DNS server that might know (closer to the target). The resolver then queries that referred server, and so on.
        *   **Mechanism (Typical interaction between DNS servers):** When a local DNS server (acting as a client to other servers) performs resolution after receiving a recursive query from an end-user resolver, it typically uses iterative queries to root servers, TLD servers, and authoritative servers.
            1.  Resolver asks Server A: "What is IP for `www.example.com`?"
            2.  Server A (e.g., root server) replies: "I don't know, but try Server B (a .com TLD server)."
            3.  Resolver asks Server B: "What is IP for `www.example.com`?"
            4.  Server B replies: "I don't know, but try Server C (the authoritative server for `example.com`)."
            5.  Resolver asks Server C: "What is IP for `www.example.com`?"
            6.  Server C replies with the IP address.
        *   The resolver does the work of chasing referrals.

*   **Common DNS Record Types (Resource Records - RRs):**

    | Type   | Name                       | Purpose                                                                 |
    |--------|----------------------------|-------------------------------------------------------------------------|
    | **A**  | Address Record             | Maps a hostname to an **IPv4 address** (e.g., `192.0.2.1`).             |
    | **AAAA**| IPv6 Address Record        | Maps a hostname to an **IPv6 address** (e.g., `2001:0db8::1`).          |
    | **CNAME**| Canonical Name Record      | Creates an **alias** from one domain name to another (the canonical name). The DNS resolution continues with the canonical name. (e.g., `ftp.example.com` might be a CNAME to `server1.example.com`). |
    | **MX** | Mail Exchange Record       | Specifies the **mail server(s)** responsible for accepting email messages on behalf of a domain. Includes a preference value to prioritize servers. |
    | **NS** | Name Server Record         | Delegates a DNS zone to use the given **authoritative name servers**.     |
    | **PTR**| Pointer Record             | Used for **reverse DNS lookups**; maps an IP address to a hostname. Resides in `in-addr.arpa` (IPv4) or `ip6.arpa` (IPv6) domains. |
    | **SOA**| Start of Authority Record  | Provides authoritative information about the DNS zone, including the primary name server, email of the domain administrator, domain serial number, and timers relating to refreshing the zone. |
    | **TXT**| Text Record                | Allows administrators to insert arbitrary text into a DNS record. Used for various purposes, like SPF (Sender Policy Framework) for email validation or domain ownership verification. |
    | **SRV**| Service Record             | Specifies the hostname and port number of servers for specified services (e.g., LDAP, XMPP). |

*   **DNS Caching:**
    *   To improve performance and reduce DNS traffic, DNS responses are cached at various levels:
        *   **Client OS/Resolver Cache:** Your computer often caches recent DNS lookups.
        *   **Local DNS Server Cache (e.g., ISP server):** These servers cache responses they get from other servers.
    *   Each DNS record has a **Time-To-Live (TTL)** value specified by the authoritative name server. This value tells caches how long they are allowed to store and use the record before it should be refreshed (requeried from an authoritative server).
    *   Caching reduces latency for users (if a lookup is cached locally or nearby) and reduces the load on authoritative name servers.

*   **Exam Angle (from pyqs.md):**
    *   **"Explain services provided by DNS":**
        *   Primary service: Domain name to IP address translation (and vice-versa).
        *   Enables use of memorable names instead of IP addresses.
        *   Provides indirection (IP can change, name remains).
        *   Facilitates load distribution (multiple A records for one name) and service location (MX, SRV records).
        *   Hierarchical and distributed database.
    *   **"Short Note on DNS":**
        *   Purpose (name-to-IP).
        *   Hierarchical structure (root, TLD, SLD).
        *   Key components (resolver, name servers).
        *   Mention query types (recursive/iterative concept).
        *   Mention caching and TTL.
    *   Focus on understanding the difference between iterative and recursive queries and being able to explain the general lookup process.
    *   Know the purpose of the main record types (A, AAAA, CNAME, MX, NS). 