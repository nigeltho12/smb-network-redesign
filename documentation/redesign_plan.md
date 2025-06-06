# SMB Network Redesign Plan

## 📌 Objective

This project simulates the redesign of a small-to-medium business (SMB) network to improve segmentation, security, and scalability. The primary goals were to implement departmental VLANs, enable inter-VLAN routing using a router-on-a-stick setup, and apply access control lists (ACLs) to isolate untrusted devices such as guest clients.

---

## 🗺️ Network Topology Summary

- **Devices Used:**
  - 2 Cisco 2960 switches
  - 1 Cisco 2900 router
  - 1 Wireless router (for extended access)
  - 6 PCs across 4 departments
  - 1 internal server

- **Interconnects:**
  - Trunking between switches and to the router (G0/0)
  - End devices connected to access ports per VLAN
  - Server placed in Admin VLAN for central access

---

## 🧱 VLAN Breakdown

| VLAN | Name     | Subnet             | Ports Assigned        | Purpose              |
|------|----------|--------------------|------------------------|----------------------|
| 10   | Admin    | 192.168.10.0/24    | Fa0/2–5                | Management/Admin PCs |
| 20   | Finance  | 192.168.20.0/24    | Fa0/7–10               | Accounting Team      |
| 30   | HR       | 192.168.30.0/24    | Fa0/2–3 (Switch 2)     | HR Staff             |
| 40   | Guest    | 192.168.40.0/24    | Fa0/4–6 (Switch 2)     | Untrusted Devices    |

Each VLAN was configured on both switches, with access ports mapped accordingly.

---

## 🔄 Inter-VLAN Routing

A **router-on-a-stick** configuration was used to enable inter-VLAN communication. The router interface `G0/0` was sub-interfaced for each VLAN with the following configuration:

```plaintext
G0/0.10 → 192.168.10.1/24 (VLAN 10)
G0/0.20 → 192.168.20.1/24 (VLAN 20)
G0/0.30 → 192.168.30.1/24 (VLAN 30)
G0/0.40 → 192.168.40.1/24 (VLAN 40)
