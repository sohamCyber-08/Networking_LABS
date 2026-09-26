# 🧪 VTP Transparent Mode Lab
 
## 🎯 Objective
 
Configure a Cisco switching topology using VTP Transparent mode 
to understand VTP Server, Client, and Transparent modes, VLAN 
database independence, VTP advertisement forwarding, and 
802.1Q trunking.
 
## 🖥️ Topology 
 
SERVER ─── TRANSPARENT ─── CLIENT
 
**SERVER:** VLAN 10, VLAN 20, VLAN 30  
**TRANSPARENT:** VLAN 40, VLAN 50  
**CLIENT:** VLAN 10, VLAN 20, VLAN 30

<br>
<br>
<br>

<img width="1218" height="761" alt="Screenshot 2026-09-23 094507" src="https://github.com/user-attachments/assets/d033e935-0640-47c8-979d-3ac86c3c5a74" />

<br>
<br>
<br>

## 🔧 Technologies 
 
- Cisco IOS
- VTP
- VTP Server Mode
- VTP Client Mode
- VTP Transparent Mode
- 802.1Q Trunking
- VLAN
- Layer 2 Switching
- MAC Address Table

## ⚙️ VTP Configuration

### 🔴 SERVER — VTP Server Mode
 
**VTP Mode:** Server
 
The SERVER switch operates in **VTP Server mode** and maintains 
the VLAN database for:
 
```text
VLAN 10
VLAN 20
VLAN 30
```

<img width="985" height="437" alt="Screenshot 2026-09-23 094604" src="https://github.com/user-attachments/assets/8910cd99-d512-48ce-929b-5a20a00df96d" />


<br>
<br>
<br>

### 🔵 TRANSPARENT — VTP Transparent Mode

**VTP Mode:** Transparent

The TRANSPARENT switch maintains its **own local VLAN database**.

Locally configured VLANs:

```text
VLAN 40
VLAN 50
```

The switch does not synchronize its local VLAN database with
the VTP Server.

<img width="1006" height="545" alt="Screenshot 2026-09-23 094713" src="https://github.com/user-attachments/assets/f0e953d5-3b22-4757-9008-fc9e6843e9b3" />


<br>
<br>
<br>

### 🟢 CLIENT — VTP Client Mode

**VTP Mode:** Client

The CLIENT switch operates in **VTP Client mode** and receives
VLAN information through VTP advertisements.

Expected VLANs:

```text
VLAN 10
VLAN 20
VLAN 30
```

<img width="1137" height="557" alt="Screenshot 2026-09-23 094833" src="https://github.com/user-attachments/assets/308d8f6c-d03b-4caf-804f-73f2b0ff8617" />


<br>
<br>
<br>

## 🔗 Trunk Configuration

The inter-switch links are configured as **802.1Q trunk links**.

```text
CLIENT
   |
   | 802.1Q Trunk
   |
TRANSPARENT
   |
   | 802.1Q Trunk
   |
SERVER
```

### 🔴 Server Mode Switch — Trunk Interfaces

<img width="940" height="387" alt="Server Mode Switch Trunk Interfaces" src="https://github.com/user-attachments/assets/66768b32-f251-4515-bd4d-ecd4767628b6" />

<br>
<br>
<br>

### 🔵 Transparent Mode Switch — Trunk Interfaces

<img width="1072" height="372" alt="Transparent Mode Switch Trunk Interfaces" src="https://github.com/user-attachments/assets/16b0510e-2590-4ed6-af18-1ccc4588a320" />

<br>
<br>
<br>

### 🟢 Client Mode Switch — Trunk Interfaces

<img width="1226" height="298" alt="Client Mode Switch Trunk Interfaces" src="https://github.com/user-attachments/assets/63e509a6-0c51-4fa5-9829-72675e21564b" />

<br>
<br>
<br>

The trunk links allow multiple VLANs to traverse a single
physical connection.

## 🔍 Verification
### 📋 VTP Information

Verify the following information using:

```text
show vtp status
```

- VTP Domain
- VTP Mode
- VTP Version
- Configuration Revision
  

Expected:

```text
SERVER        → Server
TRANSPARENT   → Transparent
CLIENT        → Client
```
<br>
<img width="1890" height="963" alt="Screenshot 2026-09-23 111553" src="https://github.com/user-attachments/assets/e5cb2de2-3ca4-4270-bf75-f311e894a6aa" />

<br>

### 🧪 VLAN Database Verification

After configuring the VTP modes and trunk links, verify the
VLAN database on each switch.

### 🔴 SERVER — VTP Server Mode

**Expected VLANs:**

```text
VLAN 10
VLAN 20
VLAN 30
```

<img width="1145" height="482" alt="Screenshot 2026-09-23 094631" src="https://github.com/user-attachments/assets/fb970435-4d79-40c5-a0e1-f8b1d98ea8d4" />


<br>
<br>
<br>

### 🔵 TRANSPARENT — VTP Transparent Mode

**Expected VLANs:**

```text
VLAN 40
VLAN 50
```

<img width="1226" height="588" alt="Screenshot 2026-09-23 094759" src="https://github.com/user-attachments/assets/3cd3ed37-909d-47a4-a7d4-952df80c8d57" />


<br>
<br>
<br>

### 🟢 CLIENT — VTP Client Mode

**Expected VLANs:**

```text
VLAN 10
VLAN 20
VLAN 30
```

<img width="1175" height="602" alt="Screenshot 2026-09-23 094853" src="https://github.com/user-attachments/assets/bcb33ac7-d5ce-4a70-b51b-b1c428ec18de" />


<br>
<br>
<br>







<br>


## 📊 VTP Communication Process

### VTP Transparent Operation

```mermaid
flowchart TD
    A[SERVER<br/>VTP Server] --> B[Creates VLAN 10 / 20 / 30]
    B --> C[802.1Q Trunk]
    C --> D[TRANSPARENT<br/>VTP Transparent]
    D --> E[VTP Advertisement]
    E --> F[Advertisement Forwarded]
    F --> G[802.1Q Trunk]
    G --> H[CLIENT<br/>VTP Client]
    H --> I[Receives VTP Information]
    I --> J[Maintains VLAN 10 / 20 / 30]
    D --> K[Maintains Local VLAN 40 / 50]
```

## 📌 VTP Transparent Behavior

The TRANSPARENT switch behaves differently from a VTP Client.

It:

- Maintains its **own local VLAN database**.
- Does **not synchronize** its VLAN database with the VTP Server.
- Allows **local VLAN creation**.
- Maintains locally configured VLANs such as **VLAN 40 and VLAN 50**.
- Forwards VTP advertisements through trunk links when applicable.

## 🧠 Key Learning

- VTP Server mode
- VTP Client mode
- VTP Transparent mode
- VTP VLAN synchronization
- VLAN database independence
- VTP advertisement forwarding
- 802.1Q trunking
- Local VLAN creation
- VLAN database verification
- Layer 2 switching

## ✅ Result

The topology successfully demonstrates **VTP Transparent mode**.

The TRANSPARENT switch maintains its own local VLAN database
containing **VLAN 40 and VLAN 50** instead of synchronizing its
local VLAN database with the VTP Server.

The inter-switch links operate as **802.1Q trunks**, allowing
VTP advertisements to traverse the switching topology.

The lab demonstrates how **VTP Server, Client, and Transparent
modes handle VLAN information differently**.




