# Wide-Area-Network-construction
# Cisco Packet Tracer: Interconnected LANs via Routers

## Overview

This project demonstrates the setup of two Local Area Networks (LANs) — **Office LAN A** and **Office LAN B** — and their interconnection to form a Wide Area Network (WAN) using routers.

---

## Network Design Summary

- **2 LANs**: Office LAN A and Office LAN B
- **Each LAN contains**:
  - 1 Router
  - 1 Switch
  - 2 PCs

---

## Devices Needed

- **2 Routers**: Cisco 2811
- **2 Switches**: Cisco 2960
- **4 PCs**: PC-PT

### Cables
- **Switch to PC**: Copper Straight-through (different devices)
- **Router to Router**: Copper Crossover (same devices)
- **Switch to Router**: Copper Straight-through

---

## Physical Connections

### LAN A (Left Side)
- PC0 → Switch0 (FastEthernet, Straight-through)
- PC1 → Switch0 (FastEthernet, Straight-through)
- Switch0 → Router0 Fa0/0 (FastEthernet, Straight-through)

### LAN B (Right Side)
- PC2 → Switch1 (FastEthernet, Straight-through)
- PC3 → Switch1 (FastEthernet, Straight-through)
- Switch1 → Router1 Fa0/0 (FastEthernet, Straight-through)

### WAN (Connecting Routers)
- Router0 Fa0/1 ↔ Router1 Fa0/1 (FastEthernet, Crossover)

---

## IP Addressing Plan

| Device     | Interface   | IP Address     | Subnet Mask       |
|------------|-------------|----------------|-------------------|
| PC0        | —           | 192.168.1.10   | 255.255.255.0     |
| PC1        | —           | 192.168.1.11   | 255.255.255.0     |
| Router0    | Fa0/0       | 192.168.1.1    | 255.255.255.0     |
| Router0    | Fa0/1       | 10.0.0.1       | 255.255.255.0     |
| Router1    | Fa0/1       | 10.0.0.2       | 255.255.255.0     |
| Router1    | Fa0/0       | 192.168.2.1    | 255.255.255.0     |
| PC2        | —           | 192.168.2.10   | 255.255.255.0     |
| PC3        | —           | 192.168.2.11   | 255.255.255.0     |

---

## Step-by-Step Configuration

### Step 1: Assign IP Addresses to PCs

For each PC:
**Desktop > IP Configuration**

**PC0**  
- IP Address: `192.168.1.10`  
- Subnet Mask: `255.255.255.0`  
- Default Gateway: `192.168.1.1`  

**PC1**  
- IP Address: `192.168.1.11`  
- Gateway: `192.168.1.1`

**PC2**  
- IP Address: `192.168.2.10`  
- Gateway: `192.168.2.1`

**PC3**  
- IP Address: `192.168.2.11`  
- Gateway: `192.168.2.1`

---

### Step 2: Configure Router0

```plaintext
Router> enable
Router# configure terminal
Router(config)# interface FastEthernet0/0
Router(config-if)# ip address 192.168.1.1 255.255.255.0
Router(config-if)# no shutdown
Router(config-if)# exit

Router(config)# interface FastEthernet0/1
Router(config-if)# ip address 10.0.0.1 255.255.255.0
Router(config-if)# no shutdown
Router(config-if)# exit

Router(config)# exit
Router# write
```
---
### Step 3: Configure Router1

```plaintext
Router> enable
Router# configure terminal
Router(config)# interface FastEthernet0/0
Router(config-if)# ip address 192.168.2.1 255.255.255.0
Router(config-if)# no shutdown
Router(config-if)# exit

Router(config)# interface FastEthernet0/1
Router(config-if)# ip address 10.0.0.2 255.255.255.0
Router(config-if)# no shutdown
Router(config-if)# exit

Router(config)# exit
Router# write
```
---
### Step 4: Add Static Routes
On Router0:
```plaintext
Router# configure terminal
Router(config)# ip route 192.168.2.0 255.255.255.0 10.0.0.2
Router(config)# exit
```
On Router1:
```plaintext
Router# configure terminal
Router(config)# ip route 192.168.1.0 255.255.255.0 10.0.0.1
Router(config)# exit
```
---
### Step 5: Test Connectivity
Go to PC0
Desktop > Command Prompt
```plaintext
ping 192.168.1.11      // Test LAN
ping 192.168.2.10      // Test across WAN
```
Also test:
From PC3 to PC0
Between all PCs

---
### Expected Outcome
 - All PCs can ping each other across LANs and WAN
 - Routers correctly route packets between networks
 - Basic routing and LAN setup validated
