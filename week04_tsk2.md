# Week 04 – Task 2: Dynamic Routing with OSPF

## Student Name: Thrinadh  
## Student ID: 12313659  

---

## 1. Aim  
The aim of this task is to observe how dynamic routing using OSPF is configured and how it automatically adapts to changes in the network. This includes analysing routing tables, identifying neighbour relationships, and verifying how traffic is redirected when a link fails.

---

## 2. Network Topology  
The network consists of two hosts and four routers configured with OSPF. The topology provides two possible paths between the hosts, allowing dynamic routing decisions.

- Path 1: FRR1 → FRR2 → FRR4  
- Path 2: FRR1 → FRR2 → FRR3 → FRR4  

### Screenshot  
![Network Topology](screenshots/Week04%20network%20topology1.png)

---

## 3. Project Setup  

The OSPF template project was used and duplicated as:

```
OSPF-Basics-12313659.gns3project
```

All nodes were started in GNS3, and sufficient time was given for the routers to fully boot. Each router displayed the `frr#` prompt, confirming that FRRouting (FRR) was running successfully.

No manual configuration of IP addresses or OSPF was required, as the template network was preconfigured.

---

## 4. Viewing OSPF Routing Information  

The FRR CLI was accessed using:

```bash
vtysh
```

The following commands were used to analyse routing behaviour:

### 4.1 OSPF Neighbours  

```bash
show ip ospf neighbor
```

This command displayed:
- Neighbour router IP addresses  
- Adjacency states (FULL)  
- Interface connections  

This confirms that OSPF neighbour relationships were successfully established.

---

### 4.2 OSPF Routes  

```bash
show ip ospf route
```

This command showed:
- Networks learned dynamically via OSPF  
- Route metrics (cost)  
- Next-hop routers  

---

### 4.3 Routing Table  

```bash
show ip route
```

This displayed:
- Complete routing table  
- OSPF routes marked with "O"  
- Directly connected networks  

---

## 5. Routing Table Analysis  

The routing tables showed that routers dynamically learned paths to remote networks.

### Summary Table  

| Router | Destination Network | Next Hop |
|--------|-------------------|----------|
| FRR1 | Host2 Network | FRR2 |
| FRR2 | Host2 Network | FRR4 |
| FRR3 | Host1 Network | FRR2 |
| FRR4 | Host1 Network | FRR2 |

This confirms that OSPF selects the best available path based on network topology and metrics.

---

## 6. Path Testing (Before Failure)  

Traceroute was executed from Host1 to Host2:

```bash
traceroute 10.0.6.2
```

### Observed Path  

```
Host1 → FRR1 → FRR2 → FRR4 → Host2
```

This indicates that OSPF selected the shortest or lowest-cost path (top path).



---

## 7. Link Failure Simulation  

To test OSPF adaptability, the link between FRR2 and FRR4 was disconnected by stopping the corresponding NETem node. This removed the primary path between the hosts.

---

## 8. Path Testing (After Failure)  

Traceroute was executed again:

```bash
traceroute 10.0.6.2
```

### Observed Path  

```
Host1 → FRR1 → FRR2 → FRR3 → FRR4 → Host2
```

This demonstrates that:
- OSPF detected the link failure  
- Routing tables were updated automatically  
- Traffic was redirected through the alternate path  

---

## 9. Observations  

- OSPF automatically discovers neighbouring routers  
- Routing tables are dynamically updated  
- Multiple paths exist between source and destination  
- Failover occurs without manual intervention  
- Network communication remains uninterrupted after link failure  

---

## 10. Conclusion  

This task successfully demonstrated the working of dynamic routing using OSPF. The routers dynamically learned routes and selected the most efficient path for communication. When a link failure occurred, OSPF automatically recalculated routes and redirected traffic through an alternate path. This highlights the importance of dynamic routing protocols in maintaining reliable and efficient network communication.

---

## 11. Outputs  

- Exported project file:  
  `OSPF-Basics-12313659.gns3project`  

- OSPF neighbour output:  
  `show ip ospf neighbor`  

- Routing table outputs:  
  `show ip route`, `show ip ospf route`  

- Traceroute outputs:  
  Before and after link failure  

---

## Final Status  

All required components of Task 2 have been completed successfully:
- OSPF routing observed ✔  
- Routing tables analysed ✔  
- Traceroute tested ✔  
- Failover demonstrated ✔  
