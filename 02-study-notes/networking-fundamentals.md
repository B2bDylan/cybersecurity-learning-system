# Networking Fundamentals

This study note brings together the main networking concepts I have studied so far during my cybersecurity learning journey.

My objective is not to describe every networking technology in depth. Instead, this document connects the foundations that helped me understand how devices communicate, how data moves through a network, how services are reached, and how networking connects directly to cybersecurity.

---

## 1. Why Networking Matters in Cybersecurity

Networking is one of the foundations of cybersecurity because systems rarely operate in isolation.

Devices communicate with other devices, applications communicate with servers, users access services across local and external networks, and security controls inspect or regulate this communication.

Before identifying abnormal network behavior, it is important to understand what normal communication looks like.

The main idea I developed during my studies is that network communication is not produced by one technology.

It is the result of multiple layers, protocols, addresses, ports, devices, and security controls working together.

A simplified mental model is:

**Application → Communication → Network Infrastructure**

For example, when a user opens a website, several different processes may happen:

**Domain name**
↓
**DNS resolution**
↓
**IP destination**
↓
**TCP connection**
↓
**TLS protection when present**
↓
**HTTP request**
↓
**Encapsulation and network delivery**
↓
**Server response**
↓
**Decapsulation**
↓
**Browser displays the result**

Understanding this overall flow helped me stop seeing networking concepts as isolated definitions.

---

# 2. The OSI Model

The **OSI model** is a conceptual framework that divides network communication into seven layers.

Its main value for me is that it provides a map for understanding where different protocols, addresses, devices, and communication processes operate.

| Layer | Name         | Examples / Function   |
| ----- | ------------ | --------------------- |
| 7     | Application  | HTTP, DNS             |
| 6     | Presentation | TLS, formatting       |
| 5     | Session      | Session control       |
| 4     | Transport    | TCP, UDP, ports       |
| 3     | Network      | IP, routing           |
| 2     | Data Link    | MAC addresses, frames |
| 1     | Physical     | Bits, cables, Wi-Fi   |

### Layer 7 — Application

This is where application-level protocols such as HTTP and DNS operate.

It is the layer closest to what the user or application is trying to accomplish.

### Layer 6 — Presentation

This layer is associated with how information is represented and protected.

TLS is one example that appeared in my studies when connecting networking with encrypted web communication.

### Layer 5 — Session

This layer relates to establishing and managing communication sessions.

### Layer 4 — Transport

This is where protocols such as **TCP and UDP** operate.

Ports are also important at this level because they help identify the service or application involved in communication.

### Layer 3 — Network

This layer works with **IP addresses and routing**.

Routers operate primarily at this layer because they move packets between different networks.

### Layer 2 — Data Link

This layer works with **MAC addresses and frames**.

Layer 2 switches use MAC addresses to forward communication inside a local network.

### Layer 1 — Physical

This represents the physical transmission of bits through media such as cables or wireless communication.

The OSI model helps me ask an important troubleshooting and security question:

**At which layer is this communication or problem happening?**

---

# 3. TCP/IP Model

The **TCP/IP model** is a more practical model for internet communication.

The model I studied contains four layers:

1. Application
2. Transport
3. Internet
4. Network Interface

A simplified relationship with the OSI model is:

| TCP/IP            | Related OSI Layers |
| ----------------- | ------------------ |
| Application       | 7, 6, 5            |
| Transport         | 4                  |
| Internet          | 3                  |
| Network Interface | 2, 1               |

The OSI model helps organize networking conceptually.

The TCP/IP model helps visualize how communication happens in practical internet environments.

Both models are useful, but for different perspectives.

---

# 4. Encapsulation and Decapsulation

One of the concepts that helped me connect the networking layers was **encapsulation**.

When information moves down through the network stack, each layer adds information needed for communication.

A simplified flow is:

**Application Data**
↓
**TCP Segment**
↓
**IP Packet**
↓
**Frame**
↓
**Bits**

This process is called **encapsulation**.

At the destination, the opposite process occurs:

**Bits**
↓
**Frame**
↓
**Packet**
↓
**Segment**
↓
**Application Data**

This is **decapsulation**.

This concept became especially useful when I started looking at packet captures because a single communication can contain information related to several networking layers.

---

# 5. IP Addresses, MAC Addresses and Ports

These concepts identify different parts of communication.

## IP Address

An IP address helps identify the host involved in network communication.

IP addressing is closely connected with Layer 3 and routing.

A simple mental model is:

**IP = which host**

## MAC Address

A MAC address identifies a network interface at the local network level.

Layer 2 switches use MAC information to forward frames inside a LAN.

A useful distinction is:

**IP → network-level communication**

**MAC → local network / interface communication**

## Port

A port identifies which application or service should receive network communication.

A useful mental model from my studies is:

**IP address = which host**

**Port = which service**

Common ports I have studied include:

| Port | Protocol / Service | Basic purpose                |
| ---- | ------------------ | ---------------------------- |
| 21   | FTP                | File transfer                |
| 22   | SSH                | Secure remote administration |
| 80   | HTTP               | Web communication            |
| 443  | HTTPS              | Encrypted web communication  |
| 445  | SMB                | File and printer sharing     |
| 3389 | RDP                | Remote desktop access        |

Ports are important in cybersecurity because understanding which service is listening or communicating helps provide context when analyzing traffic.

---

# 6. TCP — Reliable, Connection-Based Communication

**TCP — Transmission Control Protocol** is connection-oriented.

Its main characteristics in my study material are:

* reliable communication
* ordered delivery
* connection establishment before normal data transfer
* sequence numbers
* acknowledgement numbers
* flags
* checksum-based integrity checking

TCP is designed to make sure communication happens in an organized way.

---

## TCP Three-Way Handshake

Before normal data transfer begins, TCP establishes a connection using the **three-way handshake**.

### Step 1 — SYN

**Client → Server**

`SYN`

The client requests the beginning of a connection.

### Step 2 — SYN/ACK

**Server → Client**

`SYN/ACK`

The server accepts the request and synchronizes communication.

### Step 3 — ACK

**Client → Server**

`ACK`

The client confirms the connection.

The connection is now established.

Simplified:

```text
Client                           Server

   -------- SYN ---------------->

   <------ SYN / ACK ------------

   -------- ACK ---------------->

        CONNECTION ESTABLISHED

   <======= DATA ===============>
```

---

# 7. TCP Data Transfer

After the handshake, application data can begin moving between both sides.

TCP uses several mechanisms to maintain reliable communication.

## Sequence Numbers

Sequence numbers help keep transmitted data in the correct order.

## Acknowledgement Numbers

Acknowledgement numbers help confirm which data has been successfully received.

## Checksum

The checksum provides an integrity check that helps determine whether transmitted data was corrupted.

## Flags

TCP flags control different parts of connection behavior.

The flags that appeared most frequently in my studies were:

| Flag    | Meaning in my study context              |
| ------- | ---------------------------------------- |
| SYN     | Start / request connection               |
| SYN/ACK | Server response to connection request    |
| ACK     | Acknowledge communication                |
| FIN     | Close the connection normally            |
| RST     | Reset / terminate communication abruptly |

These flags become especially useful when reading traffic in Wireshark.

---

# 8. Closing a TCP Connection

The three-way handshake describes how TCP **opens** the connection.

TCP also has a normal process for closing communication.

The clean-close sequence I studied is:

### Step 1

One side sends:

`FIN`

It indicates that this side wants to close its part of the connection.

### Step 2

The other side responds:

`ACK`

The FIN request has been acknowledged.

### Step 3

The other side sends its own:

`FIN`

It now closes its side of the communication.

### Step 4

The first side responds:

`ACK`

The connection can now end.

Simplified:

```text
Side A                           Side B

   -------- FIN ---------------->

   <------- ACK -----------------

   <------- FIN -----------------

   -------- ACK ---------------->

        CONNECTION CLOSED
```

This helped me understand an important distinction:

**SYN → SYN/ACK → ACK = connection establishment**

while

**FIN → ACK → FIN → ACK = clean connection termination**

---

# 9. RST — Abrupt Termination

TCP communication does not always end through a clean FIN/ACK sequence.

The **RST — Reset** flag represents an abrupt termination of communication.

A useful mental distinction is:

**FIN = orderly close**

**RST = abrupt reset**

Understanding the difference becomes useful when examining connection failures or unusual packet behavior.

---

# 10. Why the TCP Handshake Matters for Security

Understanding the normal TCP handshake is necessary before recognizing abnormal behavior.

A normal connection begins with:

**SYN → SYN/ACK → ACK**

One security concept I encountered during my studies was the **SYN flood**.

Instead of completing normal connections, a large number of SYN requests can cause a server to keep preparing resources for connections that never finish.

The important lesson for me was not only memorizing the name of the attack.

It was understanding that:

**to recognize abnormal TCP behavior, I first need to understand normal TCP behavior.**

---

# 11. UDP — Lightweight, Stateless Communication

**UDP — User Datagram Protocol** works differently from TCP.

UDP does not establish a continuous connection before sending data.

Its main characteristics in my study material are:

* stateless communication
* no three-way handshake
* no continuous connection
* lower overhead
* faster communication
* no delivery guarantee comparable to TCP

A simple comparison is:

| TCP                        | UDP                   |
| -------------------------- | --------------------- |
| Connection-based           | Stateless             |
| Handshake required         | No handshake          |
| Reliable delivery          | No delivery guarantee |
| Ordered communication      | Simpler transmission  |
| More communication control | Lower overhead        |

UDP can be useful where speed is more important than perfect delivery.

Examples from my studies include:

* voice communication
* streaming
* online gaming
* DNS queries

The key lesson was not that one protocol is better than the other.

They are designed for different communication requirements.

---

# 12. DNS — From Names to Addresses

**DNS — Domain Name System** translates human-readable domain names into IP addresses.

Without DNS, users would have to remember IP addresses instead of names such as:

```text
example.com
```

A simple mental model is:

**People remember names. Networks communicate using addresses. DNS connects the two.**

---

# 13. DNS Hierarchy

Domain names follow a hierarchy.

A simplified example is:

```text
root
 |
 └── .com                 TLD
      |
      └── example         Second-Level Domain
           |
           └── admin      Subdomain
```

Result:

```text
admin.example.com
```

Important concepts I studied include:

* Root domain
* TLD — Top-Level Domain
* ccTLD — Country Code Top-Level Domain
* Second-Level Domain
* Subdomain

---

# 14. Common DNS Records

Some DNS record types I studied are:

| Record | Purpose                                       |
| ------ | --------------------------------------------- |
| A      | Points to an IPv4 address                     |
| AAAA   | Points to an IPv6 address                     |
| CNAME  | Points one domain name to another             |
| MX     | Defines the mail server                       |
| TXT    | Stores text used for verification or policies |

TXT records may be used for validation and security-related policies such as SPF and DMARC.

---

# 15. DNS Resolution

A simplified DNS resolution process is:

```text
Client / Browser
      ↓
Local DNS Cache
      ↓
Recursive DNS Server
      ↓
Root Server
      ↓
TLD Server
      ↓
Authoritative DNS Server
      ↓
DNS Record
      ↓
Response returned to client
```

The computer first checks whether it already knows the answer locally.

If not, a recursive DNS server can continue the lookup process.

The root server helps locate the appropriate TLD infrastructure.

The TLD server points toward the authoritative DNS server.

The authoritative server stores the official DNS records for the domain.

The answer is then returned to the client.

---

# 16. DNS Caching and TTL

DNS responses do not necessarily need to be resolved from the beginning every time.

Responses can be cached.

**TTL — Time To Live** determines how long a DNS response should remain cached before another lookup is required.

Caching can:

* improve performance
* reduce repeated lookups
* reduce unnecessary DNS traffic

DNS is also relevant to cybersecurity because domain information appears in areas such as:

* monitoring
* troubleshooting
* phishing analysis
* infrastructure analysis
* forensics

---

# 17. Router

A **router** connects different networks.

It primarily operates at Layer 3 and works with IP addresses and routing.

Its role is to determine where packets should travel.

A simple mental model is:

**Router = connects networks**

For example:

```text
Local Network
     ↓
   Router
     ↓
Other Networks / Internet
```

Routing allows communication to move beyond the local network.

---

# 18. Switch

A **switch** connects devices inside a local network.

A Layer 2 switch works mainly with:

* MAC addresses
* frames

It forwards frames toward the appropriate device inside the LAN.

A simplified distinction is:

**Router → connects networks**

**Switch → connects devices inside a network**

My study material also introduced **Layer 3 switches**, which can use both MAC and IP information and perform some routing functions between network segments.

---

# 19. VLAN

**VLAN — Virtual Local Area Network** allows one physical network infrastructure to be divided into separate logical segments.

For example:

```text
Physical Switch
      |
      ├── VLAN A
      │     └── Department A
      |
      └── VLAN B
            └── Department B
```

Both groups may use the same physical infrastructure while remaining logically separated.

VLANs can improve:

* organization
* segmentation
* traffic control
* security

The cybersecurity connection is especially important.

Segmentation helps control which parts of an environment should communicate directly with each other.

---

# 20. Firewall

A **firewall** controls inbound and outbound network traffic according to security rules.

The firewall may evaluate information such as:

* source
* destination
* port
* protocol

A simple mental model is:

**Firewall = network traffic control**

Its purpose is to decide which communication should be allowed and which should be blocked.

---

## Stateful Firewall

A stateful firewall tracks the connection.

It considers:

* connection state
* communication context
* behavior between systems

This provides more contextual decision-making.

## Stateless Firewall

A stateless firewall evaluates packets individually according to defined rules.

The main distinction I learned is:

**Stateful → understands the connection**

**Stateless → evaluates packet by packet**

---

# 21. VPN

**VPN — Virtual Private Network** creates a protected tunnel across another network such as the internet.

A simplified representation is:

```text
Network / Device A
        |
        |====== Encrypted VPN Tunnel ======|
                                          |
                                    Network / Device B
```

VPN technology can be used to:

* connect remote users
* connect geographically separate networks
* protect traffic traveling across public networks
* provide controlled access to private environments

In my practical learning, VPNs are also important because TryHackMe uses VPN connectivity to provide access to lab environments.

---

## VPN-Related Technologies Studied

My study material also introduced:

### PPP

Point-to-point communication technology associated with authentication and basic protection between two sides.

### PPTP

A tunneling technology associated with PPP.

The study material highlights that PPTP is easy to configure but its encryption is considered weak today.

### IPSec

IPSec provides protection for IP-based communication and is widely associated with secure VPN implementations.

These technologies helped me understand that **VPN is the concept of protected tunneling**, while different technologies can be involved in implementing that communication.

---

# 22. HTTP and HTTPS in the Network Flow

HTTP appeared in my networking studies because it provides a practical example of how application communication uses the underlying network.

**HTTP — HyperText Transfer Protocol** is used to request and receive web resources.

Examples include:

* HTML
* images
* CSS
* JavaScript
* other web content

**HTTPS** adds encrypted communication and stronger assurance about the server the client is communicating with.

A simplified web flow is:

```text
Browser
   ↓
DNS
   ↓
TCP
   ↓
TLS when using HTTPS
   ↓
HTTP Request
   ↓
Web Server
   ↓
HTTP Response
   ↓
Browser
```

This connects the Application Layer to the transport and network layers underneath it.

---

# 23. HTTP Requests and Responses

An HTTP request contains information such as:

* request method
* requested resource
* protocol version
* headers

Examples of methods introduced in my studies include:

* `GET`
* `POST`
* `PUT`
* `DELETE`

The server returns an HTTP response containing:

* protocol version
* status code
* headers
* response content

Examples of status code categories include:

| Range | Meaning            |
| ----- | ------------------ |
| 1xx   | Informational      |
| 2xx   | Success            |
| 3xx   | Redirection        |
| 4xx   | Client-side errors |
| 5xx   | Server-side errors |

This is important for cybersecurity because network and application investigations often require interpreting requests, responses, status codes, headers, and connection behavior together.

HTTP deserves a separate study note later, so this section only establishes its place inside the networking flow.

---

# 24. Connecting Networking Theory to Wireshark

Wireshark helped make these concepts more practical.

Instead of only reading definitions, packet analysis makes it possible to observe communication directly.

Important fields I learned to pay attention to include:

* Time
* Source
* Destination
* Protocol
* Source port
* Destination port
* TCP flags
* packet information

For TCP communication, examples include:

```text
[SYN]
[SYN, ACK]
[ACK]
[FIN]
[RST]
```

This allows theoretical concepts to become visible.

For example, instead of only memorizing:

```text
SYN → SYN/ACK → ACK
```

a packet capture can show those stages happening as network traffic.

That connection between theory and observation was one of the most useful parts of my networking studies.

---

# 25. A Complete Website Communication Mental Model

One of the most useful exercises for me has been connecting everything into one sequence.

A simplified example:

```text
1. User enters a domain name
            ↓
2. DNS resolves the domain into an IP address
            ↓
3. The destination host is identified
            ↓
4. TCP establishes a connection when TCP is used

       SYN
        ↓
     SYN/ACK
        ↓
       ACK

            ↓
5. TLS may establish protected communication
            ↓
6. HTTP sends the application request
            ↓
7. Data is encapsulated

Application Data
      ↓
Segment
      ↓
Packet
      ↓
Frame
      ↓
Bits

            ↓
8. Network infrastructure handles the traffic

Switch
Router
Firewall
Internet

            ↓
9. The server processes and returns the response
            ↓
10. Data is decapsulated

Bits
 ↓
Frame
 ↓
Packet
 ↓
Segment
 ↓
Application Data

            ↓
11. The browser receives and renders the result
```

This system-level view is more valuable to me than memorizing each technology independently.

---

# 26. Networking and Security Controls Working Together

The technologies I studied can also be viewed by their main roles:

| Concept  | Main Function                                          |
| -------- | ------------------------------------------------------ |
| OSI      | Organizes communication conceptually                   |
| TCP/IP   | Represents practical internet communication            |
| IP       | Identifies hosts in network communication              |
| MAC      | Identifies local network interfaces                    |
| Port     | Identifies applications and services                   |
| DNS      | Resolves names into IP addresses                       |
| TCP      | Reliable, ordered communication                        |
| UDP      | Lightweight communication without TCP-style guarantees |
| Router   | Connects different networks                            |
| Switch   | Connects devices inside a LAN                          |
| VLAN     | Creates logical network segmentation                   |
| Firewall | Controls network traffic                               |
| VPN      | Creates protected communication tunnels                |
| HTTP     | Carries web requests and responses                     |
| HTTPS    | Protects web communication through encryption          |

The important lesson is that these technologies are not competitors.

They perform different functions inside the same communication ecosystem.

---

# 27. Cybersecurity Relevance

Networking knowledge supports several cybersecurity activities.

Based on the concepts I have studied so far, networking helps provide context for:

* packet analysis
* traffic monitoring
* understanding firewall rules
* identifying expected and unexpected services
* reading ports and protocols
* recognizing connection behavior
* investigating DNS activity
* understanding segmentation
* interpreting web traffic
* analyzing connection failures
* recognizing abnormal TCP patterns
* troubleshooting communication

The more I understand normal network behavior, the easier it becomes to understand why unusual behavior deserves investigation.

---

# 28. What I Have Learned So Far

The biggest shift in my understanding has been moving from isolated definitions toward a connected model.

Initially, concepts such as:

* TCP
* UDP
* DNS
* ports
* routers
* switches
* firewalls
* VLANs

felt like separate subjects.

After reviewing them together and observing some of them through practical exercises, I started to see networking as a communication system.

The main ideas I currently consider foundational are:

1. **OSI provides a conceptual map.**
2. **TCP/IP represents practical internet communication.**
3. **Encapsulation explains how data moves through layers.**
4. **IP identifies hosts while ports identify services.**
5. **TCP establishes and manages reliable connections.**
6. **UDP provides lighter communication without TCP-style guarantees.**
7. **DNS connects domain names with IP destinations.**
8. **Routers connect networks.**
9. **Switches connect devices inside local networks.**
10. **VLANs create logical segmentation.**
11. **Firewalls control network traffic.**
12. **VPNs create protected communication tunnels.**
13. **HTTP and HTTPS connect application activity with the network underneath.**
14. **Packet analysis makes these concepts observable instead of purely theoretical.**

---

# 29. Current Learning Perspective

Networking is still an area I am actively developing.

This document represents the foundation I have built so far, not the end of the subject.

One of the most important lessons from this stage of my cybersecurity transition is that stronger networking knowledge makes many other cybersecurity concepts easier to understand.

Instead of trying to learn every networking topic at maximum depth immediately, my current objective is to strengthen the foundation progressively and connect each new concept with practical observation.

---

# 30. Next Steps

Future study notes can expand these topics individually:

* OSI and TCP/IP Models
* TCP Connection Lifecycle
* TCP vs UDP
* DNS Fundamentals
* HTTP and HTTPS
* Network Addressing
* Routing and Switching
* VLAN and Network Segmentation
* Firewalls
* VPN Technologies
* Wireshark Traffic Analysis

Future lab documentation can then connect these concepts with practical observation in authorized environments.

**Theory → Observation → Practice → Documentation**
