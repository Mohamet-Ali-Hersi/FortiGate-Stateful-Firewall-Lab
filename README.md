# FortiGate Firewall - Stateful Access Control Lab

This repository contains a practical network security lab implemented in **EVE-NG** to demonstrate network segmentation and stateful packet inspection using a **FortiGate Firewall (v7.0.9)**.

## 🎯 Topology & Objectives
The goal of this lab is to establish a secure one-way communication channel between two corporate zones:
*   **Network A (Marketing Zone):** `10.10.10.0/24` - Gateway: `10.10.10.1`
*   **Network B (Customer Zone):** `172.16.1.0/24` - Gateway: `172.16.1.1`

### Requirements:
1.  **Marketing to Customer:** Users in the Marketing network must be able to initiate connections and reach resources in the Customer network.
2.  **Customer to Marketing:** Users in the Customer network should **only** be allowed to return reply traffic. Direct connections initiated from the Customer zone back to Marketing must be dropped.

---

## 🛠️ Implementation Details

### 1. Firewall Policy Configuration
A single firewall policy was created to enforce this specific behavior:
*   **Incoming Interface:** `LAN-A (port2)`
*   **Outgoing Interface:** `LAN-B (port3)`
*   **Source / Destination:** `all` / `all`
*   **Service:** `ALL`
*   **Action:** `ACCEPT`
*   **NAT:** Disabled

Because FortiGate is a **Stateful Firewall**, it automatically tracks the state of active network connections in its session table. When PC1 sends an ICMP request, the firewall records the session, allowing PC2's response (Reply) to return automatically without requiring a reverse policy.

### 2. Troubleshooting: Host-Level Security
During validation, initial pings from Network A to Network B timed out despite correct routing and firewall policies. 
*   **Cause:** The destination Windows host (PC2) had its default **Windows Defender Firewall** enabled, which blocks incoming ICMP echo requests from non-local subnets.
*   **Resolution:** Enabled the inbound rule `File and Printer Sharing (Echo Request - ICMPv4-In)` within Windows Advanced Security on PC2 to allow cross-subnet pings.

---

## 📸 Lab Topology Screenshot
*(Upload your screenshot to this repository and link it here)*
![Network Topology](Screenshot%20from%202026-06-09%2012-58-15.png)
