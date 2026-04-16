# Week 05 Tutorial – Task 1: VLAN Configuration using Open vSwitch

## Student Details
- Name: Thrinadh  
- Student ID: 12313659  

---

## Aim
To configure VLANs on an Open vSwitch and verify network segmentation by testing communication between hosts.

---

## Network Topology
The network consists of four PCs connected to a single Open vSwitch.

![Topology](topology.png)

---

## IP Configuration

Each PC was assigned an IP address in the same subnet.

| PC  | IP Address   | Subnet Mask     |
|-----|-------------|-----------------|
| PC1 | 10.1.1.1    | 255.255.255.0   |
| PC2 | 10.1.1.2    | 255.255.255.0   |
| PC3 | 10.1.1.3    | 255.255.255.0   |
| PC4 | 10.1.1.4    | 255.255.255.0   |

### Commands Used
```bash
sudo ifconfig eth0 10.1.1.1 netmask 255.255.255.0 up
sudo ifconfig eth0 10.1.1.2 netmask 255.255.255.0 up
sudo ifconfig eth0 10.1.1.3 netmask 255.255.255.0 up
sudo ifconfig eth0 10.1.1.4 netmask 255.255.255.0 up
```

---

## VLAN Configuration

Based on the setup:

- VLAN 659 → PC1, PC2  
- VLAN 660 → PC3, PC4  

### Commands Executed on Switch
```bash
ovs-vsctl set port eth1 tag=659
ovs-vsctl set port eth2 tag=659
ovs-vsctl set port eth3 tag=660
ovs-vsctl set port eth4 tag=660
```

---

## VLAN Verification

### Command
```bash
ovs-vsctl show
```

### Output Screenshot
![Switch Configuration](switch-config.png)

### Observation
- eth1 and eth2 are assigned to VLAN 659  
- eth3 and eth4 are assigned to VLAN 660  

---

## Connectivity Testing

### Test 1: Same VLAN Communication

Test between PC1 and PC2.

![Same VLAN Test](same-vlan.png)

**Result:**  
Communication should be successful within the same VLAN.

---

### Test 2: Different VLAN Communication

Test between PC1 and PC3.

![Different VLAN Test](different-vlan.png)

**Result:**  
Communication failed (100% packet loss), showing VLAN isolation.

---

## Analysis
The VLAN configuration successfully separated the network into two logical groups. Even though all PCs are connected to the same physical switch and share the same subnet, communication is restricted based on VLAN membership.

- Same VLAN → Communication allowed  
- Different VLAN → Communication blocked  

This demonstrates proper VLAN segmentation and traffic isolation.

---

## Conclusion
VLANs were successfully implemented using Open vSwitch. PCs in VLAN 659 communicated with each other, and PCs in VLAN 660 communicated with each other. However, communication between VLANs was not possible, confirming correct VLAN configuration and network segmentation.

---
 
