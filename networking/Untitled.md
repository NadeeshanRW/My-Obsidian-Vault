# Trunking in Networking

**Network Trunking** කියන්නේ **එක physical network link එකක් හරහා VLAN කිහිපයක traffic එකවර ගෙන යන ක්‍රමය**.

සරලව:

```text
VLAN 10 ──┐
VLAN 20 ──┤
VLAN 30 ──┼──→ Trunk Link ──→ Switch
VLAN 40 ──┘
```

එක cable එකක් භාවිතා කරලා VLAN 10, 20, 30, 40 සියල්ලගේ traffic transport කරන්න පුළුවන්.

---

# 1. Trunking අවශ්‍ය ඇයි?

මුලින් VLAN ටිකක් තියෙන network එකක් බලමු.

```text
             Switch 1
          ┌─────────────┐
VLAN 10 ──┤             │
VLAN 20 ──┤             │
VLAN 30 ──┤             │
          └─────────────┘
```

දැන් මේ VLANs තවත් switch එකකටත් අවශ්‍යයි.

Trunking නැත්නම් එක් VLAN එකකට වෙනම link එකක් දාන්න වෙනවා:

```text
Switch 1                     Switch 2

VLAN 10 ───────────────────── VLAN 10
VLAN 20 ───────────────────── VLAN 20
VLAN 30 ───────────────────── VLAN 30
```

Links 3ක් අවශ්‍යයි.

Trunking use කළොත්:

```text
Switch 1                     Switch 2

VLAN 10 ──┐
VLAN 20 ──┤
VLAN 30 ──┼════ ONE TRUNK ════
          │
```

**එක physical link එකක් හරහා VLAN තුනම යනවා.**

---

# 2. Access Port vs Trunk Port

මේ දෙක හොඳට තේරුම්ගන්න.

## Access Port

සාමාන්‍යයෙන් **එක් VLAN එකකට** device එකක් connect කරන port එක.

```text
PC
 │
 │
 ▼
Switch
 │
Access Port
 │
VLAN 10
```

Example:

```text
PC → Switch Port Gi0/1
```

Gi0/1:

```text
Access VLAN 10
```

PC එකට VLAN 10 traffic එක ලැබෙනවා.

---

## Trunk Port

**VLAN කිහිපයක traffic** carry කරන port එක.

```text
Switch 1
   │
   │ Trunk
   │
   ▼
Switch 2

VLAN 10
VLAN 20
VLAN 30
```

---

# 3. Trunk එකේ VLAN Tagging

මෙතන තමයි trunking වල most important concept එක.

Suppose VLAN 10 traffic එකක්:

```text
PC
 ↓
Switch
 ↓
VLAN 10 Frame
```

Trunk link එක හරහා යවනකොට switch එක frame එකට **VLAN identification/tag information** එකක් add කරනවා.

Conceptually:

```text
Original Frame

[ Ethernet Header ][ Data ]

              ↓

Trunk Frame

[ Ethernet Header ][ VLAN Tag ][ Data ]
```

ඒ VLAN tag එකෙන් receiving switch එකට කියනවා:

> "මේ frame එක VLAN 10 එකට අයිති."

---

# 4. 802.1Q

Modern Ethernet VLAN trunking වල commonly used standard එක:

**IEEE 802.1Q**

කෙටියෙන්:

```text
802.1Q = VLAN tagging standard
```

Trunk link එකේ frame එකට VLAN information එක insert කරනවා.

Concept:

```text
Ethernet Frame
┌──────────┬──────────┬──────────┬─────────┐
│ Dest MAC │ Src MAC  │ 802.1Q   │ Payload │
│          │          │ VLAN Tag │         │
└──────────┴──────────┴──────────┴─────────┘
```

---

# 5. VLAN Tag එකේ වැදගත් කොටස

802.1Q tag එකේ **VLAN ID** තියෙනවා.

VLAN ID range එක:

```text
0 – 4095
```

නමුත් සියලු IDs usable VLANs ලෙස භාවිතා කරන්න බැහැ; practical configuration එකේ සාමාන්‍ය VLAN IDs `1–4094` අතර.

උදාහරණ:

```text
VLAN 10
VLAN 20
VLAN 30
```

Frame එක:

```text
VLAN ID = 10
```

කියලා tagged වෙලා trunk එක හරහා යනවා.

---

# 6. Real Example

Company එකක VLAN 3ක් තියෙනවා:

```text
VLAN 10 → Management
VLAN 20 → Users
VLAN 30 → Servers
```

Switch දෙකක් තියෙනවා:

```text
          Switch 1
       ┌────────────┐
       │            │
       │ VLAN 10    │
       │ VLAN 20    │
       │ VLAN 30    │
       └──────┬─────┘
              │
              │ TRUNK
              │
       ┌──────▼─────┐
       │            │
       │ Switch 2   │
       │            │
       └────────────┘
```

Switch 1 → Switch 2 link එක trunk.

ඒ link එක හරහා:

```text
VLAN 10 traffic
VLAN 20 traffic
VLAN 30 traffic
```

සියල්ල යනවා.

---

# 7. Trunk එකේ Native VLAN

මෙකත් interview වල අහන concept එකක්.

802.1Q trunk එකේ **native VLAN** කියන්නේ සාමාන්‍යයෙන් **untagged traffic** associate කරන VLAN එක.

Example:

```text
Trunk
├── VLAN 10 → Tagged
├── VLAN 20 → Tagged
├── VLAN 30 → Tagged
└── Native VLAN → Untagged
```

Cisco environments වල default native VLAN එක බොහෝවිට VLAN 1.

නමුත් production network එකක security/design requirements අනුව native VLAN එක explicitly configure කිරීම common practice එකක්.

**Native VLAN mismatch** වුණොත් network issues ඇතිවෙන්න පුළුවන්.

---

# 8. Trunk එක Switch ↔ Switch විතරද?

නැහැ.

Trunk link එක භාවිතා කරන්න පුළුවන්:

### Switch ↔ Switch

```text
Switch ───── Trunk ───── Switch
```

### Switch ↔ Router

Router-on-a-stick architecture:

```text
                 Router
                   │
                 Trunk
                   │
                Switch
              ┌────┼────┐
              │    │    │
           VLAN10 20   30
```

### Switch ↔ Firewall

```text
Switch
  │
  │ Trunk
  │
Firewall
```

Firewall එක VLAN interfaces කිහිපයක් handle කරන්න පුළුවන්.

### Switch ↔ Hypervisor

මේක virtualization වල **ඉතාම වැදගත්**.

```text
                 Switch
                   │
                 Trunk
                   │
              ESXi / Proxmox
              ┌────┼────┐
              │    │    │
             VM1  VM2  VM3
             │    │    │
           VLAN10 20   30
```

ඔයා Proxmox/VMware environment එකක් manage කරනවා නම් මේ concept එක practically නිතරම හම්බවෙයි.

---

# 9. Trunk + Access එක එකට

Real network එකක මෙහෙම architecture එකක් තියෙන්න පුළුවන්:

```text
                    Core Switch
                         │
                       TRUNK
                         │
                  Distribution
                         │
                       TRUNK
                         │
                  Access Switch
                 ┌───────┼───────┐
                 │       │       │
              Access   Access  Access
                 │       │       │
                PC      PC      AP
              VLAN10   VLAN20  VLAN30
```

මෙතන:

**Switch ↔ Switch**

→ Trunk

**Switch ↔ End Device**

→ Access

---

# 10. Cisco Example

Cisco switch එකක conceptual configuration:

```bash
interface GigabitEthernet0/1
 switchport mode trunk
```

ඒක trunk mode එකට දානවා.

Specific VLAN allow කරන්න:

```bash
interface GigabitEthernet0/1
 switchport mode trunk
 switchport trunk allowed vlan 10,20,30
```

දැන් trunk එක හරහා:

```text
VLAN 10 ✅
VLAN 20 ✅
VLAN 30 ✅
VLAN 40 ❌
```

වගේ traffic control කරන්න පුළුවන්.

---

# 11. Access Port Example

PC එක VLAN 10 එකට දාන්න:

```bash
interface GigabitEthernet0/2
 switchport mode access
 switchport access vlan 10
```

Architecture:

```text
PC
 │
Gi0/2
 │
Access VLAN 10
 │
Switch
```

---

# 12. Trunk එකේ traffic flow එක

Suppose PC-A:

```text
VLAN 10
```

PC-A frame එක:

```text
PC-A
 ↓
Switch 1
 ↓
VLAN 10
 ↓
802.1Q Tag = VLAN 10
 ↓
TRUNK
 ↓
Switch 2
 ↓
VLAN 10
 ↓
PC-B
```

Receiving switch එක destination access port එකට frame එක යවද්දී VLAN tag එක remove කරලා end device එකට සාමාන්‍ය Ethernet frame එකක් යවන්න පුළුවන්.

```text
Trunk side:

[Frame][VLAN 10 Tag]

       ↓

Access side:

[Frame]
```

PC එකට සාමාන්‍යයෙන් VLAN tag එක ගැන දැනගන්න අවශ්‍ය නැහැ.

---

# 13. Trunking වල common problems

## ① Native VLAN mismatch

Switch 1:

```text
Native VLAN = 10
```

Switch 2:

```text
Native VLAN = 20
```

Problem ඇතිවෙන්න පුළුවන්.

---

## ② Allowed VLAN mismatch

Switch 1:

```text
Allowed: 10,20,30
```

Switch 2:

```text
Allowed: 10,20
```

VLAN 30 traffic එක Switch 2 පැත්තට pass නොවෙයි.

---

## ③ Port accidentally configured as Access

Expected:

```text
Switch ── TRUNK ── Switch
```

Actual:

```text
Switch ── ACCESS ── Switch
```

එවිට multiple VLAN traffic fail වෙන්න පුළුවන්.

---

## ④ VLAN doesn't exist

Trunk එක configure කරලා තිබුණත් VLAN එක switch එකේ properly configured නැත්නම් traffic issue වෙන්න පුළුවන්.

---

# 14. Trunking vs Link Aggregation

මේ දෙක **එකම දෙයක් නෙවෙයි**.

### Trunking

Purpose:

> Multiple VLANs එක link එක හරහා carry කිරීම.

```text
VLAN10
VLAN20
VLAN30
   ↓
ONE LINK
```

### LACP / EtherChannel

Purpose:

> Physical links කිහිපයක් එක logical link එකක් වගේ combine කිරීම.

```text
Link 1 ─┐
Link 2 ─┼──→ LAG
Link 3 ─┘
```

මේ දෙක එකටත් භාවිතා කරන්න පුළුවන්:

```text
       LACP / LAG
┌────────┬────────┐
│        │        │
Link 1   Link 2
│        │
└─── TRUNK ──────┘
```

එතකොට **multiple physical links + multiple VLANs** දෙකම handle කරන්න පුළුවන්.

---

# 15. Trunking vs VLAN

**VLAN** කියන්නේ logical network segmentation.

```text
VLAN 10 → Users
VLAN 20 → Servers
VLAN 30 → Management
```

**Trunk** කියන්නේ ඒ VLANs වල traffic transport කරන link එක.

```text
VLAN 10 ─┐
VLAN 20 ─┼──→ TRUNK → another network device
VLAN 30 ─┘
```

ඒ නිසා:

> **VLAN = segmentation**  
> **Trunk = VLAN traffic transport**

---

# 🧠 Interview එකකට මතක තියාගන්න

**Q: What is trunking?**

> Trunking is a method of carrying traffic from multiple VLANs over a single physical network link, typically using IEEE 802.1Q VLAN tagging.

**Q: Access vs Trunk?**

```text
Access → Usually one VLAN
Trunk  → Multiple VLANs
```

**Q: What is 802.1Q?**

```text
IEEE standard for VLAN tagging.
```

**Q: What is Native VLAN?**

```text
The VLAN associated with untagged traffic on an 802.1Q trunk.
```

**Q: Where do you use trunking?**

```text
Switch ↔ Switch
Switch ↔ Router
Switch ↔ Firewall
Switch ↔ Hypervisor
```

**Main picture එක මේකයි:**

```text
             MULTIPLE VLANs
          ┌─────┬─────┬─────┐
          │ 10  │ 20  │ 30  │
          └──┬──┴──┬──┴──┬──┘
             │     │     │
             └─────┼─────┘
                   │
              802.1Q TRUNK
                   │
                   ▼
             ┌───────────┐
             │  Switch 2 │
             └───────────┘
```

**Trunking ඉගෙනගත්තට පස්සේ ඊළඟට `STP (Spanning Tree Protocol)` ඉගෙනගන්න එක හොඳයි**, මොකද real enterprise network එකක **Trunk + VLAN + STP** තුන එකටම වැඩ කරනවා.