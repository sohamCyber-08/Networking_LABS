


# 🧪 Private VLAN (PVLAN) Lab

## 🎯 Objective

Configure and verify Cisco Private VLANs (PVLANs) using:

- Primary VLAN
- Community VLANs
- Isolated VLAN
- Promiscuous port

This lab demonstrates how Private VLANs provide **Layer 2 host isolation** while still allowing hosts to communicate with a common Layer 3 gateway.

---

## 🖥️ Topology

```text
                         Router
                     192.168.10.1
                          |
                    Promiscuous Port
                       VLAN 500
                          |
                       SW1
          ________________|________________
         |                |                |
         |                |                |
   Community 1       Community 2        Isolated
     VLAN 501          VLAN 502          VLAN 503
     |     |           |     |           |     |
   PC1   PC2          PC3   PC4         PC5   PC6
   .10   .20          .30   .40         .50   .60
````

---

## 🌐 IP Addressing

| Device | PVLAN | IP Address    | Subnet Mask | Role        |
| ------ | ----: | ------------- | ----------- | ----------- |
| Router |   500 | 192.168.10.1  | /24         | Promiscuous |
| PC1    |   501 | 192.168.10.10 | /24         | Community   |
| PC2    |   501 | 192.168.10.20 | /24         | Community   |
| PC3    |   502 | 192.168.10.30 | /24         | Community   |
| PC4    |   502 | 192.168.10.40 | /24         | Community   |
| PC5    |   503 | 192.168.10.50 | /24         | Isolated    |
| PC6    |   503 | 192.168.10.60 | /24         | Isolated    |

**Default Gateway:** `192.168.10.1`

---

## 🏗️ Private VLAN Design
<br>
<img width="1856" height="902" alt="Screenshot 2026-09-23 003532" src="https://github.com/user-attachments/assets/191323f0-dc1c-4eb5-9885-2e63d4fc96dd" />




### Primary VLAN

```text
VLAN 500
```

The primary VLAN is the main VLAN associated with the secondary VLANs.

### Community VLANs

```text
VLAN 501 → Community 1
VLAN 502 → Community 2
```

Hosts within the **same community VLAN** can communicate with each other.

```text
PC1 VLAN 501  <---->  PC2 VLAN 501
       ✅ Communication

PC3 VLAN 502  <---->  PC4 VLAN 502
       ✅ Communication
```

Hosts belonging to **different community VLANs** are isolated from each other.

```text
VLAN 501  ✖  VLAN 502
```

### Isolated VLAN

```text
VLAN 503
```

Hosts in the isolated VLAN cannot communicate directly with other isolated or community hosts.

```text
PC5 VLAN 503  ✖  PC6 VLAN 503
PC5 VLAN 503  ✖  VLAN 501
PC5 VLAN 503  ✖  VLAN 502
```

However, isolated hosts can communicate with the **promiscuous port**, such as the router.

```text
PC5 VLAN 503  --->  Router
PC6 VLAN 503  --->  Router
```

---

## 🔧 Technologies

* Cisco IOS
* Private VLAN (PVLAN)
* Primary VLAN
* Community VLAN
* Isolated VLAN
* Promiscuous Port
* IPv4
* Ethernet
* ARP
* ICMP
* Layer 2 Segmentation

---

## ⚙️ PVLAN Configuration

<img width="1715" height="585" alt="Screenshot 2026-09-23 003616" src="https://github.com/user-attachments/assets/4f925fd8-8361-416b-82f6-30702be5df42" />




### 1. Create Primary VLAN

```cisco
vlan 500
 private-vlan primary
 private-vlan association 501,502,503
```

### 2. Create Community VLANs

```cisco
vlan 501
 private-vlan community

vlan 502
 private-vlan community
```

### 3. Create Isolated VLAN

```cisco
vlan 503
 private-vlan isolated
```

---

## 🔌 Configure Host Ports

### Community VLAN 501

```cisco
interface range g0/0-1
 switchport mode private-vlan host
 switchport private-vlan host-association 500 501
```

### Community VLAN 502

```cisco
interface range g0/2-3
 switchport mode private-vlan host
 switchport private-vlan host-association 500 502
```

### Isolated VLAN 503

```cisco
interface range g1/0-1
 switchport mode private-vlan host
 switchport private-vlan host-association 500 503
```

---

## 🌐 Configure Promiscuous Port

The router-facing interface is configured as a **promiscuous port**.

```cisco
interface g1/2
 switchport mode private-vlan promiscuous
 switchport private-vlan mapping 500 501
 switchport private-vlan mapping 500 502
 switchport private-vlan mapping 500 503
```

The promiscuous port can communicate with hosts belonging to all associated secondary VLANs.

---

## 🔍 Verify PVLAN Configuration

### Display PVLANs

```cisco
show vlan private-vlan
```

<img width="1895" height="790" alt="Screenshot 2026-09-23 003625" src="https://github.com/user-attachments/assets/2da47d37-5b96-4bc0-b158-616964edb704" />



# 📊 Communication Testing

## Test 1 — Same Community VLAN

PC1:

```text
192.168.10.10
```

PC2:

```text
192.168.10.20
```


Both hosts belong to **Community VLAN 501**.

```text
PC4 VLAN 501
     |
     | ICMP
     ↓
PC4 VLAN 501
```

### Result

```text
PC4 → PC3
✅ Successful
```

---

<img width="506" height="316" alt="Screenshot 2026-09-23 004816" src="https://github.com/user-attachments/assets/b32db978-eb0d-426c-953e-fd9ad94d2ae1" />





## Test 2 — Same Community VLAN


Both hosts belong to **Community VLAN 502**.

### Result


<img width="540" height="292" alt="Screenshot 2026-09-23 004615" src="https://github.com/user-attachments/assets/5db42ca7-5012-47e5-8c70-63603b4e2e5b" />


---

## Test 3 — Isolated VLAN

PC8:

```text
192.168.10.50
```

PC7:

```text
192.168.10.60
```

Both hosts belong to **Isolated VLAN 503**.

### Result

```text
PC8 → PC7
❌ Blocked
```

Even though both hosts are in the same IP subnet, the PVLAN configuration prevents direct Layer 2 communication.

---
<img width="1197" height="477" alt="Screenshot 2026-09-23 005124" src="https://github.com/user-attachments/assets/2de514f4-45c9-4035-b7be-6e8304dd109b" />


## Test 4 — Host to Router

All hosts use:

```text
Default Gateway: 192.168.10.1
```

### Results
Here I present ouput of only PC8->router but in lab all PC reach Router
<br>
```text
PC1 → Router
✅ Successful

PC2 → Router
✅ Successful

PC3 → Router
✅ Successful

PC4 → Router
✅ Successful

PC5 → Router
✅ Successful

PC8 → Router
✅ Successful


```
<img width="1020" height="631" alt="Screenshot 2026-09-23 005056" src="https://github.com/user-attachments/assets/3b81ec6c-7e85-46d7-9e7d-0a90a7e1e23d" />


This demonstrates the purpose of the **promiscuous port**.

---

# 🔄 Communication Process

## Same Community Communication

```mermaid
flowchart TD
    A[PC1<br/>192.168.10.10<br/>VLAN 501] --> B[Destination IP]
    B --> C{Same PVLAN Community?}
    C -->|Yes| D[ARP Resolution]
    D --> E[Ethernet Frame]
    E --> F[Switch]
    F --> G[Community VLAN 501]
    G --> H[PC2<br/>192.168.10.20]
    H --> I[ICMP Echo Reply]
    I --> J[Successful Communication]
```

---

## Isolated Host Communication

```mermaid
flowchart TD
    A[PC5<br/>192.168.10.50<br/>VLAN 503] --> B[Destination PC6]
    B --> C[ARP Request]
    C --> D[PVLAN Isolation]
    D --> E[Frame Not Forwarded]
    E --> F[Communication Blocked]
```

---

## Host-to-Router Communication

```mermaid
flowchart TD
    A[PVLAN Host] --> B[Secondary VLAN]
    B --> C[Primary VLAN 500]
    C --> D[Promiscuous Port]
    D --> E[Router<br/>192.168.10.1]
    E --> F[ICMP Reply]
    F --> G[Successful Communication]
```

---

# 🧠 PVLAN Communication Model

```text
                 Primary VLAN 500
                        |
          +-------------+-------------+
          |             |             |
      Community      Community      Isolated
        501             502            503
       /   \           /   \          /   \
     PC1   PC2       PC3   PC4      PC5   PC6

      ↕                 ↕              X
   Allowed           Allowed       Blocked
```



# ✅ Result

The Private VLAN configuration was successfully implemented and verified.

The lab demonstrated:

* Community-to-community communication within the same secondary VLAN
* Isolation between different community VLANs
* Complete Layer 2 isolation of isolated ports
* Communication between secondary VLANs and the promiscuous router port
* PVLAN-based segmentation within the same IP subnet

---

# 📚 Key Learning

* Private VLAN architecture
* Primary VLAN
* Secondary VLAN
* Community VLAN
* Isolated VLAN
* Promiscuous port
* PVLAN host association
* PVLAN mapping
* Layer 2 host isolation
* ARP behavior with PVLANs
* ICMP connectivity testing
* Cisco IOS PVLAN configuration
* Enterprise network segmentation




