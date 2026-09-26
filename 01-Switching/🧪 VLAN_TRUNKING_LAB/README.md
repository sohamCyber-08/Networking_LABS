# 🧪 VLAN_TRUNKING_LAB

## 🎯 Objective

Build a multi-switch LAN using VLANs and 802.1Q trunking to understand VLAN segmentation, access ports, trunk links, VLAN communication, and Layer 2 isolation.

<br>

## 🖥️ Topology

<img width="1826" height="857" alt="VLAN Trunking Topology" src="https://github.com/user-attachments/assets/889d2d16-f679-4477-a5ba-f17de4012a53" />

<br><br>

## 🌐 IP Addressing

| Device           | VLAN | IP Address | Subnet Mask   |
| ---------------- | ---: | ---------- | ------------- |
| Cyber_EMPLOYER_1 |   10 | 10.1.1.1   | 255.255.255.0 |
| Cyber_EMPLOYER_2 |   10 | 10.1.1.2   | 255.255.255.0 |
| IT_EMPLOYER_1    |   20 | 10.2.2.1   | 255.255.255.0 |
| IT_EMPLOYER_2    |   20 | 10.2.2.2   | 255.255.255.0 |

<br>

## 🔧 Technologies

* EVE-NG
* Cisco IOS
* VPCS
* IPv4
* VLAN
* Ethernet
* IEEE 802.1Q
* ICMP
* MAC Address Table

<br>

## ⚙️ Configuration & Verification

### 🔹 VLAN Configuration

<img width="1817" height="898" alt="VLAN Configuration" src="https://github.com/user-attachments/assets/468a2d3c-50f7-4fd5-a242-618d8768fcfa" />

<br><br>

### 🔹 Access Port Configuration

<img width="1897" height="955" alt="Access Port Configuration" src="https://github.com/user-attachments/assets/d778d4aa-821b-4e1c-a7a5-4adc6d4c956c" />

<br><br>

### 🔹 802.1Q Trunk Configuration

<img width="1890" height="980" alt="802.1Q Trunk Configuration" src="https://github.com/user-attachments/assets/7967c27a-b1ed-40e2-ab51-15f2a8e3d19a" />

<br><br>

### 🔹 VLAN & Trunk Verification

<img width="1897" height="1003" alt="VLAN and Trunk Verification" src="https://github.com/user-attachments/assets/20134de8-cf23-41ba-bafd-15997448f139" />

<br><br>

## 📊 Communication Process

### Same-VLAN Communication — Cyber Team

```mermaid
flowchart TD
    A[Cyber_TEAM_EMPLOYER_1<br/>10.1.1.1<br/>VLAN 10] --> B[Check Destination IP]
    B --> C{Same VLAN?}

    C -->|Yes| D[Check ARP Table]
    D --> E{MAC Address Known?}

    E -->|No| F[ARP Request]
    F --> G[ARP Reply]
    G --> H[Create Ethernet Frame]

    E -->|Yes| H

    H --> I[First_Floor_Switch<br/>Gi0/2<br/>Access Port]
    I --> J[Gi0/0<br/>802.1Q Trunk]
    J --> K[Main_Switch<br/>Gi0/0 → Gi0/1]
    K --> L[Gi0/0<br/>802.1Q Trunk]
    L --> M[Second_Floor_Switch<br/>Gi0/2<br/>Access Port]

    M --> N[Cyber_TEAM_EMPLOYER_2<br/>10.1.1.2<br/>VLAN 10]

    N --> O[ICMP Echo Request]
    O --> P[ICMP Echo Reply]
    P --> Q[Successful Same-VLAN Communication]
```

<br>

## 🔒 Layer 2 VLAN Isolation

### Cyber Team — VLAN 10

`10.1.1.1` ↔ `10.1.1.2`

✅ Communication successful

<br>

### IT Team — VLAN 20

`10.2.2.1` ↔ `10.2.2.2`

✅ Communication successful

<br>

### Cyber Team ↔ IT Team

`VLAN 10` ↛ `VLAN 20`

❌ Communication blocked

**Reason:** VLAN 10 and VLAN 20 are separate Layer 2 broadcast domains, and no Layer 3 routing is configured between them.

<br>

## 📸 Connectivity Verification

<img width="1868" height="936" alt="Connectivity Verification" src="https://github.com/user-attachments/assets/2f06ee68-1862-4673-ac64-9572d6f4c9fb" />

<br><br>

## ✅ Result

* Cyber_EMPLOYER_1 and Cyber_EMPLOYER_2 successfully communicated within **VLAN 10**.
* IT_EMPLOYER_1 and IT_EMPLOYER_2 successfully communicated within **VLAN 20**.
* Communication between the Cyber and IT teams was blocked because they belong to different VLANs.
* No Layer 3 routing was configured between VLAN 10 and VLAN 20.

This lab successfully demonstrated **same-VLAN communication, 802.1Q trunking, VLAN segmentation, MAC address learning, ARP, and Layer 2 isolation**.

<br>

## 📚 Key Learning

This lab helped me understand:

* VLAN segmentation
* Access port configuration
* 802.1Q trunking
* Allowed VLANs
* Inter-switch VLAN communication
* MAC address learning
* ARP resolution
* Same-VLAN communication
* Layer 2 broadcast-domain isolation
* Difference between Layer 2 switching and Layer 3 routing

