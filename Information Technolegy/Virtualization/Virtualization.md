**Virtualization Concepts** කියන්නේ Architecture level එකට යනකොට අනිවාර්ය foundation එකක්. මේකේ Hypervisor, VM, Virtual Network, Virtual Storage, Resource Allocation කියන 5ක් වෙන වෙනම පාඩම් කරනවාට වඩා **physical datacenter එකක් virtual datacenter එකක් බවට transform කරන්නේ කොහොමද** කියන එක තේරුම් ගන්න ඕන.

---

# 00.4 Virtualization Concepts

මුලින්ම entire picture එක බලමු.

```text
PHYSICAL DATACENTER
│
├── Physical Server
│   ├── CPU
│   ├── RAM
│   ├── NIC
│   └── Disk
│
└───────────────┐
                │
                ▼
           HYPERVISOR
                │
       ┌────────┼────────┐
       ▼        ▼        ▼
      VM-1     VM-2     VM-3
       │        │        │
     OS       OS       OS
       │        │        │
     App      App      App
```

ඒ කියන්නේ එක physical server එකක්:

```text
Physical Server
       ↓
Hypervisor
       ↓
Multiple Virtual Machines
```

බවට convert කරනවා.

---

# 00.4.1 ඇයි Virtualization ඕන?

Virtualization නැති server environment එකක් හිතන්න.

```text
Server 1
└── Application A

Server 2
└── Application B

Server 3
└── Database

Server 4
└── Monitoring
```

සමහරවිට server එකේ capacity එකෙන් 10–20% විතරයි use වෙන්නේ.

උදාහරණයක්:

```text
Server
CPU = 32 cores
RAM = 128 GB

Application uses:
CPU = 4 cores
RAM = 16 GB
```

ඉතිරි:

```text
28 CPU cores
112 GB RAM
```

wasted.

Virtualization දාපු ගමන්:

```text
Physical Server
│
└── Hypervisor
    │
    ├── VM-1 → 4 CPU / 16 GB
    ├── VM-2 → 8 CPU / 32 GB
    ├── VM-3 → 4 CPU / 16 GB
    ├── VM-4 → 8 CPU / 32 GB
    └── VM-5 → 4 CPU / 16 GB
```

එක physical machine එකෙන් workloads ගොඩක් run කරන්න පුළුවන්.

---

# 00.4.2 Virtualization කියන්නේ මොකක්ද?

Simple definition:

> **Physical hardware resources abstract කරලා, ඒවා multiple isolated virtual computers වලට ලබාදෙන technology එක.**

මෙතන key word එක:

### Abstraction

Physical:

```text
Physical CPU
Physical RAM
Physical Disk
Physical NIC
```

Virtual:

```text
vCPU
vRAM
vDisk
vNIC
```

VM එකට පේන්නේ:

> "මට computer එකක් තියෙනවා."

ඇත්තටම ඒ computer එක physical එකක් නෙමෙයි.

---

# 00.4.3 Full Virtualization Stack

Architecture level එකේ මේ diagram එක මතක තියාගන්න.

```text
                    USERS
                      │
                      ▼
                APPLICATION
                      │
                      ▼
                 GUEST OS
                      │
                      ▼
              VIRTUAL HARDWARE
              │      │      │
              ▼      ▼      ▼
            vCPU    vRAM    vDisk
              │      │      │
              └──────┼──────┘
                     ▼
                 HYPERVISOR
                     │
          ┌──────────┼──────────┐
          ▼          ▼          ▼
       Physical    Physical   Physical
         CPU         RAM        Storage
                     │
                     ▼
                 Physical NIC
```

මේක තමයි virtualization architecture එකේ core.

---

# 1. Hypervisor

මේක තමයි first major concept.

## Hypervisor කියන්නේ?

> **Physical hardware සහ Virtual Machines අතර resource management layer එක.**

උදාහරණයක්:

```text
Physical Hardware
       │
       ▼
   Hypervisor
       │
 ┌─────┼─────┐
 ▼     ▼     ▼
VM1   VM2   VM3
```

Hypervisor එක decide කරනවා:

```text
VM1 → CPU කොච්චරද?
VM2 → RAM කොච්චරද?
VM3 → Disk කොහෙද?
VM1 → Network එක කොහොමද?
VM2 → Storage access කොහොමද?
```

---

# 1.1 Hypervisor Types

ප්‍රධාන වශයෙන් දෙකක්.

```text
Hypervisor
│
├── Type 1
│
└── Type 2
```

---

## Type 1 — Bare Metal Hypervisor

මෙක direct hardware මත run වෙනවා.

```text
Physical Hardware
       │
       ▼
   Hypervisor
       │
 ┌─────┼─────┐
 ▼     ▼     ▼
VM1   VM2   VM3
```

සාමාන්‍ය desktop OS එකක් අතරින් යන්නේ නැහැ.

Examples:

- VMware ESXi
    
- Microsoft Hyper-V
    
- KVM-based virtualization
    
- Xen
    

### Type 1 use වෙන තැන්

- Datacenters
    
- Enterprise
    
- Cloud
    
- Production infrastructure
    
- Server virtualization
    

---

# 1.2 Type 2 — Hosted Hypervisor

මෙක existing operating system එකක් උඩ run වෙනවා.

```text
Physical Hardware
       │
       ▼
Host OS
       │
       ▼
Hypervisor
       │
 ┌─────┼─────┐
 ▼     ▼     ▼
VM1   VM2   VM3
```

Examples:

- VMware Workstation
    
- Oracle VirtualBox
    
- Parallels Desktop
    

Use cases:

```text
Developer Laptop
        │
        ├── Windows
        │     └── Linux VM
        │
        └── Linux
              └── Windows VM
```

Labs වලට excellent.

---

# 1.3 Type 1 vs Type 2

||Type 1|Type 2|
|---|---|---|
|Runs on|Hardware|Host OS|
|Performance|High|Generally lower|
|Production|Yes|Rare|
|Datacenter|Yes|Rare|
|Laptop labs|Less common|Very common|
|Examples|ESXi, Hyper-V, KVM|VirtualBox, VMware Workstation|

---

# 1.4 Hypervisor එකේ actual job එක

Hypervisor එකේ job එක "VM run කරනවා" කියන එකට වඩා deep.

එයා manage කරනවා:

```text
CPU Scheduling
Memory Management
I/O
Storage
Networking
Isolation
Security
VM lifecycle
```

---

# 2. Virtual Machine — VM

VM කියන්නේ:

> **Software-defined computer එකක්.**

Physical computer එකක:

```text
CPU
RAM
Disk
NIC
BIOS/UEFI
```

VM එකක:

```text
vCPU
vRAM
vDisk
vNIC
Virtual firmware
```

---

# 2.1 VM එකක් ඇතුළත

```text
VM
│
├── vCPU
├── vRAM
├── vDisk
├── vNIC
├── Virtual BIOS/UEFI
│
└── Guest OS
      │
      ├── Linux
      └── Windows
```

ඊට පස්සේ applications:

```text
VM
│
└── Ubuntu
     │
     ├── Nginx
     ├── Python
     └── Application
```

---

# 2.2 Host vs Guest

මේ terms දෙක **අනිවාර්යයෙන්** දැනගන්න.

### Host

Virtualization infrastructure එක run කරන physical/underlying system.

### Guest

VM එක ඇතුළේ run වෙන OS.

```text
Physical Server
     │
     └── Host / Hypervisor
             │
             ├── Guest VM
             │     └── Ubuntu
             │
             └── Guest VM
                   └── Windows
```

---

# 2.3 VM Isolation

VM1:

```text
VM1
└── Application A
```

VM2:

```text
VM2
└── Application B
```

VM1 crash වුණාම theoretically VM2 unaffected වෙන්න පුළුවන්.

මේ isolation එක virtualization වල major benefit එකක්.

---

# 2.4 VM Lifecycle

Architecture level එකට:

```text
Create
  ↓
Configure
  ↓
Boot
  ↓
Run
  ↓
Monitor
  ↓
Shutdown
  ↓
Restart
  ↓
Migrate
  ↓
Clone
  ↓
Delete
```

VM management platforms මේ lifecycle එක automate කරනවා.

---

# 2.5 Snapshot

Snapshot කියන්නේ VM එකේ state එකක් capture කිරීම.

```text
VM
│
├── Disk state
└── Configuration/state
       │
       ▼
    Snapshot
```

Example:

```text
Before Upgrade
      ↓
   Snapshot
      ↓
Upgrade
      ↓
Something breaks
      ↓
Rollback
```

**Important:** Snapshot එක සාමාන්‍යයෙන් full backup එකක් ලෙස treat කරන්න එපා.

---

# 3. Virtual Network

මේක architecture වල **ඉතාම වැදගත්**.

VM එකකට internet/network access ඕන.

නමුත් VM එකට physical network card එකක් නැහැ.

ඒ වෙනුවට:

```text
VM
│
└── vNIC
      │
      ▼
Virtual Switch
      │
      ▼
Physical NIC
      │
      ▼
Physical Network
```

---

# 3.1 vNIC

Virtual Network Interface Card.

Physical:

```text
Physical Server
└── NIC
```

VM:

```text
VM
└── vNIC
```

Guest OS එකට:

```bash
ip addr
```

run කළාම network interface එකක් පේනවා.

ඒක virtual interface එකක්.

---

# 3.2 Virtual Switch

Physical switch එක:

```text
Server ─── Switch ─── Router
```

Virtual switch:

```text
VM1 ──┐
VM2 ──┼── vSwitch ── Physical NIC
VM3 ──┘
```

vSwitch එක software-defined switching layer එකක්.

---

# 3.3 VM-to-VM Network

VM1 සහ VM2 එකම virtual network එකක නම්:

```text
VM1
 │
vNIC
 │
 ▼
vSwitch
 │
 ▼
vNIC
 │
VM2
```

Traffic එක physical switch එකකට යන්නත් අවශ්‍ය නොවෙන්න පුළුවන්.

---

# 3.4 VM-to-Physical Network

```text
VM
 │
vNIC
 │
vSwitch
 │
Physical NIC
 │
Physical Switch
 │
Router
 │
Internet
```

මෙන්න virtual network එක physical network එකට connect වෙන හැටි.

---

# 3.5 Network Modes

Virtualization platforms වල common networking models:

```text
Virtual Network
│
├── Bridged
├── NAT
├── Host-only
└── Isolated/Internal
```

---

## Bridged

VM එක physical LAN එකේ වෙනම machine එකක් වගේ.

```text
LAN
│
├── Physical Server
│
├── VM1
├── VM2
└── VM3
```

VM එකට LAN IP එකක් ලැබෙනවා.

---

## NAT

VM outbound traffic host එක හරහා යනවා.

```text
VM
 ↓
NAT
 ↓
Host
 ↓
Internet
```

Labs/development වල common.

---

## Host-only

```text
VM1
 │
 ├── Host
 │
 └── VM2
```

Internet access නැති isolated lab network එකක් වගේ use කරන්න පුළුවන්.

---

# 3.6 VLANs + Virtualization

Enterprise architecture එකේ මේක තවත් deep.

```text
Physical Switch
│
├── VLAN 10 → Management
├── VLAN 20 → Servers
├── VLAN 30 → Storage
└── VLAN 40 → DMZ
```

Hypervisor එකේ virtual networks ඒ VLANs වලට map කරන්න පුළුවන්.

```text
VM
 │
vNIC
 │
vSwitch
 │
VLAN
 │
Physical Switch
```

ඒකෙන් VM traffic segmentation කරන්න පුළුවන්.

---

# 4. Virtual Storage

VM එකට disk එකක් ඕන.

නමුත් physical disk එක VM එකට direct දෙන්නේ නැතිව virtual disk එකක් දෙන්න පුළුවන්.

```text
VM
│
└── vDisk
       │
       ▼
Hypervisor
       │
       ▼
Physical Storage
```

---

# 4.1 Virtual Disk

Examples:

```text
VM
│
├── vDisk 1 → 100 GB
├── vDisk 2 → 500 GB
└── vDisk 3 → 1 TB
```

Guest OS එකට:

```text
/dev/sda
/dev/sdb
/dev/sdc
```

වගේ පේන්න පුළුවන්.

---

# 4.2 Virtual Disk Formats

Platform අනුව formats වෙනස්.

Examples:

```text
VMDK
VHDX
QCOW2
VHD
```

Architecture level එකේ important idea:

> **Virtual disk file/object එක physical storage එකක් මත reside කරනවා.**

---

# 4.3 Local Storage

```text
Physical Server
│
└── Local SSD
      │
      └── VM disks
```

Simple.

But server fail වුණොත් storage availability issue එකක් වෙන්න පුළුවන්.

---

# 4.4 Shared Storage

Enterprise architecture:

```text
Server 1 ───┐
Server 2 ───┼── Storage Network ── Shared Storage
Server 3 ───┘
```

Examples:

- SAN
    
- NAS
    
- Distributed storage
    

ඒකෙන් VM එක වෙන host එකකට move කිරීම පහසු වෙනවා.

---

# 4.5 Datastore

Virtualization environments වල:

```text
Datastore
│
├── VM-01
│   ├── config
│   └── virtual disk
│
├── VM-02
│   ├── config
│   └── virtual disk
│
└── VM-03
```

Datastore කියන්නේ VM files/disks store කරන logical storage location එකක් ලෙස හිතන්න.

---

# 5. Resource Allocation

මේක virtualization වල **heart එකක්**.

Physical server එක:

```text
CPU = 32 cores
RAM = 128 GB
Storage = 4 TB
Network = 10 Gbps
```

VMs වලට allocate කරන්න පුළුවන්:

```text
VM1
4 vCPU
16 GB RAM

VM2
8 vCPU
32 GB RAM

VM3
4 vCPU
16 GB RAM

VM4
8 vCPU
32 GB RAM
```

---

# 5.1 vCPU

Physical CPU cores → Virtual CPUs.

```text
Physical CPU
32 cores
   │
   ▼
Hypervisor
   │
   ├── VM1 → 4 vCPU
   ├── VM2 → 8 vCPU
   ├── VM3 → 4 vCPU
   └── VM4 → 8 vCPU
```

Total allocated:

```text
24 vCPU
```

Remaining physical capacity:

```text
8 cores
```

But virtualization එකේ allocation එක **always one-to-one physical core mapping එකක් නෙමෙයි**.

---

# 5.2 CPU Overcommit

මේක architecture-level concept එකක්.

Physical:

```text
32 CPU cores
```

VMs:

```text
VM1 → 16 vCPU
VM2 → 16 vCPU
VM3 → 16 vCPU
VM4 → 16 vCPU
```

Total:

```text
64 vCPU
```

Physical:

```text
32 cores
```

මෙය possible because:

> සියලු VMs එකම වෙලාවේ සියලු CPU resources full utilization කරනවා කියලා assume කරන්නේ නැහැ.

Hypervisor CPU scheduling කරනවා.

But excessive overcommit:

```text
CPU contention
     ↓
Performance degradation
```

---

# 5.3 RAM Allocation

Physical:

```text
128 GB RAM
```

VMs:

```text
VM1 → 16 GB
VM2 → 32 GB
VM3 → 32 GB
VM4 → 32 GB
```

Total:

```text
112 GB
```

Remaining:

```text
16 GB
```

---

# 5.4 Memory Overcommit

මෙතන complexity වැඩියි.

Hypervisor එක technologies use කරන්න පුළුවන්:

```text
Memory sharing
Ballooning
Compression
Swapping
```

Goal එක:

> Physical RAM efficiently utilize කිරීම.

But RAM overcommit වැඩි කරලා performance sacrifice කරන්න පුළුවන්.

---

# 5.5 Thin Provisioning

Storage වල super-important.

VM එකට:

```text
Virtual disk = 1 TB
```

කියලා configure කළා කියලා physical storage එකෙන් immediately 1 TB consume වෙනවා කියලා අනිවාර්ය නැහැ.

Thin provisioning:

```text
VM sees:
1 TB

Actual used:
100 GB
```

Physical storage:

```text
~100 GB consumed
```

As data grows:

```text
100 GB
 ↓
200 GB
 ↓
500 GB
 ↓
800 GB
```

physical storage usage එක වැඩි වෙනවා.

---

# 5.6 Thick Provisioning

Opposite:

```text
VM disk = 1 TB
       ↓
Allocate 1 TB immediately
```

Predictability වැඩි.

But storage utilization less efficient.

---

# 5.7 Resource Allocation — Reservation / Limit / Share

Enterprise virtualization වල මේ concepts එනවා.

### Reservation

VM එකට minimum guaranteed resource.

```text
VM
CPU Reservation = 4 cores
```

අර්ථය:

> "Resource contention ආවත් මේ capacity එක protect කරන්න."

---

### Limit

Maximum resource usage.

```text
VM
CPU Limit = 8 cores
```

VM එකට 8 cores වලට වඩා consume කරන්න බැහැ.

---

### Shares / Priority

Resource contention වෙලාවේ:

```text
VM1 → High priority
VM2 → Normal
VM3 → Low
```

වගේ priority model එකක් use කරන්න පුළුවන්.

---

# 6. VM Isolation

Virtualization වල security concept එක.

```text
VM1
│
├── CPU
├── RAM
├── Disk
└── Network

VM2
│
├── CPU
├── RAM
├── Disk
└── Network
```

VM1 normally VM2ගේ memory directly access කරන්න බැහැ.

Hypervisor isolation enforce කරනවා.

---

# 7. Virtualization + Security

Architecture එක:

```text
Physical Hardware
       │
       ▼
   Hypervisor
       │
 ┌─────┼─────┐
 ▼     ▼     ▼
VM1   VM2   VM3
 │     │     │
OS    OS    OS
 │     │     │
App   App   App
```

Security layers:

```text
Physical Security
      ↓
Hypervisor Security
      ↓
VM Isolation
      ↓
Guest OS Security
      ↓
Application Security
```

Hypervisor compromise වුණොත් multiple VMs risk වෙන්න පුළුවන්.

ඒ නිසා hypervisor security **critical infrastructure security** එකක්.

---

# 8. Virtualization Features You Must Know

ඔයා Architecture level එකට යනවා නම් මේ concepts පස්සේ අනිවාර්යයෙන් එනවා.

---

## VM Migration

VM එක:

```text
Host A
  │
  ▼
Host B
```

move කිරීම.

---

## Live Migration

VM running while migrate කිරීම.

Concept:

```text
VM running
     │
     ▼
Copy memory/state
     │
     ▼
Synchronize changes
     │
     ▼
Switch execution
     │
     ▼
Host B
```

Downtime very low.

Enterprise datacenters වල ඉතා වැදගත්.

---

# 9. High Availability

Hosts:

```text
Host A
Host B
Host C
```

VM:

```text
VM-01
```

Host A fail වුණොත්:

```text
Host A
   X
   │
   ▼
VM-01
   │
   ▼
Host B
```

HA system එක VM workload එක restart/move කරන්න පුළුවන්.

---

# 10. Fault Tolerance

HA සහ Fault Tolerance same නෙමෙයි.

### HA

Failure පස්සේ workload recover/restart කරන concept.

### Fault Tolerance

Failure එකේ impact එක almost continuously avoid කිරීමේ higher availability approach.

Architecture:

```text
Primary VM
     │
     │ synchronized
     ▼
Secondary VM
```

---

# 11. Virtualization vs Containerization

මේ distinction එක **අනිවාර්යයෙන්** දැනගන්න.

VM:

```text
Hardware
 ↓
Hypervisor
 ↓
VM
 ↓
Guest OS
 ↓
Application
```

Container:

```text
Hardware
 ↓
OS
 ↓
Container Runtime
 ↓
Containers
 ↓
Applications
```

VM එකකට usually:

```text
Own Guest OS
```

Container එක:

```text
Host Kernel
```

share කරනවා.

---

# 12. VM vs Container

||VM|Container|
|---|---|---|
|Virtualizes|Hardware|OS-level environment|
|Guest OS|Yes|No separate full OS|
|Isolation|Strong|Process-level|
|Startup|Usually slower|Usually faster|
|Size|Larger|Smaller|
|Density|Lower|Higher|
|Use|Full OS workloads|Microservices/apps|

Architecture:

```text
VM
│
├── OS
├── Libraries
└── App
```

Container:

```text
Container
│
├── Libraries
└── App
```

---

# 13. Virtualization in a Real Datacenter

දැන් ඔක්කොම එකට connect කරමු.

```text
                         USERS
                           │
                           ▼
                       INTERNET
                           │
                       FIREWALL
                           │
                     PHYSICAL SWITCH
                           │
              ┌────────────┴────────────┐
              │                         │
          HYPERVISOR 1             HYPERVISOR 2
              │                         │
       ┌──────┼──────┐           ┌──────┼──────┐
       ▼      ▼      ▼           ▼      ▼      ▼
      VM1    VM2    VM3         VM4    VM5    VM6
       │      │      │           │      │      │
      App    App    DB          App   API    DB
       │      │      │           │      │      │
       └──────┴──────┴───────────┴──────┴──────┘
                         │
                  VIRTUAL NETWORK
                         │
                    STORAGE NETWORK
                         │
                   SHARED STORAGE
```

මේක තමයි basic **virtualized datacenter architecture**.

---

# 14. Modern Virtualization Stack

Industry architecture එකේ තවත් level එකක්:

```text
Physical Infrastructure
│
├── Compute
├── Network
└── Storage
        │
        ▼
Virtualization Layer
        │
        ├── Hypervisor
        ├── Virtual Switch
        ├── Virtual Storage
        └── Resource Scheduler
                │
                ▼
Virtual Infrastructure
        │
        ├── VM
        ├── vCPU
        ├── vRAM
        ├── vNIC
        └── vDisk
                │
                ▼
Operating Systems
                │
                ▼
Applications
```

---

# 15. Cloud එකට මේක connect වෙන්නේ කොහොමද?

Cloud services understand කරන්න virtualization foundation එක අනිවාර්යයි.

ඔයා cloud provider එකක:

> "Give me a virtual server."

කියනකොට background එකේ conceptually:

```text
Cloud Datacenter
│
├── Physical Servers
│
├── Hypervisor / Virtualization
│
├── Virtual Networks
├── Virtual Storage
└── Resource Scheduling
        │
        ▼
     Your VM
        │
        ├── vCPU
        ├── RAM
        ├── Disk
        └── Network
```

ඒ නිසා:

**Cloud VM = virtualization concepts වලින් build වෙන service එකක්.**

---

# 16. Architecture level එකේ මතක තියාගන්න Master Model

මේක තමයි ඔයාට actual mental model එක:

```text
                PHYSICAL INFRASTRUCTURE
                         │
        ┌────────────────┼────────────────┐
        ▼                ▼                ▼
      Compute          Network          Storage
      CPU/RAM            NIC             Disk
        │                │                │
        └────────────────┼────────────────┘
                         ▼
                    HYPERVISOR
                         │
        ┌────────────────┼────────────────┐
        ▼                ▼                ▼
       VM-1             VM-2             VM-3
        │                │                │
     vCPU/vRAM        vCPU/vRAM        vCPU/vRAM
     vDisk            vDisk            vDisk
     vNIC             vNIC             vNIC
        │                │                │
        └────────────────┼────────────────┘
                         ▼
                  VIRTUAL NETWORK
                         │
                         ▼
                  PHYSICAL NETWORK

                  VIRTUAL STORAGE
                         │
                         ▼
                  PHYSICAL STORAGE
```

---

# 17. ඔයා මේ Topic එකෙන් අනිවාර්යයෙන් දැනගන්න ඕන Terms

මේවා **memorize කරනවාට වඩා practical lab එකකින් touch කරන්න**.

### Core

- Virtualization
    
- Hypervisor
    
- Type 1
    
- Type 2
    
- Host
    
- Guest
    
- VM
    
- vCPU
    
- vRAM
    
- vDisk
    
- vNIC
    

### Network

- Virtual Switch
    
- Bridge
    
- NAT
    
- VLAN
    
- Port Group
    
- Virtual Network
    
- Network Adapter
    
- Overlay Network
    

### Storage

- Datastore
    
- Virtual Disk
    
- Thin Provisioning
    
- Thick Provisioning
    
- Snapshot
    
- SAN
    
- NAS
    
- Shared Storage
    
- IOPS
    
- Latency
    
- Throughput
    

### Resource Management

- CPU Scheduling
    
- CPU Overcommit
    
- Memory Overcommit
    
- Reservation
    
- Limit
    
- Shares
    
- Resource Pool
    

### Availability

- Live Migration
    
- High Availability
    
- Fault Tolerance
    
- Cluster
    
- Failover
    

### Security

- VM Isolation
    
- Hypervisor Security
    
- Virtual Firewall
    
- Network Segmentation
    
- Secure Boot
    
- vTPM
    

---

# 18. මේක practically ඉගෙන ගන්න හොඳම sequence එක

ඔයා Architecture path එකේ නම් මේ order එකෙන් යන්න:

```text
00.4 Virtualization
│
├── 1. Virtualization fundamentals
│
├── 2. Hypervisor
│   ├── Type 1
│   └── Type 2
│
├── 3. VM
│   ├── vCPU
│   ├── vRAM
│   ├── vDisk
│   └── vNIC
│
├── 4. Virtual Network
│   ├── vSwitch
│   ├── NAT
│   ├── Bridge
│   ├── VLAN
│   └── Virtual Firewall
│
├── 5. Virtual Storage
│   ├── Datastore
│   ├── SAN
│   ├── NAS
│   ├── Thin
│   └── Thick
│
├── 6. Resource Allocation
│   ├── CPU
│   ├── RAM
│   ├── Storage
│   └── Network
│
├── 7. VM Operations
│   ├── Snapshot
│   ├── Clone
│   ├── Migration
│   └── Templates
│
├── 8. Availability
│   ├── HA
│   ├── Failover
│   └── Fault Tolerance
│
└── 9. Advanced
    ├── Clustering
    ├── Distributed Storage
    ├── SDN
    ├── Software-defined Storage
    └── Cloud Infrastructure
```

### අවසානයේ එකම picture එක:

**Physical Compute + Physical Network + Physical Storage → Hypervisor → Virtual Compute + Virtual Network + Virtual Storage → VMs → OS → Applications.**

මේ relationship එක solid වුණාම පස්සේ **VMware / Hyper-V / KVM / Proxmox / AWS EC2 / Azure VM / GCP Compute Engine / Kubernetes** වගේ technologies ඉගෙන ගන්නකොට ඒවා random tools වගේ පේන්නේ නැහැ. ඒවා මේ **එකම virtualization architecture එකේ different implementations/abstractions** කියලා පේන්න පටන් ගන්නවා.