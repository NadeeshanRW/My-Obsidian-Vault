<<<<<<< HEAD
 මේක **Linux distributions 4ක් නම කියලා memorize කරන එකක් නෙමෙයි**. Architecture/Infrastructure level එකට යනවා නම් තේරුම් ගන්න ඕනේ **“මේවා එකිනෙකට ඇයි වෙනස්? කොහෙද use කරන්නේ? Industry එකේ ඇයි එකක් තෝරන්නේ?”** කියන logic එක.

---

# 01.1 Linux — මුලින්ම Core Concept එක

මුලින්ම එක වැදගත් correction එකක්:

**Ubuntu, Debian, RHEL, Rocky, AlmaLinux කියන්නේ Linux නෙමෙයි. ඒවා Linux distributions (distros).**

Linux architecture එක roughly: 

```text
                    LINUX ECOSYSTEM
                          │
                          ▼
                    Linux Kernel
                          │
        ┌─────────────────┼──────────────────┐
        ▼                 ▼                  ▼
   Device Drivers     Networking          Process/
   CPU / RAM / Disk   TCP/IP              Memory
        │                 │                  │
        └─────────────────┼──────────────────┘
                          ▼
                   User Space
                          │
       ┌──────────────────┼───────────────────┐
       ▼                  ▼                   ▼
   systemd            shell/bash          libraries
       │                  │                   │
       └──────────────────┼───────────────────┘
                          ▼
                    Applications
                          │
       ┌──────────────────┼───────────────────┐
       ▼                  ▼                   ▼
    Nginx              Docker              Python
    Apache             Kubernetes          PostgreSQL
    SSH                HAProxy             etc.
```

**Linux Kernel** එක තමයි base.

Distro එකක් basically:

> Linux kernel + package manager + system tools + libraries + defaults + security model + support/lifecycle + ecosystem

ඒ නිසා:

```text
Linux
 │
 ├── Debian Family
 │     ├── Debian
 │     └── Ubuntu
 │
 └── Red Hat Family
       ├── RHEL
       ├── Rocky Linux
       └── AlmaLinux
```

මේ family concept එක තේරුම් ගත්තොත් distro landscape එක ගොඩක් ලේසි.

---

# 01.1.1 ඇයි Linux servers වල මෙච්චර popular?

Linux වල main strength එක **control + stability + automation + networking + ecosystem**.

Server එකක් කියන්නේ:

```text
Hardware
   ↓
Operating System
   ↓
Network
   ↓
Services
   ↓
Applications
   ↓
Users / Clients
```

Linux එක මේ layers අතර control එක ඉතා හොඳට දෙන්න පුළුවන්.

### Linux servers වල common workloads

```text
Web Servers
 ├── Nginx
 └── Apache

Application Servers
 ├── Java
 ├── Python
 ├── Node.js
 └── Go

Databases
 ├── PostgreSQL
 ├── MySQL
 └── MariaDB

Containers
 ├── Docker
 └── Kubernetes

Networking
 ├── DNS
 ├── DHCP
 ├── Proxy
 ├── Load Balancer
 └── VPN

Security
 ├── Firewall
 ├── IDS/IPS
 ├── SIEM agents
 └── Security monitoring

Cloud
 ├── AWS
 ├── Azure
 └── GCP
```

---

# 01.2 Debian

![Image](https://images.openai.com/static-rsc-4/DOv--yxrlSFCJUxxcK0nwvo5cUBvFEMCajypHiDWQLF6NWzUwLeTvUJebIgXWXL--uys6JxtI3BVCk0SOaG_OyBIqHfYncdvG8VWRiKK4h_glGWSCQrVHT2Mj8HyCfQfq_Z8sqftnz-b_5nSINSA4HK01rgjvJ7qrwsRr13qyjg3cdW__UyXbGitZ8ypuK13?purpose=fullsize)

![Image](https://images.openai.com/static-rsc-4/xkMeXFBGcmQ7HMzyoiRFfEf2ylXw79cV-X9E1qGu3Q5kjrdbl4rdBXD4uhUi84A6s_hYlTg8majykCBItFAisc4LxLN3B3lOREU9a58PMaFbEKuc8qOAWbL9svN0P-cL7y2cnugeD80LDG6agoUWx5sgxPMitmaNaaL1iSeUt9D1J3V4V0hb4qJ_AVkdHXmA?purpose=fullsize)

![Image](https://images.openai.com/static-rsc-4/YBj3c97HfmhGtnzzSv_WKqcbwfA0IBVZ7diiRUuIru7asPVLeA3DkugllX4Z5plKiqa6rjYP3ItD523aQV2Vg6Or1UGSbpge8X4zQDWPsplzhj70a3zFS_hvf2ahoIs-lxX366xbG_hbOd-kGZ1pFF7Vj12YHzOIF9d7uTbGEaeiN24z4OhobrM53OP_PslA?purpose=fullsize)

![Image](https://images.openai.com/static-rsc-4/SJXos4Pniyd066SN8v_UxGcYPsxC0LmYqI7nmGwflxObllmxiFUUoWuq2AyfxgDY1l4DM2RATIiQGs3XxLt29l1sNLz19TmE3sG-V1axQXHDBZwkKS3lFyzp7PxY1sVuGgbusJKUeEGbIlGvOTMPck6KGcUQJ00kg-Ut-b2nOnvAsHBwSd2WBSFsArLm3EO_?purpose=fullsize)

![Image](https://images.openai.com/static-rsc-4/Z0_Rw2ZLUW7is1Df3dY880q4us5IyCuOyJovWdhlOpUq3ocZP8UqjCJql6PK0c1xDK_WVJgIjnPJYQleo8r0SKOQqWcr1NZBc3b8o68Jb241n_wa2WTBPmjCYaSDwElBGCAzBy308DiAQqI0QFgaqNQgVWluYI5rAxOpyRcAYbDocA_MsnNBcljcgwNHFHNM?purpose=fullsize)

Debian කියන්නේ **old + stable + community-driven** Linux distribution එකක්.

Debian ecosystem එකේ philosophy එක:

> **Stability and freedom first.**

Debian directly තමන්ගේ packages aggressively latest version වලට push කරන්නේ නැහැ.

ඒක intentional.

---

## Debian architecture


Debian
 │
 ├── [[Linux system components#1. Linux Kernel 🧠|Linux Kernel]]
 │
 ├── [[Linux system components#2. systemd ⚙️|systemd]]
 │
 ├── [[Linux system components#3. APT 📦|APT]]
 │
 ├── [[Linux system components#4. DPKG|dpkg]]
 │
 ├── [[Linux system components#6. GNU Tools 🛠️|GNU tools]]
 │
 └── Debian packages
       └── .deb


Package management:

```text
apt
 │
 └── dpkg
```

Example:

```bash
apt update
apt upgrade
apt install nginx
apt remove nginx
```

---

# Debian ඇයි stable?

Imagine production server එකක්:

```text
Customer Application
        ↓
Production Server
        ↓
Database
        ↓
Millions of transactions
```

ඔයාට අවශ්‍ය නැහැ:

> "අද package update එකක් ආවා, application එක break වුණා."

Debian therefore prioritizes:

```text
Stability
   ↑
Predictability
   ↑
Testing
   ↑
Conservative package versions
```

මේකේ downside එක:

**Software versions sometimes older.**

උදාහරණයක්:

```text
Latest software = v5
Debian stable = v4
```

ඒක bug එකක් නෙමෙයි.

**Stability trade-off එකක්.**

---

# Debian use කරන්නේ කොහෙද?

විශේෂයෙන්:

- Servers
    
- Web servers
    
- Infrastructure
    
- DNS
    
- Network services
    
- Containers
    
- Labs
    
- VPS
    
- Small/medium businesses
    
- Privacy-focused environments
    
- Organizations wanting minimal licensing cost
    

Debian එකේ තවත් ලොකු strength එකක්:

**Ubuntu ecosystem එකට base එකක්.**

---

# 01.3 Ubuntu

![Image](https://images.openai.com/static-rsc-4/TQOP98cYjcqVeJCBfgnEgGdKTdvEq6mEjMXj25MFlbVLgYFUssiT_37glitcMvR2PELaKZZzQ4rGgU33CHXa_DjqwOmVTnKe7PS6oDFsEUw5XVSD-KBPRN4J6UbI1Hiwmmq-cIJJ6DYNX2DD7u0z_C1xxrv5SCGzctEgRweZmleTTYYuv9dQ3ffRUCXZgd8b?purpose=fullsize)

![Image](https://images.openai.com/static-rsc-4/vXXkn1gMN45AuYkYLi2_9ZKfPHWd08sbiJ6-KWO6BUwPUiyMwBI8bjcz9s7OjpCaTY62p21JtGwi-7bm__ERuU-6Ekm9S4X5QzllpNPGIR-TDDfNjdVXtFd-8hgheWujkccOU6lnqR10uV6r_UkHFvq8Kthw7qK4MPeKg2lT6wC5CRhPDKfs0ZsKez9YMEYC?purpose=fullsize)

![Image](https://images.openai.com/static-rsc-4/fzQJQ8jZZXCuQySQK5hhyDFmQb3y7JX8oJ2YHcCD-M7aS03Nn_MxQdbK6hfKtRZmeAsIcfADfNTkm_1B8HTHS3en7ArBmZKfRWfmE7g_Y9dD6DprENsCc9TGdWvCHuzI8T0DJjx4I1TFRq0NnrGJGFZIjY5p2l1WG_SOFjJsBZaX6QSrnKrTVCkPbWZlxT2o?purpose=fullsize)

![Image](https://images.openai.com/static-rsc-4/gO2HUiKv6XbfY2Qj8TUWbXLTT1xEP0xcr-9VoX0kUxkPK8Hdej-M2R26YtxXcgf3rocH_I_9txfnkbOQ6TxTM00Ua_S6FugXQRdGuuqXI6jGmI1iitRMArGxrJEKZ3MzodFKdBS-Nll652B_5MTVoomTDiy6jmRUM4fQ_fMUwzvJmNfgKrEtyMJKxmlMWytB?purpose=fullsize)

![Image](https://images.openai.com/static-rsc-4/oR-PHtwslxFICJ9W4pqWbEpUBLOnve6w2WhceZHRcZAaYSJFzbATfgni2Igd-6qEIuKmRnJBVNkFzl8InylNIFkcZ5k274wzKj5eiDRHoqmyg6gsz3c622qvbEQgNs4RCptIw2eHGYuSlppM-k3rV3kV0Q5fnl1OegjiAXCJRWjV78vT_pyDilqvMjQPAA-a?purpose=fullsize)

![Image](https://images.openai.com/static-rsc-4/BwkUeeggv0FPXOXUV9IOvOLjmUmLs_bLD4di9tDiOpzX93R6iEJQeDA5KpoxF87gu3Py03A92wXrhwsMTGQEI9MZ_IOfX5HOjmdryN6QkcaBDUUZNQic53WuVQI8BNua-2Z2YUCHTgKubZmKfZ8nOemA3L48CPFUKxZSkyzvdtHjF9QjGJzpx738jxRDidaP?purpose=fullsize)

![Image](https://images.openai.com/static-rsc-4/NX1yZSZrFKuyCjqFH4xwTQabcyuNlNYx37wJ97iw0s45MCMJsMJhBdbdKp_kTu7kPqWgM0JaX-yAcgUkif6RTFHKdlOpMWHtd6Pf6RzTULyJFA7-gJbpx2x2O2mnpLwEIjN_cNuOXtvNIRejZ7tvf8kDxWhhxmfWjqIo2dxIvIZMpkUqPBnDQUbdP0x-_OOP?purpose=fullsize)

Ubuntu කියන්නේ **Debian-based distribution එකක්**.

```text
Debian
   │
   └──────► Ubuntu
```

Ubuntu develop කරන්නේ Canonical.

Ubuntu philosophy එක roughly:

> Debian stability + easier usability + newer packages + commercial ecosystem

---

# Ubuntu ඇයි හදලා තියෙන්නේ?

Debian excellent.

නමුත් beginner / enterprise / cloud user කෙනෙකුට Debian ecosystem එක sometimes complex.

Ubuntu therefore focuses heavily on:

```text
Ease of use
+
Cloud
+
Developer ecosystem
+
Enterprise support
+
Frequent releases
```

---

# Ubuntu versions දෙකක් වැදගත්

## Ubuntu Desktop

```text
GUI
↓
Developer workstation
↓
Desktop applications
```

## Ubuntu Server

```text
CLI
↓
SSH
↓
Services
↓
Cloud
↓
Containers
```

Infrastructure engineer කෙනෙක්ට **Ubuntu Server** තමයි වැදගත්.

---

# Ubuntu LTS

මේක **අනිවාර්යයෙන් ඉගෙන ගන්න concept එකක්.**

LTS = **Long Term Support**

Production environment එකකට company එකක් කියන්නේ:

> "අපි මේ OS එක අද deploy කරලා අවුරුදු ගණනක් maintain කරන්න ඕන."

එතකොට LTS release එක useful.

Conceptually:

```text
Ubuntu release
      │
      ├── Regular
      │
      └── LTS
            │
            └── Long-term maintenance
```

Industry එකේ servers වල **Ubuntu LTS** common reason එක මේක.

---

# Ubuntu use වෙන තැන්


Cloud
 │
 ├── AWS
 ├── Azure
 └── GCP

DevOps
 │
 ├── Docker
 ├── Kubernetes
 ├── Terraform
 └── [[Ansible|Ansible]]

Web
 │
 ├── Nginx
 ├── Apache
 └── HAProxy

Development
 │
 ├── Python
 ├── Node.js
 ├── Java
 └── Go

AI / ML
 │
 ├── CUDA
 ├── PyTorch
 └── TensorFlow

**Cloud + DevOps + Kubernetes path එකකට Ubuntu දැනගැනීම ඉතා useful.**

---

# Debian vs Ubuntu

මේ දෙකේ relationship එක:

```text
                 Debian
                    │
          ┌─────────┴─────────┐
          │                   │
       Stable              Ubuntu
                              │
                       ┌──────┴──────┐
                       │             │
                    Desktop        Server
                                      │
                                   Cloud
```

Simple way එකට:

|Feature|Debian|Ubuntu|
|---|---|---|
|Base|Independent|Debian-based|
|Philosophy|Stability|Usability + ecosystem|
|Packages|Conservative|Generally newer|
|Beginner friendly|Medium|High|
|Cloud|Good|Excellent|
|DevOps|Good|Excellent|
|Enterprise support|Community|Commercial option|
|Package|`.deb`|`.deb`|
|Package manager|APT|APT|

---

# 01.4 RHEL

![Image](https://images.openai.com/static-rsc-4/cJoccTYkzQ0JEOb3Rlw88tEc_zFbnwbZH9gTnNhy-k6MTH1iLbWm5ncn29fBEQSrJtiuEXnIB0LdMQpS1cw3_xOJOUqwZPy5QQP7-v3DUj9MBco3M5YBBUmK7VQWKGKsSrs1_mf-YM0B6dLOaAXJqgaEAOR_Sx3Jv12o6DRLQuLDc2j8PO2LLQu4KmPqiXte?purpose=fullsize)

![Image](https://images.openai.com/static-rsc-4/_R9eyPIu9lrISRrwR3K3jLWb7qbAwwewSFuFBKqpXp1aZVB09SbH7dCITBWtzN6RqgNniI3JKvW7l-jyeyb9memhQIC9JuGI9iiDvSqhPv1VjbUKGWVOZAIHfR1ko0VCj6H43w-ugoyIXLNNEMJYvVwteXZWTAEvJIEKKpBALpAH8bl56M1q-cxIwBaMoaK_?purpose=fullsize)

![Image](https://images.openai.com/static-rsc-4/gWaroa4dnXoZ6Lg23Woiea6Py0esAw9XSXi0oFTILAwTOWlzRg2EY2XNjxaCBL4XooGGUOor3uHIIRz7sF8VHytFsmXX6oqn1G2jtXynO5DqSXLHPCDCjDriofZskNIav6tYI-ZdOJnWWWOOF_wP4TR5NrHFKErzzNQHIYNdfYREmhN0yY8BvSsUmoOv5XNF?purpose=fullsize)

![Image](https://images.openai.com/static-rsc-4/nxjNfTMc6cY3M3nXShTu7tfNdm6oJhmJ5iC2ula1tz2f1eqN8cRyvAqSPvotzpMDwv27Rt_uLJgl6L9wjl1GlHOVLHr3K-wjrZ_xu4c2UxpsXGjE6hC0XOxqib781lyAPtMyIa1DyupY_y9DJxXYAlPImUmASQyrP99F7lfw8mtNXkvFoPNGHBfhIpgi59t7?purpose=fullsize)

![Image](https://images.openai.com/static-rsc-4/olE38M8SDBw6VtlAU_KYuqgGn5pskODGAB-6WLpB9lEcYm_bCBYd1dKgq607bhA2Q4dEzwBbr1RG4XIqL7xPhqN9-oGtA_Y-KkgRQR0gBi2tdAu7-4ofYqgpadyh2KLp_J-k4Oqu5GEj0SS3C5LT2YwW8I3pPPKLE1nIfvZcdLxgrnIOyvSt702IZAkB1-PL?purpose=fullsize)

දැන් අපි **වෙනම world එකකට** එනවා.

RHEL = **Red Hat Enterprise Linux**

මේක develop/distribute කරන්නේ Red Hat.

RHEL philosophy එක:

> **Enterprise stability + security + certification + lifecycle + support**

Ubuntu vs RHEL difference එක තේරුම් ගන්න මේක බලන්න:

```text
Ubuntu ecosystem
      │
      ├── Cloud
      ├── Developer
      └── Open-source ecosystem

RHEL ecosystem
      │
      ├── Enterprise
      ├── Government
      ├── Banking
      ├── Telecom
      ├── Datacenter
      ├── Compliance
      └── Vendor certification
```

---

# RHEL ඇයි companies use කරන්නේ?

Company එකකට server එකක් තියෙනවා කියලා හිතන්න.

```text
Bank
 │
 ├── Application
 ├── Database
 ├── Security
 ├── Compliance
 └── Audit
```

Bank එකට ප්‍රශ්නය:

> "Linux free ද?"

එක විතරක් නෙමෙයි.

ඔවුන්ට අවශ්‍ය:

```text
Who supports this?
       ↓
Security updates?
       ↓
Patch lifecycle?
       ↓
Compliance?
       ↓
Vendor certification?
       ↓
Enterprise support?
       ↓
Predictable lifecycle?
```

මෙතන RHEL strong.

---

# RHEL ecosystem

මේක architecture level එකේ **ගොඩක් වැදගත්**:

```text
                 Red Hat
                    │
                    ▼
                  RHEL
                    │
        ┌───────────┼────────────┐
        ▼           ▼            ▼
      RPM         DNF/YUM      SELinux
        │           │            │
        └───────────┼────────────┘
                    ▼
              Enterprise
               Workloads
```

Package:

```text
.rpm
```

Package manager:

```text
dnf
```

Old environments වල:

```text
yum
```

Security:

```text
SELinux
```

[[SELinux , AppArmor#3. SELinux 🔐|SELinux]] කියන්නේ RHEL ecosystem එකේ **must-know** concept එකක්.

---

# [[SELinux , AppArmor#3. SELinux 🔐|SELinux]] කියන්නේ මොකක්ද?

Linux permissions normally:

```text
User
 │
 └── File permissions
       ├── Read
       ├── Write
       └── Execute
```

[[SELinux , AppArmor#3. SELinux 🔐|SELinux]] එක additional security layer එකක්.

```text
User
 ↓
Linux permissions
 ↓
SELinux policy
 ↓
Application
 ↓
Resource
```

ඒකෙන් application එකට:

> "මේ file එක technically readable වුණත් policy එකෙන් allow කරන්නේ නැත්නම් access කරන්න බෑ."

Enterprise security වල මේක valuable.

---

# RHEL use වෙන industry

Especially:

### Banking

```text
Core Banking
Payment systems
Databases
Middleware
```

### Telecom

```text
Network services
OSS/BSS
5G infrastructure
Databases
```

### Government

```text
Critical systems
Secure infrastructure
Compliance
```

### Enterprise

```text
SAP
Oracle
VMware-related infrastructure
Databases
Middleware
Internal applications
```

### Cloud

RHEL cloud images exist on major cloud providers.

---

# 01.5 Rocky Linux

![Image](https://images.openai.com/static-rsc-4/gMGbcLvV9TXsnuOFvnxCTww5GaLLoOdlWeonB_QIOYUdrYxQHUufjMJzIGazhGusIaeRiN4-7Jq5P7ExGEOR-ZTa6DYIZWd4GGHuJmgvUiKzChwPZ9T4YkGFMFIKXmThwE263l_7Dv4km7BYRW_V3JrX0pLf_taHbggEb7giuDF-VI9uoWAQwNVTxUyxJkgY?purpose=fullsize)

![Image](https://images.openai.com/static-rsc-4/gIeDiERB2bJ0s4t-FI2U2iB1mm-ae-Eu0TrSvYXr0j9AEPI7ZIU4mbKVcDyCpER9x5b5T46r-dxFbXeigJP13hiws8_u0FjRBIrCOWZ30KUV_PBdb04hHJZKXHpKNEUZMgQCkAyj1NPxFPey-1cRKupfmmJQ0xsJoyEzJtNvEnBElMuL8xWvqeWSNoC2Rbw2?purpose=fullsize)

![Image](https://images.openai.com/static-rsc-4/W5qFmI7gqpRM9SmpCniVv3lYp_bfBvtDgpA0OiUFWQEUOP--eNQZn0rS_YvfYhF0SlhRaJ68TWul1xhAwRRU5s0iMJcKJMkcPFjiK2V5zL5uwutUU9_8sWP6C0h0gpIm4iGGFE5nPUvmHeS1UZWVELO0VuRZ6z3TRFHiIBMYz4hwCsC-y8dZ4L6pYNaF-oVK?purpose=fullsize)

![Image](https://images.openai.com/static-rsc-4/dehwa-Tzbpscn9omiBgkqjibjZbz7biBafl99md6GZlAa37sST3rw1dyEazZBs_m4G2eZxC33d1gTBHwEwjLsWkW_9cGA7LObQPU5ta38ArEiF9omRpxsrEXu-rdoWacXJnJVoHV5rELr8Nuiht6y5gl2NNg_90pobYjiFTMn9QZwgJupaW_VDIMyKhkA6yt?purpose=fullsize)

![Image](https://images.openai.com/static-rsc-4/Rq7TQrXjB24LKQ0WQfG2gxhdkgX0k9lbHGYDySkUNTA-fx7sKhgBZjqptr94Flck4Np7vUvUpN7hUQaHpqfnGDu8R3p9KRq01lb-17Q8UlGoEBKlIx31KPaZx6k8g4xcvoZUZfJuG_icUN0Ujy2QoThkbTQxngPrd7dfBygAYCPdp3wS_U9ORXiW-cp4tbtZ?purpose=fullsize)

දැන් interesting part එක.

Rocky Linux කියන්නේ **RHEL-compatible enterprise Linux** එකක්.

Concept එක:

```text
Enterprise Linux ecosystem
          │
          ▼
         RHEL
          │
          │
      Compatible
          │
     ┌────┴────┐
     ▼         ▼
 Rocky       Alma
 Linux       Linux
```

Rocky Linux goal එක:

> RHEL ecosystem එකට compatible, community-driven alternative එකක්.

---

# ඇයි Rocky හදන්න ඕන වුණේ?

CentOS historically RHEL ecosystem එකේ very popular.

Conceptually:

```text
RHEL
 │
 └── CentOS
       │
       └── RHEL-compatible ecosystem
```

CentOS project direction එක වෙනස් වුණා.

ඒකෙන් community එකට අවශ්‍ය වුණා:

> "RHEL-compatible, community-driven enterprise OS එකක්."

ඒ environment එකෙන් **Rocky Linux** සහ **AlmaLinux** වගේ projects important වුණා.

---

# Rocky Linux use කරන්නේ කොහෙද?

```text
Enterprise Lab
      ↓
Production Server
      ↓
Web Server
      ↓
Database
      ↓
Virtualization
      ↓
Containers
      ↓
HPC
```

ඒක especially useful:

- RHEL skills practice
    
- Enterprise Linux labs
    
- Production workloads
    
- Hosting
    
- Servers
    
- Infrastructure
    
- Cost-sensitive environments
    

---

# 01.6 AlmaLinux

![Image](https://images.openai.com/static-rsc-4/uEymqNjuscTQYesm4u_IVntCSbCoP-H8rBqRosY_3T81v178UM4Lfi-5_Cp05bx247RKQtdSwYYmlojLpk1Lg1wBoBVj8KSs73x2IrZUfGov7z9hiZb588ER3CV8NJHwoFx-gJ3peQsv5ASnrDLwOwU6olxgcDM56LoiKSwvGa6X1mKZPvezHq06oBcECCyy?purpose=fullsize)

![Image](https://images.openai.com/static-rsc-4/-kCuhf0KXexB6V7EoLWVBxObLZUM7QtcWqg29KQpa2F75DQHHEQzzYFVoGmDgIbVuWU_MvYcX4Wjt7EtUOh1MI_iyzGGZ5LB43CQM_cdwNQw9pPU5A2rcD1J0C7EnJVC5PKGsiHJpWYgvbP80p-Nlsrsa4qVfgsX_55R70rqRQPSDe9f0By-1EiauC2GXV40?purpose=fullsize)

![Image](https://images.openai.com/static-rsc-4/7TtN_pQE-gpKohqoNzR_QocHWpH-3kcducKgu5REemM1vQ2ZzIZs48Hnkw0-uupUYG8Uwz4VRpMvHMpEHkOGZIP1znj_Wi4rO7KNo3NPL5pABlCKMueBlGFcryrBlaKQ7oVXatDsK5SV6X62fx2qXxhq87XWf8H2kpIc7Kk9MoLeL8odjm0BbT-F5DStCj3k?purpose=fullsize)

![Image](https://images.openai.com/static-rsc-4/bpJxwIqeT_mbvyOODXz0sZcFPwfs2czMn8ocZse9uOj7aMJ-I7mBq7pyW5yWsvroD78Mu3kH4RuvLqG4xMmcegNLw1vBP2vVOum8O8_MZsqUO4L77-6-crIZBovG0xNtZSfEUx01GYN9f3h0rsD47EVtZNzFCgICZW8S6PSCmRdSnpzeWcmySaXZBAWGN93n?purpose=fullsize)

![Image](https://images.openai.com/static-rsc-4/KkehGRsmfHPqN7a5ltbTsAz3--Hq7JHgccbx4L2xkC_v_IrIJFRXMojY6cps_VfFpc_oGE_N8UX6uZ5BLPDl6hGuOWfukBybekp-WxfRTRHbZHgq6l1NnVPaK3lHs85S4YDNMywvYg6TReIX6KB1jvYHv_UBFQ06jr1UI6llvJ0YxiEm2ExEt3G_tdxJDlRL?purpose=fullsize)

AlmaLinux එකත් **RHEL-compatible enterprise Linux** ecosystem එකේ.

So:

```text
RHEL
 │
 ├── Rocky Linux
 │
 └── AlmaLinux
```

නමුත් මේක:

> "Rocky = bad, Alma = good"

වගේ comparison එකක් නෙමෙයි.

දෙකම enterprise Linux ecosystem එකේ useful alternatives.

---

# Rocky vs Alma

දෙකේ basic philosophy එක similar.

```text
                    Enterprise Linux
                           │
             ┌─────────────┼─────────────┐
             │             │             │
            RHEL         Rocky         Alma
             │
        Commercial
         support
```

Difference එක mostly:

- project governance
    
- community
    
- ecosystem integrations
    
- organizational preferences
    
- vendor relationships
    
- release/build processes
    

**Architecture learner කෙනෙක්ට මුලින් දෙකම deep-dive කරන එක priority නෙමෙයි.**

මුලින්:

> **RHEL ecosystem එක understand කරන්න.**

ඊට පස්සේ Rocky/Alma.

---

# 01.7 මේ 4 දේ actually කොහොමද වෙනස්?

මේක තමයි ඔයාගේ question එකේ heart එක.

```text
                 Linux Kernel
                      │
          ┌───────────┴───────────┐
          │                       │
      Debian Family          Red Hat Family
          │                       │
       Debian                    RHEL
          │                       │
       Ubuntu              ┌──────┴──────┐
                           │             │
                         Rocky         Alma
```

---

# Package management difference

මේක **අනිවාර්යයෙන් practical කරලා ඉගෙන ගන්න.**

## Debian / Ubuntu

```text
Package format
      ↓
    .deb

Package database
      ↓
    dpkg

High-level package manager
      ↓
     apt
```

Examples:

```bash
apt update
apt upgrade
apt install nginx
```

---

## RHEL / Rocky / Alma

```text
Package format
      ↓
    .rpm

Package manager
      ↓
     dnf
```

Examples:

```bash
dnf update
dnf install nginx
dnf remove nginx
```

---

# 01.8 Filesystem එක වෙනස්ද?

මෙන්න important point එක:

**Basic Linux filesystem concepts mostly same.**

```text
/
├── boot
├── etc
├── home
├── var
├── usr
├── opt
├── tmp
├── dev
├── proc
└── sys
```

Ubuntu server එකකත් මෙහෙම.

RHEL server එකකත් මෙහෙම.

Rocky එකකත් මෙහෙම.

Alma එකකත් මෙහෙම.

ඒ නිසා distro 4ක් ඉගෙන ගන්නවා කියලා:

> "Filesystem 4ක් වෙන වෙනම ඉගෙන ගන්න ඕන"

නෑ.

**Linux fundamentals එක පාරක් solid කරගන්න.**

---

# 01.9 systemd

මේකත් distro 4ටම common concept එකක්.

Modern Linux:

```text
systemd
   │
   ├── services
   ├── boot
   ├── logging integration
   ├── processes
   └── system state
```

Examples:

```bash
systemctl status nginx
systemctl start nginx
systemctl stop nginx
systemctl restart nginx
systemctl enable nginx
```

මේ commands Ubuntu/RHEL/Rocky/Alma වල conceptually same.

---

# 01.10 Networking

Architecture level එකට යනවා නම් Linux networking **extremely important**.

```text
Linux Network Stack
       │
       ▼
Network Interface
       │
       ▼
IP Address
       │
       ▼
Routing Table
       │
       ▼
ARP / Neighbor
       │
       ▼
TCP / UDP
       │
       ▼
Application
```

Commands:

```bash
ip addr
ip route
ss
ping
traceroute
tcpdump
```

මේවා distro එකකට වඩා **Linux fundamentals**.

---

# 01.11 Security

Linux security layer එක:

```text
User
 │
 ▼
Authentication
 │
 ▼
Authorization
 │
 ├── File permissions
 ├── sudo
 ├── ACL
 └── capabilities
 │
 ▼
MAC
 ├── SELinux
 └── AppArmor
 │
 ▼
Firewall
 ├── nftables
 └── firewalld / ufw
 │
 ▼
Application
```

මෙතන distro difference එක එනවා.

### Ubuntu

Commonly:

```text
AppArmor
ufw
```

### RHEL family

Commonly:

```text
SELinux
firewalld
```

ඒ නිසා:

```text
Ubuntu security mindset
       ≠
RHEL security implementation
```

නමුත් underlying Linux security principles same.

---

# 01.12 Ubuntu vs RHEL — Industry එකේ biggest difference

මේක හොඳට මතක තියාගන්න.

### Ubuntu ecosystem

```text
Cloud
DevOps
Startups
Developers
Containers
Kubernetes
AI/ML
Modern infrastructure
```

### RHEL ecosystem

```text
Enterprise
Banking
Telecom
Government
SAP
Large datacenters
Compliance
Vendor-supported infrastructure
```

**මේක absolute rule එකක් නෙමෙයි.**

Enterprise එකක Ubuntu තියෙන්න පුළුවන්.

Startup එකක RHEL තියෙන්න පුළුවන්.

නමුත් **general industry pattern** එකක් විදිහට මේක useful.

---

# 01.13 ඇයි companies එක OS එකක් තෝරන්නේ?

මේක architecture mindset එකෙන් බලන්න.

Company එක Linux distro තෝරන්නේ:

```text
                OS Selection
                     │
      ┌──────────────┼──────────────┐
      ▼              ▼              ▼
   Technical      Business       Operational
      │              │              │
      ▼              ▼              ▼
 Performance       Cost          Support
 Security          Licensing     Lifecycle
 Compatibility     Risk          Skills
 Applications      Compliance    Automation
```

ඒ කියන්නේ:

> "Ubuntu හොඳද RHEL හොඳද?"

කියන question එක incomplete.

Correct question:

> **"මේ workload එකට, මේ organization එකට, මේ risk model එකට, මේ ecosystem එකට best OS එක මොකක්ද?"**

---

# 01.14 Real-world example

හිතන්න company එකක් තියෙනවා:

```text
                Internet
                    │
                 Firewall
                    │
             Load Balancer
                    │
         ┌──────────┴──────────┐
         │                     │
     Web Server             Web Server
      Ubuntu                 Ubuntu
         │                     │
         └──────────┬──────────┘
                    │
              Application
                    │
                  RHEL
                    │
                Database
                    │
                  RHEL
```

ඇයි මෙහෙම?

Web layer එක:

```text
Ubuntu
↓
Developer ecosystem
↓
Cloud friendly
↓
Fast deployment
```

Enterprise backend:

```text
RHEL
↓
Vendor support
↓
Security
↓
Compliance
↓
Enterprise software
```

**එක organization එකක distro දෙකම තියෙන්න පුළුවන්.**

---

# 01.15 Architecture level එකේ තවත් වැදගත් concept එකක්

Distro එක තෝරලා ඉවර නෑ.

ඊට පස්සේ:

```text
OS
 │
 ├── Compute
 │
 ├── Memory
 │
 ├── Storage
 │
 ├── Networking
 │
 ├── Security
 │
 └── Services
```

ඊට පස්සේ:

```text
Linux
 │
 ├── Virtual Machines
 │
 ├── Containers
 │
 ├── Kubernetes
 │
 ├── Cloud
 │
 └── Bare Metal
```

ඊට පස්සේ:

```text
Infrastructure
      │
      ▼
Platform
      │
      ▼
Applications
      │
      ▼
Users
```

මේ connection එක තේරුම් ගත්තොත් තමයි **Architecture thinking** develop වෙන්නේ.

---

# 01.16 ඔයාට මේ 4 distro ඉගෙන ගන්න priority එක

මම නම් ඔයාට මෙහෙම order එකක් දෙන්නෙ:

```text
                  Linux
                    │
                    ▼
            Linux Fundamentals
                    │
        ┌───────────┴───────────┐
        ▼                       ▼
 Debian Family             RHEL Family
        │                       │
     Ubuntu                    RHEL
        │                       │
        │                ┌──────┴──────┐
        │                ▼             ▼
        │              Rocky         Alma
        │
        ▼
 Cloud / DevOps
```

### Level 1 — Linux fundamentals

මේවා **deep**:

```text
1. Filesystem
2. Users & Groups
3. Permissions
4. Processes
5. systemd
6. Package management
7. Networking
8. SSH
9. Logs
10. Storage
11. Security
12. Bash
13. Cron
14. Services
15. Troubleshooting
```

---

### Level 2 — Ubuntu

Deep:

```text
Ubuntu Server
APT
systemd
Netplan
UFW
AppArmor
cloud-init
SSH
Docker
Kubernetes basics
```

---

### Level 3 — RHEL

Deep:

```text
RHEL
RPM
DNF
systemd
SELinux
firewalld
NetworkManager
nmcli
LVM
SSH
journald
```

---

### Level 4 — Rocky / Alma

මෙතන objective එක:

> **RHEL knowledge එක practically transfer කරන්න.**

ඒ නිසා Rocky/Alma වලට මුලින්ම වෙනම මාස ගණන් දාන්න එපා.

---

# 01.17 Commands එකින් difference එක බලමු

### Install Nginx

Ubuntu:

```bash
sudo apt update
sudo apt install nginx
```

RHEL/Rocky/Alma:

```bash
sudo dnf install nginx
```

---

### Service

දෙකේම:

```bash
sudo systemctl start nginx
sudo systemctl enable nginx
sudo systemctl status nginx
```

**මේක තමයි important lesson එක.**

Package management වෙනස්.

But:

```text
Linux fundamentals
       ↓
Shared
```

---

# 01.18 Architecture engineer කෙනෙක්ට මොනවාද really වැදගත්?

Distro names memorize කරන එකට වඩා මේ matrix එක තේරුම් ගන්න:

|Layer|Learn|
|---|---|
|Kernel|Linux kernel concepts|
|Process|process/thread/signals|
|Memory|RAM/swap/virtual memory|
|Storage|disk/LVM/filesystem|
|Network|IP/routes/TCP/UDP|
|Security|permissions/SELinux/AppArmor|
|Services|systemd|
|Packages|APT vs DNF|
|Logs|journald/log files|
|Automation|Bash/Ansible|
|Virtualization|KVM/VM concepts|
|Containers|Docker/containerd|
|Orchestration|Kubernetes|
|Cloud|AWS/Azure/GCP|
|Observability|logs/metrics/traces|
|Troubleshooting|CPU/RAM/disk/network|

මේවා දන්නවා නම් distro එක මාරු වුණත් ඔයා lost වෙන්නේ නෑ.

---
=======
 මේක **Linux distributions 4ක් නම කියලා memorize කරන එකක් නෙමෙයි**. Architecture/Infrastructure level එකට යනවා නම් තේරුම් ගන්න ඕනේ **“මේවා එකිනෙකට ඇයි වෙනස්? කොහෙද use කරන්නේ? Industry එකේ ඇයි එකක් තෝරන්නේ?”** කියන logic එක.

---

# 01.1 Linux — මුලින්ම Core Concept එක

මුලින්ම එක වැදගත් correction එකක්:

**Ubuntu, Debian, RHEL, Rocky, AlmaLinux කියන්නේ Linux නෙමෙයි. ඒවා Linux distributions (distros).**

Linux architecture එක roughly: 

```text
                    LINUX ECOSYSTEM
                          │
                          ▼
                    Linux Kernel
                          │
        ┌─────────────────┼──────────────────┐
        ▼                 ▼                  ▼
   Device Drivers     Networking          Process/
   CPU / RAM / Disk   TCP/IP              Memory
        │                 │                  │
        └─────────────────┼──────────────────┘
                          ▼
                   User Space
                          │
       ┌──────────────────┼───────────────────┐
       ▼                  ▼                   ▼
   systemd            shell/bash          libraries
       │                  │                   │
       └──────────────────┼───────────────────┘
                          ▼
                    Applications
                          │
       ┌──────────────────┼───────────────────┐
       ▼                  ▼                   ▼
    Nginx              Docker              Python
    Apache             Kubernetes          PostgreSQL
    SSH                HAProxy             etc.
```

**Linux Kernel** එක තමයි base.

Distro එකක් basically:

> Linux kernel + package manager + system tools + libraries + defaults + security model + support/lifecycle + ecosystem

ඒ නිසා:

```text
Linux
 │
 ├── Debian Family
 │     ├── Debian
 │     └── Ubuntu
 │
 └── Red Hat Family
       ├── RHEL
       ├── Rocky Linux
       └── AlmaLinux
```

මේ family concept එක තේරුම් ගත්තොත් distro landscape එක ගොඩක් ලේසි.

---

# 01.1.1 ඇයි Linux servers වල මෙච්චර popular?

Linux වල main strength එක **control + stability + automation + networking + ecosystem**.

Server එකක් කියන්නේ:

```text
Hardware
   ↓
Operating System
   ↓
Network
   ↓
Services
   ↓
Applications
   ↓
Users / Clients
```

Linux එක මේ layers අතර control එක ඉතා හොඳට දෙන්න පුළුවන්.

### Linux servers වල common workloads

```text
Web Servers
 ├── Nginx
 └── Apache

Application Servers
 ├── Java
 ├── Python
 ├── Node.js
 └── Go

Databases
 ├── PostgreSQL
 ├── MySQL
 └── MariaDB

Containers
 ├── Docker
 └── Kubernetes

Networking
 ├── DNS
 ├── DHCP
 ├── Proxy
 ├── Load Balancer
 └── VPN

Security
 ├── Firewall
 ├── IDS/IPS
 ├── SIEM agents
 └── Security monitoring

Cloud
 ├── AWS
 ├── Azure
 └── GCP
```

---

# 01.2 Debian

![Image](https://images.openai.com/static-rsc-4/DOv--yxrlSFCJUxxcK0nwvo5cUBvFEMCajypHiDWQLF6NWzUwLeTvUJebIgXWXL--uys6JxtI3BVCk0SOaG_OyBIqHfYncdvG8VWRiKK4h_glGWSCQrVHT2Mj8HyCfQfq_Z8sqftnz-b_5nSINSA4HK01rgjvJ7qrwsRr13qyjg3cdW__UyXbGitZ8ypuK13?purpose=fullsize)

![Image](https://images.openai.com/static-rsc-4/xkMeXFBGcmQ7HMzyoiRFfEf2ylXw79cV-X9E1qGu3Q5kjrdbl4rdBXD4uhUi84A6s_hYlTg8majykCBItFAisc4LxLN3B3lOREU9a58PMaFbEKuc8qOAWbL9svN0P-cL7y2cnugeD80LDG6agoUWx5sgxPMitmaNaaL1iSeUt9D1J3V4V0hb4qJ_AVkdHXmA?purpose=fullsize)

![Image](https://images.openai.com/static-rsc-4/YBj3c97HfmhGtnzzSv_WKqcbwfA0IBVZ7diiRUuIru7asPVLeA3DkugllX4Z5plKiqa6rjYP3ItD523aQV2Vg6Or1UGSbpge8X4zQDWPsplzhj70a3zFS_hvf2ahoIs-lxX366xbG_hbOd-kGZ1pFF7Vj12YHzOIF9d7uTbGEaeiN24z4OhobrM53OP_PslA?purpose=fullsize)

![Image](https://images.openai.com/static-rsc-4/SJXos4Pniyd066SN8v_UxGcYPsxC0LmYqI7nmGwflxObllmxiFUUoWuq2AyfxgDY1l4DM2RATIiQGs3XxLt29l1sNLz19TmE3sG-V1axQXHDBZwkKS3lFyzp7PxY1sVuGgbusJKUeEGbIlGvOTMPck6KGcUQJ00kg-Ut-b2nOnvAsHBwSd2WBSFsArLm3EO_?purpose=fullsize)

![Image](https://images.openai.com/static-rsc-4/Z0_Rw2ZLUW7is1Df3dY880q4us5IyCuOyJovWdhlOpUq3ocZP8UqjCJql6PK0c1xDK_WVJgIjnPJYQleo8r0SKOQqWcr1NZBc3b8o68Jb241n_wa2WTBPmjCYaSDwElBGCAzBy308DiAQqI0QFgaqNQgVWluYI5rAxOpyRcAYbDocA_MsnNBcljcgwNHFHNM?purpose=fullsize)

Debian කියන්නේ **old + stable + community-driven** Linux distribution එකක්.

Debian ecosystem එකේ philosophy එක:

> **Stability and freedom first.**

Debian directly තමන්ගේ packages aggressively latest version වලට push කරන්නේ නැහැ.

ඒක intentional.

---

## Debian architecture


Debian
 │
 ├── [[Linux system components#1. Linux Kernel 🧠|Linux Kernel]]
 │
 ├── [[Linux system components#2. systemd ⚙️|systemd]]
 │
 ├── [[Linux system components#3. APT 📦|APT]]
 │
 ├── [[Linux system components#4. DPKG|dpkg]]
 │
 ├── [[Linux system components#6. GNU Tools 🛠️|GNU tools]]
 │
 └── Debian packages
       └── .deb


Package management:

```text
apt
 │
 └── dpkg
```

Example:

```bash
apt update
apt upgrade
apt install nginx
apt remove nginx
```

---

# Debian ඇයි stable?

Imagine production server එකක්:

```text
Customer Application
        ↓
Production Server
        ↓
Database
        ↓
Millions of transactions
```

ඔයාට අවශ්‍ය නැහැ:

> "අද package update එකක් ආවා, application එක break වුණා."

Debian therefore prioritizes:

```text
Stability
   ↑
Predictability
   ↑
Testing
   ↑
Conservative package versions
```

මේකේ downside එක:

**Software versions sometimes older.**

උදාහරණයක්:

```text
Latest software = v5
Debian stable = v4
```

ඒක bug එකක් නෙමෙයි.

**Stability trade-off එකක්.**

---

# Debian use කරන්නේ කොහෙද?

විශේෂයෙන්:

- Servers
    
- Web servers
    
- Infrastructure
    
- DNS
    
- Network services
    
- Containers
    
- Labs
    
- VPS
    
- Small/medium businesses
    
- Privacy-focused environments
    
- Organizations wanting minimal licensing cost
    

Debian එකේ තවත් ලොකු strength එකක්:

**Ubuntu ecosystem එකට base එකක්.**

---

# 01.3 Ubuntu

![Image](https://images.openai.com/static-rsc-4/TQOP98cYjcqVeJCBfgnEgGdKTdvEq6mEjMXj25MFlbVLgYFUssiT_37glitcMvR2PELaKZZzQ4rGgU33CHXa_DjqwOmVTnKe7PS6oDFsEUw5XVSD-KBPRN4J6UbI1Hiwmmq-cIJJ6DYNX2DD7u0z_C1xxrv5SCGzctEgRweZmleTTYYuv9dQ3ffRUCXZgd8b?purpose=fullsize)

![Image](https://images.openai.com/static-rsc-4/vXXkn1gMN45AuYkYLi2_9ZKfPHWd08sbiJ6-KWO6BUwPUiyMwBI8bjcz9s7OjpCaTY62p21JtGwi-7bm__ERuU-6Ekm9S4X5QzllpNPGIR-TDDfNjdVXtFd-8hgheWujkccOU6lnqR10uV6r_UkHFvq8Kthw7qK4MPeKg2lT6wC5CRhPDKfs0ZsKez9YMEYC?purpose=fullsize)

![Image](https://images.openai.com/static-rsc-4/fzQJQ8jZZXCuQySQK5hhyDFmQb3y7JX8oJ2YHcCD-M7aS03Nn_MxQdbK6hfKtRZmeAsIcfADfNTkm_1B8HTHS3en7ArBmZKfRWfmE7g_Y9dD6DprENsCc9TGdWvCHuzI8T0DJjx4I1TFRq0NnrGJGFZIjY5p2l1WG_SOFjJsBZaX6QSrnKrTVCkPbWZlxT2o?purpose=fullsize)

![Image](https://images.openai.com/static-rsc-4/gO2HUiKv6XbfY2Qj8TUWbXLTT1xEP0xcr-9VoX0kUxkPK8Hdej-M2R26YtxXcgf3rocH_I_9txfnkbOQ6TxTM00Ua_S6FugXQRdGuuqXI6jGmI1iitRMArGxrJEKZ3MzodFKdBS-Nll652B_5MTVoomTDiy6jmRUM4fQ_fMUwzvJmNfgKrEtyMJKxmlMWytB?purpose=fullsize)

![Image](https://images.openai.com/static-rsc-4/oR-PHtwslxFICJ9W4pqWbEpUBLOnve6w2WhceZHRcZAaYSJFzbATfgni2Igd-6qEIuKmRnJBVNkFzl8InylNIFkcZ5k274wzKj5eiDRHoqmyg6gsz3c622qvbEQgNs4RCptIw2eHGYuSlppM-k3rV3kV0Q5fnl1OegjiAXCJRWjV78vT_pyDilqvMjQPAA-a?purpose=fullsize)

![Image](https://images.openai.com/static-rsc-4/BwkUeeggv0FPXOXUV9IOvOLjmUmLs_bLD4di9tDiOpzX93R6iEJQeDA5KpoxF87gu3Py03A92wXrhwsMTGQEI9MZ_IOfX5HOjmdryN6QkcaBDUUZNQic53WuVQI8BNua-2Z2YUCHTgKubZmKfZ8nOemA3L48CPFUKxZSkyzvdtHjF9QjGJzpx738jxRDidaP?purpose=fullsize)

![Image](https://images.openai.com/static-rsc-4/NX1yZSZrFKuyCjqFH4xwTQabcyuNlNYx37wJ97iw0s45MCMJsMJhBdbdKp_kTu7kPqWgM0JaX-yAcgUkif6RTFHKdlOpMWHtd6Pf6RzTULyJFA7-gJbpx2x2O2mnpLwEIjN_cNuOXtvNIRejZ7tvf8kDxWhhxmfWjqIo2dxIvIZMpkUqPBnDQUbdP0x-_OOP?purpose=fullsize)

Ubuntu කියන්නේ **Debian-based distribution එකක්**.

```text
Debian
   │
   └──────► Ubuntu
```

Ubuntu develop කරන්නේ Canonical.

Ubuntu philosophy එක roughly:

> Debian stability + easier usability + newer packages + commercial ecosystem

---

# Ubuntu ඇයි හදලා තියෙන්නේ?

Debian excellent.

නමුත් beginner / enterprise / cloud user කෙනෙකුට Debian ecosystem එක sometimes complex.

Ubuntu therefore focuses heavily on:

```text
Ease of use
+
Cloud
+
Developer ecosystem
+
Enterprise support
+
Frequent releases
```

---

# Ubuntu versions දෙකක් වැදගත්

## Ubuntu Desktop

```text
GUI
↓
Developer workstation
↓
Desktop applications
```

## Ubuntu Server

```text
CLI
↓
SSH
↓
Services
↓
Cloud
↓
Containers
```

Infrastructure engineer කෙනෙක්ට **Ubuntu Server** තමයි වැදගත්.

---

# Ubuntu LTS

මේක **අනිවාර්යයෙන් ඉගෙන ගන්න concept එකක්.**

LTS = **Long Term Support**

Production environment එකකට company එකක් කියන්නේ:

> "අපි මේ OS එක අද deploy කරලා අවුරුදු ගණනක් maintain කරන්න ඕන."

එතකොට LTS release එක useful.

Conceptually:

```text
Ubuntu release
      │
      ├── Regular
      │
      └── LTS
            │
            └── Long-term maintenance
```

Industry එකේ servers වල **Ubuntu LTS** common reason එක මේක.

---

# Ubuntu use වෙන තැන්


Cloud
 │
 ├── AWS
 ├── Azure
 └── GCP

DevOps
 │
 ├── Docker
 ├── Kubernetes
 ├── Terraform
 └── [[Ansible|Ansible]]

Web
 │
 ├── Nginx
 ├── Apache
 └── HAProxy

Development
 │
 ├── Python
 ├── Node.js
 ├── Java
 └── Go

AI / ML
 │
 ├── CUDA
 ├── PyTorch
 └── TensorFlow

**Cloud + DevOps + Kubernetes path එකකට Ubuntu දැනගැනීම ඉතා useful.**

---

# Debian vs Ubuntu

මේ දෙකේ relationship එක:

```text
                 Debian
                    │
          ┌─────────┴─────────┐
          │                   │
       Stable              Ubuntu
                              │
                       ┌──────┴──────┐
                       │             │
                    Desktop        Server
                                      │
                                   Cloud
```

Simple way එකට:

|Feature|Debian|Ubuntu|
|---|---|---|
|Base|Independent|Debian-based|
|Philosophy|Stability|Usability + ecosystem|
|Packages|Conservative|Generally newer|
|Beginner friendly|Medium|High|
|Cloud|Good|Excellent|
|DevOps|Good|Excellent|
|Enterprise support|Community|Commercial option|
|Package|`.deb`|`.deb`|
|Package manager|APT|APT|

---

# 01.4 RHEL

![Image](https://images.openai.com/static-rsc-4/cJoccTYkzQ0JEOb3Rlw88tEc_zFbnwbZH9gTnNhy-k6MTH1iLbWm5ncn29fBEQSrJtiuEXnIB0LdMQpS1cw3_xOJOUqwZPy5QQP7-v3DUj9MBco3M5YBBUmK7VQWKGKsSrs1_mf-YM0B6dLOaAXJqgaEAOR_Sx3Jv12o6DRLQuLDc2j8PO2LLQu4KmPqiXte?purpose=fullsize)

![Image](https://images.openai.com/static-rsc-4/_R9eyPIu9lrISRrwR3K3jLWb7qbAwwewSFuFBKqpXp1aZVB09SbH7dCITBWtzN6RqgNniI3JKvW7l-jyeyb9memhQIC9JuGI9iiDvSqhPv1VjbUKGWVOZAIHfR1ko0VCj6H43w-ugoyIXLNNEMJYvVwteXZWTAEvJIEKKpBALpAH8bl56M1q-cxIwBaMoaK_?purpose=fullsize)

![Image](https://images.openai.com/static-rsc-4/gWaroa4dnXoZ6Lg23Woiea6Py0esAw9XSXi0oFTILAwTOWlzRg2EY2XNjxaCBL4XooGGUOor3uHIIRz7sF8VHytFsmXX6oqn1G2jtXynO5DqSXLHPCDCjDriofZskNIav6tYI-ZdOJnWWWOOF_wP4TR5NrHFKErzzNQHIYNdfYREmhN0yY8BvSsUmoOv5XNF?purpose=fullsize)

![Image](https://images.openai.com/static-rsc-4/nxjNfTMc6cY3M3nXShTu7tfNdm6oJhmJ5iC2ula1tz2f1eqN8cRyvAqSPvotzpMDwv27Rt_uLJgl6L9wjl1GlHOVLHr3K-wjrZ_xu4c2UxpsXGjE6hC0XOxqib781lyAPtMyIa1DyupY_y9DJxXYAlPImUmASQyrP99F7lfw8mtNXkvFoPNGHBfhIpgi59t7?purpose=fullsize)

![Image](https://images.openai.com/static-rsc-4/olE38M8SDBw6VtlAU_KYuqgGn5pskODGAB-6WLpB9lEcYm_bCBYd1dKgq607bhA2Q4dEzwBbr1RG4XIqL7xPhqN9-oGtA_Y-KkgRQR0gBi2tdAu7-4ofYqgpadyh2KLp_J-k4Oqu5GEj0SS3C5LT2YwW8I3pPPKLE1nIfvZcdLxgrnIOyvSt702IZAkB1-PL?purpose=fullsize)

දැන් අපි **වෙනම world එකකට** එනවා.

RHEL = **Red Hat Enterprise Linux**

මේක develop/distribute කරන්නේ Red Hat.

RHEL philosophy එක:

> **Enterprise stability + security + certification + lifecycle + support**

Ubuntu vs RHEL difference එක තේරුම් ගන්න මේක බලන්න:

```text
Ubuntu ecosystem
      │
      ├── Cloud
      ├── Developer
      └── Open-source ecosystem

RHEL ecosystem
      │
      ├── Enterprise
      ├── Government
      ├── Banking
      ├── Telecom
      ├── Datacenter
      ├── Compliance
      └── Vendor certification
```

---

# RHEL ඇයි companies use කරන්නේ?

Company එකකට server එකක් තියෙනවා කියලා හිතන්න.

```text
Bank
 │
 ├── Application
 ├── Database
 ├── Security
 ├── Compliance
 └── Audit
```

Bank එකට ප්‍රශ්නය:

> "Linux free ද?"

එක විතරක් නෙමෙයි.

ඔවුන්ට අවශ්‍ය:

```text
Who supports this?
       ↓
Security updates?
       ↓
Patch lifecycle?
       ↓
Compliance?
       ↓
Vendor certification?
       ↓
Enterprise support?
       ↓
Predictable lifecycle?
```

මෙතන RHEL strong.

---

# RHEL ecosystem

මේක architecture level එකේ **ගොඩක් වැදගත්**:

```text
                 Red Hat
                    │
                    ▼
                  RHEL
                    │
        ┌───────────┼────────────┐
        ▼           ▼            ▼
      RPM         DNF/YUM      SELinux
        │           │            │
        └───────────┼────────────┘
                    ▼
              Enterprise
               Workloads
```

Package:

```text
.rpm
```

Package manager:

```text
dnf
```

Old environments වල:

```text
yum
```

Security:

```text
SELinux
```

[[SELinux , AppArmor#3. SELinux 🔐|SELinux]] කියන්නේ RHEL ecosystem එකේ **must-know** concept එකක්.

---

# [[SELinux , AppArmor#3. SELinux 🔐|SELinux]] කියන්නේ මොකක්ද?

Linux permissions normally:

```text
User
 │
 └── File permissions
       ├── Read
       ├── Write
       └── Execute
```

[[SELinux , AppArmor#3. SELinux 🔐|SELinux]] එක additional security layer එකක්.

```text
User
 ↓
Linux permissions
 ↓
SELinux policy
 ↓
Application
 ↓
Resource
```

ඒකෙන් application එකට:

> "මේ file එක technically readable වුණත් policy එකෙන් allow කරන්නේ නැත්නම් access කරන්න බෑ."

Enterprise security වල මේක valuable.

---

# RHEL use වෙන industry

Especially:

### Banking

```text
Core Banking
Payment systems
Databases
Middleware
```

### Telecom

```text
Network services
OSS/BSS
5G infrastructure
Databases
```

### Government

```text
Critical systems
Secure infrastructure
Compliance
```

### Enterprise

```text
SAP
Oracle
VMware-related infrastructure
Databases
Middleware
Internal applications
```

### Cloud

RHEL cloud images exist on major cloud providers.

---

# 01.5 Rocky Linux

![Image](https://images.openai.com/static-rsc-4/gMGbcLvV9TXsnuOFvnxCTww5GaLLoOdlWeonB_QIOYUdrYxQHUufjMJzIGazhGusIaeRiN4-7Jq5P7ExGEOR-ZTa6DYIZWd4GGHuJmgvUiKzChwPZ9T4YkGFMFIKXmThwE263l_7Dv4km7BYRW_V3JrX0pLf_taHbggEb7giuDF-VI9uoWAQwNVTxUyxJkgY?purpose=fullsize)

![Image](https://images.openai.com/static-rsc-4/gIeDiERB2bJ0s4t-FI2U2iB1mm-ae-Eu0TrSvYXr0j9AEPI7ZIU4mbKVcDyCpER9x5b5T46r-dxFbXeigJP13hiws8_u0FjRBIrCOWZ30KUV_PBdb04hHJZKXHpKNEUZMgQCkAyj1NPxFPey-1cRKupfmmJQ0xsJoyEzJtNvEnBElMuL8xWvqeWSNoC2Rbw2?purpose=fullsize)

![Image](https://images.openai.com/static-rsc-4/W5qFmI7gqpRM9SmpCniVv3lYp_bfBvtDgpA0OiUFWQEUOP--eNQZn0rS_YvfYhF0SlhRaJ68TWul1xhAwRRU5s0iMJcKJMkcPFjiK2V5zL5uwutUU9_8sWP6C0h0gpIm4iGGFE5nPUvmHeS1UZWVELO0VuRZ6z3TRFHiIBMYz4hwCsC-y8dZ4L6pYNaF-oVK?purpose=fullsize)

![Image](https://images.openai.com/static-rsc-4/dehwa-Tzbpscn9omiBgkqjibjZbz7biBafl99md6GZlAa37sST3rw1dyEazZBs_m4G2eZxC33d1gTBHwEwjLsWkW_9cGA7LObQPU5ta38ArEiF9omRpxsrEXu-rdoWacXJnJVoHV5rELr8Nuiht6y5gl2NNg_90pobYjiFTMn9QZwgJupaW_VDIMyKhkA6yt?purpose=fullsize)

![Image](https://images.openai.com/static-rsc-4/Rq7TQrXjB24LKQ0WQfG2gxhdkgX0k9lbHGYDySkUNTA-fx7sKhgBZjqptr94Flck4Np7vUvUpN7hUQaHpqfnGDu8R3p9KRq01lb-17Q8UlGoEBKlIx31KPaZx6k8g4xcvoZUZfJuG_icUN0Ujy2QoThkbTQxngPrd7dfBygAYCPdp3wS_U9ORXiW-cp4tbtZ?purpose=fullsize)

දැන් interesting part එක.

Rocky Linux කියන්නේ **RHEL-compatible enterprise Linux** එකක්.

Concept එක:

```text
Enterprise Linux ecosystem
          │
          ▼
         RHEL
          │
          │
      Compatible
          │
     ┌────┴────┐
     ▼         ▼
 Rocky       Alma
 Linux       Linux
```

Rocky Linux goal එක:

> RHEL ecosystem එකට compatible, community-driven alternative එකක්.

---

# ඇයි Rocky හදන්න ඕන වුණේ?

CentOS historically RHEL ecosystem එකේ very popular.

Conceptually:

```text
RHEL
 │
 └── CentOS
       │
       └── RHEL-compatible ecosystem
```

CentOS project direction එක වෙනස් වුණා.

ඒකෙන් community එකට අවශ්‍ය වුණා:

> "RHEL-compatible, community-driven enterprise OS එකක්."

ඒ environment එකෙන් **Rocky Linux** සහ **AlmaLinux** වගේ projects important වුණා.

---

# Rocky Linux use කරන්නේ කොහෙද?

```text
Enterprise Lab
      ↓
Production Server
      ↓
Web Server
      ↓
Database
      ↓
Virtualization
      ↓
Containers
      ↓
HPC
```

ඒක especially useful:

- RHEL skills practice
    
- Enterprise Linux labs
    
- Production workloads
    
- Hosting
    
- Servers
    
- Infrastructure
    
- Cost-sensitive environments
    

---

# 01.6 AlmaLinux

![Image](https://images.openai.com/static-rsc-4/uEymqNjuscTQYesm4u_IVntCSbCoP-H8rBqRosY_3T81v178UM4Lfi-5_Cp05bx247RKQtdSwYYmlojLpk1Lg1wBoBVj8KSs73x2IrZUfGov7z9hiZb588ER3CV8NJHwoFx-gJ3peQsv5ASnrDLwOwU6olxgcDM56LoiKSwvGa6X1mKZPvezHq06oBcECCyy?purpose=fullsize)

![Image](https://images.openai.com/static-rsc-4/-kCuhf0KXexB6V7EoLWVBxObLZUM7QtcWqg29KQpa2F75DQHHEQzzYFVoGmDgIbVuWU_MvYcX4Wjt7EtUOh1MI_iyzGGZ5LB43CQM_cdwNQw9pPU5A2rcD1J0C7EnJVC5PKGsiHJpWYgvbP80p-Nlsrsa4qVfgsX_55R70rqRQPSDe9f0By-1EiauC2GXV40?purpose=fullsize)

![Image](https://images.openai.com/static-rsc-4/7TtN_pQE-gpKohqoNzR_QocHWpH-3kcducKgu5REemM1vQ2ZzIZs48Hnkw0-uupUYG8Uwz4VRpMvHMpEHkOGZIP1znj_Wi4rO7KNo3NPL5pABlCKMueBlGFcryrBlaKQ7oVXatDsK5SV6X62fx2qXxhq87XWf8H2kpIc7Kk9MoLeL8odjm0BbT-F5DStCj3k?purpose=fullsize)

![Image](https://images.openai.com/static-rsc-4/bpJxwIqeT_mbvyOODXz0sZcFPwfs2czMn8ocZse9uOj7aMJ-I7mBq7pyW5yWsvroD78Mu3kH4RuvLqG4xMmcegNLw1vBP2vVOum8O8_MZsqUO4L77-6-crIZBovG0xNtZSfEUx01GYN9f3h0rsD47EVtZNzFCgICZW8S6PSCmRdSnpzeWcmySaXZBAWGN93n?purpose=fullsize)

![Image](https://images.openai.com/static-rsc-4/KkehGRsmfHPqN7a5ltbTsAz3--Hq7JHgccbx4L2xkC_v_IrIJFRXMojY6cps_VfFpc_oGE_N8UX6uZ5BLPDl6hGuOWfukBybekp-WxfRTRHbZHgq6l1NnVPaK3lHs85S4YDNMywvYg6TReIX6KB1jvYHv_UBFQ06jr1UI6llvJ0YxiEm2ExEt3G_tdxJDlRL?purpose=fullsize)

AlmaLinux එකත් **RHEL-compatible enterprise Linux** ecosystem එකේ.

So:

```text
RHEL
 │
 ├── Rocky Linux
 │
 └── AlmaLinux
```

නමුත් මේක:

> "Rocky = bad, Alma = good"

වගේ comparison එකක් නෙමෙයි.

දෙකම enterprise Linux ecosystem එකේ useful alternatives.

---

# Rocky vs Alma

දෙකේ basic philosophy එක similar.

```text
                    Enterprise Linux
                           │
             ┌─────────────┼─────────────┐
             │             │             │
            RHEL         Rocky         Alma
             │
        Commercial
         support
```

Difference එක mostly:

- project governance
    
- community
    
- ecosystem integrations
    
- organizational preferences
    
- vendor relationships
    
- release/build processes
    

**Architecture learner කෙනෙක්ට මුලින් දෙකම deep-dive කරන එක priority නෙමෙයි.**

මුලින්:

> **RHEL ecosystem එක understand කරන්න.**

ඊට පස්සේ Rocky/Alma.

---

# 01.7 මේ 4 දේ actually කොහොමද වෙනස්?

මේක තමයි ඔයාගේ question එකේ heart එක.

```text
                 Linux Kernel
                      │
          ┌───────────┴───────────┐
          │                       │
      Debian Family          Red Hat Family
          │                       │
       Debian                    RHEL
          │                       │
       Ubuntu              ┌──────┴──────┐
                           │             │
                         Rocky         Alma
```

---

# Package management difference

මේක **අනිවාර්යයෙන් practical කරලා ඉගෙන ගන්න.**

## Debian / Ubuntu

```text
Package format
      ↓
    .deb

Package database
      ↓
    dpkg

High-level package manager
      ↓
     apt
```

Examples:

```bash
apt update
apt upgrade
apt install nginx
```

---

## RHEL / Rocky / Alma

```text
Package format
      ↓
    .rpm

Package manager
      ↓
     dnf
```

Examples:

```bash
dnf update
dnf install nginx
dnf remove nginx
```

---

# 01.8 Filesystem එක වෙනස්ද?

මෙන්න important point එක:

**Basic Linux filesystem concepts mostly same.**

```text
/
├── boot
├── etc
├── home
├── var
├── usr
├── opt
├── tmp
├── dev
├── proc
└── sys
```

Ubuntu server එකකත් මෙහෙම.

RHEL server එකකත් මෙහෙම.

Rocky එකකත් මෙහෙම.

Alma එකකත් මෙහෙම.

ඒ නිසා distro 4ක් ඉගෙන ගන්නවා කියලා:

> "Filesystem 4ක් වෙන වෙනම ඉගෙන ගන්න ඕන"

නෑ.

**Linux fundamentals එක පාරක් solid කරගන්න.**

---

# 01.9 systemd

මේකත් distro 4ටම common concept එකක්.

Modern Linux:

```text
systemd
   │
   ├── services
   ├── boot
   ├── logging integration
   ├── processes
   └── system state
```

Examples:

```bash
systemctl status nginx
systemctl start nginx
systemctl stop nginx
systemctl restart nginx
systemctl enable nginx
```

මේ commands Ubuntu/RHEL/Rocky/Alma වල conceptually same.

---

# 01.10 Networking

Architecture level එකට යනවා නම් Linux networking **extremely important**.

```text
Linux Network Stack
       │
       ▼
Network Interface
       │
       ▼
IP Address
       │
       ▼
Routing Table
       │
       ▼
ARP / Neighbor
       │
       ▼
TCP / UDP
       │
       ▼
Application
```

Commands:

```bash
ip addr
ip route
ss
ping
traceroute
tcpdump
```

මේවා distro එකකට වඩා **Linux fundamentals**.

---

# 01.11 Security

Linux security layer එක:

```text
User
 │
 ▼
Authentication
 │
 ▼
Authorization
 │
 ├── File permissions
 ├── sudo
 ├── ACL
 └── capabilities
 │
 ▼
MAC
 ├── SELinux
 └── AppArmor
 │
 ▼
Firewall
 ├── nftables
 └── firewalld / ufw
 │
 ▼
Application
```

මෙතන distro difference එක එනවා.

### Ubuntu

Commonly:

```text
AppArmor
ufw
```

### RHEL family

Commonly:

```text
SELinux
firewalld
```

ඒ නිසා:

```text
Ubuntu security mindset
       ≠
RHEL security implementation
```

නමුත් underlying Linux security principles same.

---

# 01.12 Ubuntu vs RHEL — Industry එකේ biggest difference

මේක හොඳට මතක තියාගන්න.

### Ubuntu ecosystem

```text
Cloud
DevOps
Startups
Developers
Containers
Kubernetes
AI/ML
Modern infrastructure
```

### RHEL ecosystem

```text
Enterprise
Banking
Telecom
Government
SAP
Large datacenters
Compliance
Vendor-supported infrastructure
```

**මේක absolute rule එකක් නෙමෙයි.**

Enterprise එකක Ubuntu තියෙන්න පුළුවන්.

Startup එකක RHEL තියෙන්න පුළුවන්.

නමුත් **general industry pattern** එකක් විදිහට මේක useful.

---

# 01.13 ඇයි companies එක OS එකක් තෝරන්නේ?

මේක architecture mindset එකෙන් බලන්න.

Company එක Linux distro තෝරන්නේ:

```text
                OS Selection
                     │
      ┌──────────────┼──────────────┐
      ▼              ▼              ▼
   Technical      Business       Operational
      │              │              │
      ▼              ▼              ▼
 Performance       Cost          Support
 Security          Licensing     Lifecycle
 Compatibility     Risk          Skills
 Applications      Compliance    Automation
```

ඒ කියන්නේ:

> "Ubuntu හොඳද RHEL හොඳද?"

කියන question එක incomplete.

Correct question:

> **"මේ workload එකට, මේ organization එකට, මේ risk model එකට, මේ ecosystem එකට best OS එක මොකක්ද?"**

---

# 01.14 Real-world example

හිතන්න company එකක් තියෙනවා:

```text
                Internet
                    │
                 Firewall
                    │
             Load Balancer
                    │
         ┌──────────┴──────────┐
         │                     │
     Web Server             Web Server
      Ubuntu                 Ubuntu
         │                     │
         └──────────┬──────────┘
                    │
              Application
                    │
                  RHEL
                    │
                Database
                    │
                  RHEL
```

ඇයි මෙහෙම?

Web layer එක:

```text
Ubuntu
↓
Developer ecosystem
↓
Cloud friendly
↓
Fast deployment
```

Enterprise backend:

```text
RHEL
↓
Vendor support
↓
Security
↓
Compliance
↓
Enterprise software
```

**එක organization එකක distro දෙකම තියෙන්න පුළුවන්.**

---

# 01.15 Architecture level එකේ තවත් වැදගත් concept එකක්

Distro එක තෝරලා ඉවර නෑ.

ඊට පස්සේ:

```text
OS
 │
 ├── Compute
 │
 ├── Memory
 │
 ├── Storage
 │
 ├── Networking
 │
 ├── Security
 │
 └── Services
```

ඊට පස්සේ:

```text
Linux
 │
 ├── Virtual Machines
 │
 ├── Containers
 │
 ├── Kubernetes
 │
 ├── Cloud
 │
 └── Bare Metal
```

ඊට පස්සේ:

```text
Infrastructure
      │
      ▼
Platform
      │
      ▼
Applications
      │
      ▼
Users
```

මේ connection එක තේරුම් ගත්තොත් තමයි **Architecture thinking** develop වෙන්නේ.

---

# 01.16 ඔයාට මේ 4 distro ඉගෙන ගන්න priority එක

මම නම් ඔයාට මෙහෙම order එකක් දෙන්නෙ:

```text
                  Linux
                    │
                    ▼
            Linux Fundamentals
                    │
        ┌───────────┴───────────┐
        ▼                       ▼
 Debian Family             RHEL Family
        │                       │
     Ubuntu                    RHEL
        │                       │
        │                ┌──────┴──────┐
        │                ▼             ▼
        │              Rocky         Alma
        │
        ▼
 Cloud / DevOps
```

### Level 1 — Linux fundamentals

මේවා **deep**:

```text
1. Filesystem
2. Users & Groups
3. Permissions
4. Processes
5. systemd
6. Package management
7. Networking
8. SSH
9. Logs
10. Storage
11. Security
12. Bash
13. Cron
14. Services
15. Troubleshooting
```

---

### Level 2 — Ubuntu

Deep:

```text
Ubuntu Server
APT
systemd
Netplan
UFW
AppArmor
cloud-init
SSH
Docker
Kubernetes basics
```

---

### Level 3 — RHEL

Deep:

```text
RHEL
RPM
DNF
systemd
SELinux
firewalld
NetworkManager
nmcli
LVM
SSH
journald
```

---

### Level 4 — Rocky / Alma

මෙතන objective එක:

> **RHEL knowledge එක practically transfer කරන්න.**

ඒ නිසා Rocky/Alma වලට මුලින්ම වෙනම මාස ගණන් දාන්න එපා.

---

# 01.17 Commands එකින් difference එක බලමු

### Install Nginx

Ubuntu:

```bash
sudo apt update
sudo apt install nginx
```

RHEL/Rocky/Alma:

```bash
sudo dnf install nginx
```

---

### Service

දෙකේම:

```bash
sudo systemctl start nginx
sudo systemctl enable nginx
sudo systemctl status nginx
```

**මේක තමයි important lesson එක.**

Package management වෙනස්.

But:

```text
Linux fundamentals
       ↓
Shared
```

---

# 01.18 Architecture engineer කෙනෙක්ට මොනවාද really වැදගත්?

Distro names memorize කරන එකට වඩා මේ matrix එක තේරුම් ගන්න:

|Layer|Learn|
|---|---|
|Kernel|Linux kernel concepts|
|Process|process/thread/signals|
|Memory|RAM/swap/virtual memory|
|Storage|disk/LVM/filesystem|
|Network|IP/routes/TCP/UDP|
|Security|permissions/SELinux/AppArmor|
|Services|systemd|
|Packages|APT vs DNF|
|Logs|journald/log files|
|Automation|Bash/Ansible|
|Virtualization|KVM/VM concepts|
|Containers|Docker/containerd|
|Orchestration|Kubernetes|
|Cloud|AWS/Azure/GCP|
|Observability|logs/metrics/traces|
|Troubleshooting|CPU/RAM/disk/network|

මේවා දන්නවා නම් distro එක මාරු වුණත් ඔයා lost වෙන්නේ නෑ.

---
>>>>>>> a159bce7962d6af920654f254fefef8e27f23cd4
