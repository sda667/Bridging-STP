# Bridging and Spanning Tree Protocol

This project analyzes the behavior of Ethernet switches and the operation of the **Spanning Tree Protocol (STP)**.  
It explores frame filtering, ARP exchanges, MAC learning, and loop prevention in redundant switch topologies.

## Project Structure
- `Bridging_and_spanning_tree_protocol.pdf` → Full detailed lab report (LaTeX formatted with figures)
- `Bridging_and_spanning_tree_protocol.md` → Markdown version of the report for quick reading on GitHub

## Tools
- Wireshark
- Ethernet switches (simulated)
- ARP, BPDU analysis

## Key Results
- Switches dynamically learn MAC addresses for efficient frame forwarding.  
- STP prevents loops by electing a root bridge and blocking redundant ports.  
- Convergence after failures takes 30–50s, slower than RSTP.  
- Comparison with RIP highlights differences between Layer 2 and Layer 3 path management.  

## Report Access
- [Full Report (PDF)](Bridging_and_spanning_tree_protocol.pdf)
- [Full Report (Markdown)](Bridging_and_spanning_tree_protocol.md)
