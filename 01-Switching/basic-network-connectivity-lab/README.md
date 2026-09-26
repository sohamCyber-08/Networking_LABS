# basic-network-connectivity-lab



## 📌 Overview

This lab demonstrates basic Layer 2 network connectivity using **EVE-NG, Cisco IOS, and VPCS**.

The lab focuses on IP addressing, ARP resolution, MAC address learning, Ethernet frame forwarding, and ICMP connectivity testing.

## 🎯 Objectives

* Configure IPv4 addressing on end devices
* Verify connectivity between two hosts
* Understand ARP-based IP-to-MAC resolution
* Verify MAC address learning on a switch
* Understand basic Ethernet frame forwarding
* Perform basic network troubleshooting

## 🖥️ Lab Environment

* EVE-NG
* Cisco IOS Switch
* VPCS
* Ethernet

## 🌐 Network Topology

```text
PC1 ───────── SW1 ───────── PC2
```
<img width="1762" height="758" alt="Screenshot 2026-09-20 031250" src="https://github.com/user-attachments/assets/a8b1e83b-ee6c-4c29-b825-de40e7c63f46" />




## 📋 IP Addressing

| Device | IP Address    | Subnet Mask   | Default Gateway |
| ------ | ------------- | ------------- | --------------- |
| PC1    | 192.168.10.1  | 255.255.255.0 | Not Required    |
| PC2    | 192.168.10.2  | 255.255.255.0 | Not Required    |

## ⚙️ Configuration

### PC1

```bash
ip 192.168.10.1/24
```

### PC2

```bash
ip 192.168.10.2/24
```
<img width="1868" height="912" alt="Screenshot 2026-09-20 025526" src="https://github.com/user-attachments/assets/ef6997f0-eabe-4f49-8273-d0217d24b633" />

## 🔍 Verification


### Test connectivity

```bash
ping 192.168.10.1
```

### Check ARP table

```bash
show arp
```
<img width="1872" height="912" alt="Screenshot 2026-09-20 025701" src="https://github.com/user-attachments/assets/653010d3-2d87-461f-a232-3b1ad7d395e1" />


### Check switch MAC address table

```cisco
show mac address-table
```
<img width="1862" height="881" alt="Screenshot 2026-09-20 025609" src="https://github.com/user-attachments/assets/d4e1040d-b811-4b08-9dab-e12a79de793a" />


## ✅ Result

Successfully configured and verified basic Layer 2 connectivity between two hosts through a Cisco switch.

## 🔄 Communication Process

When PC1 communicates with PC2:

```text
PC1
 ↓
Checks destination network
 ↓
Determines PC2 is on the same subnet
 ↓
ARP Request
 ↓
ARP Reply
 ↓
PC1 learns PC2's MAC address
 ↓
Ethernet Frame
 ↓
SW1 performs MAC address lookup
 ↓
Frame forwarded to PC2
 ↓
ICMP Echo Reply
 ↓
PC1
```


### Key Concepts Demonstrated

* IPv4 addressing
* Subnetting
* ARP
* MAC address learning
* Ethernet switching
* ICMP
* Basic network troubleshooting

## 🧠 Key Learning

This lab provided practical understanding of how a host resolves a destination IP address to a MAC address using ARP and how a Layer 2 switch uses its MAC address table to forward Ethernet frames.

