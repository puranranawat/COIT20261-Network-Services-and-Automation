# Week 04 – Viewing Routing Tables and Inter-Subnet Communication

## Student Name: Thrinadh

## Student ID: 12313659

---

# 1. Introduction

This task focuses on understanding how routing works in a network where multiple subnets are involved. In earlier tasks, all devices were placed within a single subnet, allowing direct communication through switching. However, in real-world networks, devices are often divided into multiple subnets, requiring a router to enable communication between them.

The aim of this experiment is to configure a network consisting of multiple Linux hosts and a router using GNS3, assign appropriate IP addresses, configure default gateways, enable packet forwarding on the router, and analyze routing tables. Additionally, connectivity between different subnets is verified using ICMP-based ping tests.

This task demonstrates core networking concepts such as Layer 3 routing, gateway functionality, routing tables, and packet forwarding, which are fundamental in modern network design.

---

# 2. Network Topology Design

The network topology consists of three Linux-based end devices (PC1, PC2, and PC3), one router, and one Ethernet switch. The design intentionally separates devices into two different subnets to demonstrate routing.

## Topology Structure

* PC1 and PC2 are connected to an Ethernet switch
* The switch is connected to one interface of the router (Subnet A)
* PC3 is directly connected to another interface of the router (Subnet B)

**Screenshot: Network Topology**
![Network Topology](screenshots/Week04%20network%20topology.png)

**Explanation:**
This topology creates two isolated broadcast domains (subnets). Devices within the same subnet can communicate directly, while communication between subnets must pass through the router. The router acts as an intermediary device that examines IP packets and forwards them to the correct destination network.

---

# 3. IP Addressing Scheme

To implement subnet-based communication, two separate IPv4 networks were used.

## Subnet A (10.1.1.0/24)

| Device        | IP Address | Subnet Mask   | Gateway    |
| ------------- | ---------- | ------------- | ---------- |
| PC1           | 10.1.1.1   | 255.255.255.0 | 10.1.1.254 |
| PC2           | 10.1.1.2   | 255.255.255.0 | 10.1.1.254 |
| Router (eth0) | 10.1.1.254 | 255.255.255.0 | -          |

## Subnet B (10.1.2.0/24)

| Device        | IP Address | Subnet Mask   | Gateway    |
| ------------- | ---------- | ------------- | ---------- |
| PC3           | 10.1.2.1   | 255.255.255.0 | 10.1.2.254 |
| Router (eth1) | 10.1.2.254 | 255.255.255.0 | -          |

**Explanation:**
Each subnet has its own unique network address, ensuring proper segmentation. Hosts within a subnet use the router’s interface IP as their default gateway to communicate with external networks. This setup ensures that packets destined for another subnet are forwarded to the router.

---

# 4. Router Configuration

The router was configured with two interfaces, each connected to a different subnet. IP forwarding was enabled so that the router can transfer packets between networks.

### Commands Used:

```bash
sudo ifconfig eth0 10.1.1.254 netmask 255.255.255.0 up
sudo ifconfig eth1 10.1.2.254 netmask 255.255.255.0 up
sudo sysctl -w net.ipv4.ip_forward=1
```

**Screenshot: Router Configuration**
![Router](screenshots/Configure%20Router.png)

**Explanation:**
The router acts as a Layer 3 device. Each interface belongs to a different subnet, and enabling IP forwarding allows the router to examine incoming packets and forward them to the appropriate network. Without enabling forwarding, communication between subnets would fail.

---

# 5. Host Configuration

Each host was configured with a static IP address and a default gateway pointing to the router.

### Example (PC1):

```bash
sudo ifconfig eth0 10.1.1.1 netmask 255.255.255.0 up
sudo route add default gw 10.1.1.254
```

**Screenshot: PC1 Configuration**
![PC1](screenshots/PC1%20configuration.png)

**Screenshot: PC2 Configuration**
![PC2](screenshots/PC2%20configuration.png)

**Screenshot: PC3 Configuration**
![PC3](screenshots/PC3%20configuration.png)

**Explanation:**
The default gateway is critical for inter-network communication. When a host needs to send data to a device outside its subnet, it forwards the packet to the router. The router then determines the correct destination network and forwards the packet accordingly.

---

# 6. Routing Table Analysis

Routing tables were examined using the following command:

```bash
route -n
```

Each host routing table includes:

* A direct route to its own subnet
* A default route pointing to the router

The router’s routing table includes:

* Routes for both connected subnets
* Directly connected network entries

**Explanation:**
Routing tables define how packets are forwarded. When a packet arrives at a host or router, the routing table is consulted to determine the next hop. If the destination is outside the local subnet, the packet is sent to the default gateway.

---

# 7. Connectivity Testing

Connectivity between subnets was verified using the ping command.

```bash
ping 10.1.2.1 -c 5
```

**Screenshot: Ping Test Result**
![Ping](screenshots/Ping%20Test.png)

**Explanation:**
The successful ping confirms that PC1 (Subnet A) can communicate with PC3 (Subnet B). The result shows:

* 0% packet loss → reliable communication
* TTL decreased from 64 to 63 → packet passed through one router

This validates that routing is functioning correctly.

---

# 8. Results and Analysis

The results of the experiment confirm that:

* Devices within the same subnet communicate directly
* Devices in different subnets require a router
* The router successfully forwards packets between networks
* Routing tables correctly guide packet transmission
* No packet loss occurred during testing
* TTL analysis confirms that packets traverse the router

This demonstrates the correct implementation of Layer 3 routing and validates the importance of default gateways and routing tables in network communication.

---

# 9. Conclusion

In conclusion, the experiment successfully demonstrated how routing enables communication between different subnets. By configuring IP addresses, assigning default gateways, and enabling forwarding on the router, seamless communication was achieved between devices in separate networks.

The routing tables played a crucial role in determining packet paths, while the router ensured correct delivery across subnets. This task reinforces fundamental networking principles such as routing, packet forwarding, and subnet communication, which are essential in real-world network environments.

---

 
