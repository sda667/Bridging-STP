# Bridging and Spanning Tree Protocol  
**Author:** Ali Bahja  
**Date:** 13 December 2024  

---

## Objective
This lab studies the behavior of Ethernet switches at the data link layer, focusing on frame filtering, **ARP exchanges**, and **MAC learning**.  
It also analyzes the **Spanning Tree Protocol (STP)**, which prevents switching loops by creating a loop-free logical topology.  
Finally, similarities and differences between STP and RIP are discussed.

---

## Topology and Setup
- Multiple hosts (pc1, pc2, pc3) connected via two Ethernet switches.  
- Additional experiments with four interconnected bridges (bridge1–bridge4) with redundant links to study STP behavior.  
- **Wireshark** captures used to analyze ARP and BPDU traffic.  


---

## Procedure and Results

### Frame Filtering and ARP Behavior
- First RTT between pc2 → pc3 significantly longer (11 ms vs 0.5 ms on average).  
- Delay caused by ARP resolution: pc2 broadcasts an ARP request before sending the first ICMP Echo Request.  
- Once cached, subsequent pings are immediate with lower RTT.  
<img width="1372" height="557" alt="ARP_request_pc2" src="https://github.com/user-attachments/assets/9a270a93-9ea7-452c-9848-8b6f98637b6c" />

**Figure: ARP request broadcast from pc2** 

<img width="1294" height="293" alt="ARP_reply_pc3" src="https://github.com/user-attachments/assets/a4f828c9-6812-4c78-9093-36f499f7299d" />

**Figure: ARP reply from pc3**


---

### Broadcast Visibility
- pc1 also sees ARP requests from pc2.  
- ARP requests are broadcasts → all hosts in the collision domain receive them.  

---

### Switch MAC Learning
Switches dynamically learn MAC addresses during ARP exchange:  
- Switch1 learns MAC of pc2 from ARP request.  
- Switch2 learns MAC of pc2, and later MAC of pc3 from its reply.  

This builds forwarding tables for efficient frame delivery.  

---

### Switch Tranp`arency and TTL
- Switches operate at Layer 2 and do not modify IP headers.  
- TTL remains unchanged when passing through switches.  
- Only routers decrement TTL.  

---

### IP Addresses on Switches
- Switches do not require IP addresses for forwarding.  
- They operate at Layer 2 using MAC addresses.  
- Routers (Layer 3) require IP addresses to make routing decisions.  

---

### Spanning Tree Protocol (STP)
- STP eliminates loops in redundant switch topologies.  
- Root bridge elected: bridge1 (lowest bridge ID, priority 80-00).  
- Other bridges select root ports (lowest-cost path to root):  
  - bridge2 → direct link to bridge1 (segment A)  
  - bridge4 → direct link to bridge1 (segment B)  
  - bridge3 → can reach bridge1 via bridge2 or bridge4; STP breaks tie and selects one  

On each link, STP designates one forwarding port.  
Root bridge ports are always designated. Some redundant ports are blocked to prevent loops.  
<img width="500" height="436" alt="STP_topology" src="https://github.com/user-attachments/assets/af70c283-365d-4dd6-bdb3-036eed977105" />

**Figure: STP topology with root and designated ports**

---

### Traffic Flow
- Frame from host1 → host2 passes through bridge4 → bridge3 → host2.  
- Blocked ports prevent loops.  

---

### Failure and Recalculation
- When bridge2 fails, STP requires ~30–50s to recalculate topology.  
- Other bridges detect missing BPDUs and recompute roles.  
- bridge1 remains root; blocked ports (e.g., bridge3–bridge4 link) move through Listening → Learning → Forwarding states.  
- Connectivity restored after convergence.  

---

### Comparison of RIP and STP
- **STP**: Layer 2 protocol, prevents loops by blocking redundant links, ensures one active path.  
- **RIP**: Layer 3 protocol, computes shortest paths using hop counts, supports multiple routes, updates periodically.  

---

## Analysis
- Switches learn MAC addresses dynamically.  
- ARP explains longer RTT for first ping.  
- STP is essential in redundant networks for loop prevention, though convergence is slower than modern protocols (e.g., RSTP).  
- Comparison with RIP highlights different roles of Layer 2 vs Layer 3 protocols in path selection and redundancy management.  

---

## Conclusion
- Demonstrated frame filtering, ARP exchanges, and MAC learning at the data link layer.  
- Explained STP operation: root bridge election, port roles, failure recovery.  
- Compared STP vs RIP to show differences in loop prevention and routing optimization across network layers.  
