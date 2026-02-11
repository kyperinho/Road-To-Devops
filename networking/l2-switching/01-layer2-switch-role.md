# CCNA / Networking Notes — Layer 2 Switch Fundamentals (Day X)

**Date:** 2026-01-XX  
**Module:** *Understand the Role of a Layer 2 Switch* (Keith Barker, ~1h 16m)  
**Goal:** Understand how an L2 switch forwards Ethernet frames and how MAC address learning works.

---

## 1) What is a Layer 2 switch (L2)?

A **Layer 2 (Ethernet) switch** is a network device that makes forwarding decisions based on the:

- ✅ **Destination MAC address** in the **Layer 2 Ethernet header**

In practice, an L2 switch:
- receives Ethernet frames on one port
- learns where source MAC addresses live (which port)
- forwards frames out the correct port(s) based on the destination MAC

---

## 2) How an L2 switch forwards frames (high-level flow)

1. A frame enters a switch port.
2. The switch reads the **source MAC address** and updates its **MAC Address Table** (CAM table):
   - `source MAC → incoming switch port`
3. The switch checks the **destination MAC**:
   - If known (exists in MAC table): forward only to the matching port (**unicast forwarding**).
   - If unknown: flood frame to all ports in the VLAN except the incoming port (**unknown unicast flooding**).
   - If broadcast (`ff:ff:ff:ff:ff:ff`): flood to all ports in the VLAN (**broadcast**).

Key idea: **switches learn from source MAC addresses**, not destination.

---

## 3) Why hosts and switches need MAC learning

### Hosts (IPv4)
To send traffic inside the same LAN, a host needs a destination MAC address.

Hosts learn MAC addresses via:
- ✅ **ARP (Address Resolution Protocol)**

Example:
- Host wants to send traffic to its **default gateway**
- It uses **ARP** to discover the gateway’s MAC address

### Routers (Cisco IOS)
Routers also use ARP on Ethernet interfaces.

Useful IOS command:
- `show arp` → view ARP cache

---

## 4) MAC Address Table (CAM table)

A switch builds a MAC table by:
- ✅ listening to incoming frames and reading the **source MAC address**

To view the MAC table on Cisco switches:
- `show mac address-table`

To clear dynamically learned MAC entries:
- `clear mac address-table dynamic`

**What causes the MAC table to populate/update?**  
Any traffic that generates Ethernet frames (ARP, DNS, HTTP, ICMP/ping, etc.) can update it — because the switch learns from frames entering ports.

---

## 5) Port types: copper vs fiber

Switch ports come in different physical types. For flexible media choice:
- ✅ **SFP** ports allow fiber or copper (depending on the inserted transceiver/module)

---

## 6) Quick “exam-style” takeaways (the ones I want to remember)

- L2 switch forwards using **destination MAC (L2 header)**
- L2 switch learns by inspecting **source MAC (incoming frame)**
- Host learns MAC for gateway using **ARP**
- Cisco router ARP cache: `show arp`
- Switch MAC table: `show mac address-table`
- Clear dynamic entries: `clear mac address-table dynamic`

---

## 7) Lab plan (hands-on)

**Purpose:** validate MAC learning behavior.

### Commands used
**Windows host**
- `ipconfig /all`
- `ping A.B.C.D`

**Cisco switch**
- `clear mac address-table dynamic`
- `show mac address-table`

### What I expect to observe
1) After clearing the table, it should be mostly empty (dynamic entries removed).  
2) After generating traffic (e.g., ping), the switch should learn the host MAC(s) and populate the table.

---

## 8) Next step
- Repeat lab and document:
  - screenshot/output of `show mac address-table` before and after traffic
  - short explanation of what changed and why (source MAC learning)
- Move on to: VLANs, trunking (802.1Q), STP basics
