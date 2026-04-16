# Week 06: Address Resolution Protocol (ARP)

## Task 1: Resolving IP Addresses to Hardware Addresses

---

## Student Details
- Student ID: 12313659

---

# 1. Aim

The aim of this task is to analyse how the Address Resolution Protocol (ARP) resolves IP addresses into corresponding hardware (MAC) addresses within a Local Area Network (LAN), and to observe how the ARP table dynamically updates during communication between devices.

---

# 2. Network Configuration

A simple LAN was created using GNS3 consisting of four Linux hosts connected through a single Ethernet switch. All hosts were configured within the same subnet to enable direct communication.

## Network Components:
- 4 × Linux Hosts (PC1, PC2, PC3, PC4)
- 1 × Ethernet Switch

---

# 3. IP Addressing Scheme

| Device | Role   | IP Address | Subnet Mask |
|--------|--------|------------|-------------|
| PC1    | Host A | 10.1.1.1   | 255.255.255.0 |
| PC2    | Host B | 10.1.1.2   | 255.255.255.0 |
| PC3    | Host C | 10.1.1.3   | 255.255.255.0 |
| PC4    | Host D | 10.1.1.4   | 255.255.255.0 |

---

# 4. Initial ARP Table Analysis

Before any communication, the ARP table of PC1 (Host A) was examined.

## Command Used:
```bash
ip neigh show
```

## Observation:
- The ARP table was initially empty or contained minimal entries.
- No mapping existed between IP addresses and MAC addresses of other hosts.
- This indicates that no prior communication had occurred within the network.


---

# 5. ARP Trigger through Communication

To initiate ARP resolution, a ping request was sent from PC1 to PC2.

## Command Used:
```bash
ping 10.1.1.2
```

## Observation:
- The ping was successful, confirming connectivity between hosts.
- During this process, ARP was automatically triggered to resolve the MAC address of PC2.
- The system broadcasted an ARP request and received a reply from PC2.

---

# 6. ARP Table After First Communication

After the ping operation, the ARP table was checked again on PC1.

## Command Used:
```bash
ip neigh show
```

## Observation:
- A new entry appeared in the ARP table.
- The entry contained:
  - IP address of PC2 (10.1.1.2)
  - Corresponding MAC address
  - State: REACHABLE
- This confirms that ARP successfully resolved the hardware address.

 

---

# 7. Additional Communication Scenario

To further analyse ARP behaviour, a ping was initiated from PC3 (Host C) to PC1.

## Command Used:
```bash
ping 10.1.1.1
```

## Observation:
- The communication was successful.
- Additional ARP interactions occurred within the network.

---

# 8. Final ARP Table Analysis

The ARP table on PC1 was examined once more.

## Command Used:
```bash
ip neigh show
```

## Observation:
- Multiple entries were now present in the ARP table.
- Each entry mapped an IP address to a corresponding MAC address.
- Entries displayed states such as:
  - REACHABLE
  - STALE (in some cases over time)
- This demonstrates that ARP dynamically updates based on network interactions.



---

# 9. Results and Analysis

The experiment clearly demonstrates the dynamic behaviour of ARP within a LAN environment:

- Initially, the ARP table contained no mappings due to lack of communication.
- After initiating communication (ping), ARP resolved IP addresses into MAC addresses.
- The ARP table was updated with new entries containing IP-to-MAC mappings.
- Further communication resulted in additional entries being added.
- ARP entries change over time based on usage and network activity.

This confirms that ARP operates automatically in the background and is essential for enabling communication at the data link layer.

---

# 10. Conclusion

In conclusion, the Address Resolution Protocol plays a critical role in network communication by mapping logical IP addresses to physical MAC addresses. The experiment demonstrated that ARP entries are created dynamically when communication occurs and are maintained temporarily in the ARP table. This process ensures that data packets are delivered to the correct hardware destination within a LAN. The observations confirm that ARP is an essential mechanism for efficient and accurate communication in networked systems.
