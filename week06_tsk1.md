# Week 06: ARP (Address Resolution Protocol)

## Task 1: Resolving IP Addresses to Hardware Addresses

---

## Student Details
- Student ID: 12313659

---

## Aim
The aim of this task is to observe how ARP (Address Resolution Protocol) is used to map IP addresses to MAC (hardware) addresses within a Local Area Network (LAN).

---

## Network Setup

The network consists of:
- 4 Linux Hosts (PC1, PC2, PC3, PC4)
- 1 Ethernet Switch

All hosts are connected to the same switch and belong to the same subnet.

---

## IP Address Configuration

| Device | IP Address | Subnet Mask |
|--------|------------|-------------|
| PC1 (Host A) | 10.1.1.1 | 255.255.255.0 |
| PC2 (Host B) | 10.1.1.2 | 255.255.255.0 |
| PC3 (Host C) | 10.1.1.3 | 255.255.255.0 |
| PC4 (Host D) | 10.1.1.4 | 255.255.255.0 |

---

## Step 1: Viewing Initial ARP Table

On PC1 (Host A), the ARP table was checked before any communication.

### Command:
```bash
ip neigh show
```

### Observation:
- The ARP table was empty or contained very few entries.
- No MAC address mappings were present for other hosts.

### Screenshot:
![ARP Before](screenshots/arp-before.png)

---

## Step 2: Ping from PC1 to PC2

A ping was initiated from PC1 to PC2 to trigger ARP resolution.

### Command:
```bash
ping 10.1.1.2
```

### Observation:
- Communication between PC1 and PC2 was successful.
- ARP protocol was used internally to resolve the MAC address.

---

## Step 3: Viewing ARP Table After Ping

The ARP table on PC1 was checked again.

### Command:
```bash
ip neigh show
```

### Observation:
- A new entry was added in the ARP table.
- The entry contained:
  - IP address of PC2
  - Corresponding MAC address
  - State: REACHABLE

### Screenshot:
![ARP After Ping](screenshots/arp-after.png)

---

## Step 4: Ping from PC3 to PC1

A ping was initiated from PC3 to PC1.

### Command:
```bash
ping 10.1.1.1
```

### Observation:
- PC3 successfully communicated with PC1.
- This triggered additional ARP entries.

---

## Step 5: Final ARP Table Analysis

The ARP table on PC1 was checked once again.

### Command:
```bash
ip neigh show
```

### Observation:
- Additional entries were visible.
- Multiple IP-to-MAC mappings were present.
- The table dynamically updated as communication occurred.

### Screenshot:
![ARP Final](screenshots/arp-final.png)

---

## Results and Analysis

- Initially, the ARP table was empty as no communication had occurred.
- After pinging another host, the ARP table was updated with MAC address mappings.
- Further communication resulted in additional entries.
- The ARP table dynamically learns and updates entries based on network activity.

This demonstrates that ARP works automatically in the background to resolve IP addresses into hardware addresses required for communication.

---

## Conclusion

In this task, ARP functionality was successfully observed. It was demonstrated that devices do not initially know the MAC address of other devices and must use ARP to discover it. Once communication occurs, the ARP table is updated dynamically with IP-to-MAC mappings. This process is essential for communication within a LAN, ensuring that packets are delivered to the correct hardware destination.
