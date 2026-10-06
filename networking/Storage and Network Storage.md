ඔව්. මේ concepts 4ම **Storage / Network Storage** වල ඉතා වැදගත් topics. විශේෂයෙන් System Engineer කෙනෙක්ට **NAS, SAN, Fibre Channel, iSCSI** අතර වෙනස හොඳට තේරුම් ගන්න ඕන.

මුලින්ම big picture එක බලමු:

```text
                         STORAGE
                            │
              ┌─────────────┴─────────────┐
              │                           │
         File Storage                Block Storage
              │                           │
             NAS                         SAN
              │                           │
       SMB / NFS                    ┌──────┴──────┐
                                    │             │
                              Fibre Channel    iSCSI
```

---

# 1. NAS – Network Attached Storage

![Image](https://images.openai.com/static-rsc-4/cOKEILXO_QS3g46Kl8rLqlH52UqI3hSFJliLccZdJbDvJVcT8auCOdgsjocxZONi4rSEqwZWB3srI3D5FZV-0gtY_sGPzfgEV0aKOvHhN95ENZK_jzlnRtv7ybNDIRwzj-vugMDySeEoB1BwNR3zXTkJBUawbebAE2lownB_kULrrklo5_hXUzVES_JZbb8G?purpose=fullsize)

![Image](https://images.openai.com/static-rsc-4/EWx30MrPjrmEDT-MF-zSKny5GZ_NUXod6QNNdnumRS4XWfz46jayUxrOYSIZkURcwyUhz9L7Ien191fYkkBACGPdyz658baNbpCc9JC3ePCpoE9Wfc99kD0CgbE-AlvHzSo8uNQmZzpPqFf9k9DpWwjBT1qC8194KHnrQ9IQGuhW643IJpPgtDHcW6rmnVlg?purpose=fullsize)

![Image](https://images.openai.com/static-rsc-4/Hd8pAcrLGOcZrMu1qXpQmOW2aNp3TkG7jitFg5M6rP6vyYw2uXK1SHFk1TbWO5j4RK4nefxjLxy-LFTFqblCSLqhAhF8P50M2FGn3SDFo71AXfDoFKPQc5h1rFe_fF4SmjcLON2RV3puiBFGB_LXRphrWWT-V6LZBx6qP2oPx0-WA29QYRL_fq_7FXeXVWh3?purpose=fullsize)

![Image](https://images.openai.com/static-rsc-4/SaZ_LNkJsNYLX-JPXYPy8C45addzwBa8FLfWKBDndosiOzbhjfDegUEiLzjVDC8y_umfo1Y_5F2bNA9y5SN152t1R1ow6wBbFHG1e_Y_AJb_idAlY8oFjzuP4zeWIpy1gFHAoUCY_igzg1Zaiz6NrYIQ6KLbn3V2fi_tdbOlBYZqbr3Tk1Ff8bRJX88MooPA?purpose=fullsize)

![Image](https://images.openai.com/static-rsc-4/3n6UGKCcmQXMMQAr4MA-MXw2l8CkXlefj15YqcdpkUzY84D5WzAl8_kKEAqMu-SeBLHy_umjfRiKOxcYzx53qEcVpCboUSDVqBC3FDfrL8E6rON0t6vaRHDPhj0dcWjAy2n0XeyYZ9jVETMk0H2jaEXEcVoL1jE_KdxDB-xGahHftd8diyY0gr1tmQpr3RiY?purpose=fullsize)

### NAS කියන්නේ මොකක්ද?

**NAS = Network Attached Storage**

Network එකට directly connect කරලා, computers/servers කිහිපයකට **files/folders share කරන storage system එකක්** තමයි NAS.

සරලව:

> **NAS = Network එක හරහා files share කරන storage.**

උදාහරණයක්:

```text
                 LAN
                  │
        ┌─────────┼─────────┐
        │         │         │
      PC-01     PC-02     Server
        │         │         │
        └─────────┼─────────┘
                  │
              ┌───▼───┐
              │  NAS  │
              │       │
              │ HDDs  │
              └───────┘
```

NAS එක ඇතුළේ HDD/SSD කිහිපයක් තියෙන්න පුළුවන්.

උදාහරණයක්:

```text
NAS
│
├── HDD 1 – 4 TB
├── HDD 2 – 4 TB
├── HDD 3 – 4 TB
└── HDD 4 – 4 TB
```

RAID භාවිතා කරලා ඒවා combine කරන්නත් පුළුවන්.

---

# 2. NAS වැඩ කරන විදිහ

NAS එක සාමාන්‍යයෙන් තමන්ගේම operating system එකක්/firmware එකක් තියෙන storage appliance එකක්.

උදාහරණ:

```text
Client
   │
   │ SMB / NFS
   ▼
Network Switch
   │
   ▼
NAS
   │
   ▼
Filesystem
   │
   ▼
HDD / SSD
```

Client එක NAS එකෙන් file එකක් request කරනවා.

උදාහරණයක්:

```text
\\192.168.1.100\Documents
```

Windows එකෙන් මේක network drive එකක් විදිහට map කරන්න පුළුවන්.

Linux වල:

```bash
mount -t nfs 192.168.1.100:/data /mnt/data
```

---

# 3. NAS වල භාවිතා වන protocols

ප්‍රධාන protocols:

### SMB

**SMB = Server Message Block**

ප්‍රධාන වශයෙන් Windows environments වල භාවිතා කරනවා.

```text
Windows Client
      │
      │ SMB
      ▼
     NAS
```

Windows network share එකක්:

```text
\\NAS-SERVER\Shared
```

SMB සාමාන්‍යයෙන් TCP port:

```text
445
```

---

### NFS

**NFS = Network File System**

Linux/Unix environments වල බහුලව භාවිතා කරනවා.

```text
Linux Server
     │
     │ NFS
     ▼
    NAS
```

Example:

```bash
mount -t nfs 192.168.1.50:/storage /mnt/storage
```

---

# 4. NAS වල advantages

### 1. Easy to use

Storage share එකක් create කරලා users ලට access දෙන්න පුළුවන්.

### 2. Centralized storage

Files එක තැනක තියාගන්න පුළුවන්.

```text
PC 1 ─┐
PC 2 ─┤
PC 3 ─┼──> NAS
PC 4 ─┘
```

### 3. Backup

NAS එක backup destination එකක් විදිහට භාවිතා කරන්න පුළුවන්.

### 4. RAID

NAS එකේ RAID configure කරන්න පුළුවන්.

### 5. User permissions

උදාහරණයක්:

```text
Finance → Read/Write
HR      → Read/Write
Guests  → Read only
```

---

# 5. NAS වල disadvantages

NAS network එක හරහා files access කරන නිසා network performance එක බලපානවා.

```text
Client
  │
  ▼
Network
  │
  ▼
NAS
```

Network congestion තිබුණොත් storage performance අඩු වෙන්න පුළුවන්.

ඒ නිසා **high-performance database storage** වගේ workloads වල NAS හැමවිටම best option එක නෙමෙයි.

---

# 6. SAN – Storage Area Network

![Image](https://images.openai.com/static-rsc-4/z6zAb8sNSyvQjbjiJjrA8LlDBIeao1jCFIjn-qqUYG3byALaKY8_I-0X0_PmK5dd5iAVCZxPRoy9XZeNsySwchtOq0Hy8mjrdWpB6nPP8O2SZfYM-I1iOht-FLkQJ3nYP7unNZlFPIr5C-N2GdyHkM484ermO73gvcNkC8KSNn5Z7BCH_9ApTe8vSng3jTlq?purpose=fullsize)

![Image](https://images.openai.com/static-rsc-4/68y8vGJ2IgSp51lTYwm62wopTR59ktCM4oEEnmBoY2ojPn8tf5JDD637DXTDFvI825BLh5hApvgMN6S9aIzRU7D4QEUW4AEhp1VYXZuczTOxSdLMzasFAS16T-4lQG30f8KxQlHhNDMaCwQOPmrXXppHxkZDN3fQ9n_lpMKVaDChWFoIqL4mIvIBd07eNa4b?purpose=fullsize)

![Image](https://images.openai.com/static-rsc-4/pPK1Nzo2hrKUEYWDdYi0w3w2OO-Sd5af-hIwn1bRjq_zvoAk5q8Zw6R2wqBCB01DmVM0TS9gDmFkaEPPjqkcdyw4Ygwd3crohJUJM7keYC_D3LWBSO19DxygRcvWUFYgseZVrIeTux9IC48YX_TNuNzTfwIwicBpOb-a5EERjYkwSZSAO8zVAMvx5tV6TGki?purpose=fullsize)

![Image](https://images.openai.com/static-rsc-4/7UokKUKDlzunrplNnJMqc7quPNLuJjCDfJVUgmRvjBKEvlsVdt1DZ6v7cJuZn5Ho3V55-Zvr5c-s74Itp04kTGEUULP_8EF-1Zh4rqlaWXfM9zhOA-75dHxaOZTGZkYAdETyPr8TIwsctZxsRlFE2Dc_u7i6dsDfImEQ47wBNSgsjWEWRGxiRacZTaLlp1l0?purpose=fullsize)

![Image](https://images.openai.com/static-rsc-4/cGotreDab_VX4EGW2Ln6XdOT61jnxE6dDGTuwbKSix87FkgIFTU1GalD3GbNw7c55KpV5sGRvc7hX0GFDVkbFw0r9qTw2gruEoW1fe_sFB4xygbsotEPLJmrv2NbyHVv3FZHH2VUL10c12x2nMbIzsowVCDXTyO6iFifWckvdfWjM-DXQHK3-5iqnCGgIngw?purpose=fullsize)

![Image](https://images.openai.com/static-rsc-4/EvAog2dWUyrE_wxChFrIrvhv4Z6zkPxIm-d2ejdulOuAVyNjcxMDU3dM0ZGDNMG0DJKyB9J4ntXSJXdBb0C5y_tmKEO_BpEjeGflPx5sO5Aw21E2H1HrWboczKDrWB2qrS_kYUV_Rqmjb4k1G3palIC1ywg9XhA5K9-Vb8U-QC5-XVTzvbynd9GGctN3fPhY?purpose=fullsize)

දැන් NAS වලට වඩා advanced concept එකක්.

**SAN = Storage Area Network**

SAN කියන්නේ servers වලට **block-level storage** ලබාදෙන dedicated storage network එකක්.

මේකේ main difference එක:

```text
NAS → File level

SAN → Block level
```

---

# 7. NAS vs SAN

NAS එකේ:

```text
Server
   │
   │ "Give me this FILE"
   ▼
 NAS
```

SAN එකේ:

```text
Server
   │
   │ "Give me this BLOCK STORAGE"
   ▼
 SAN Storage
```

Server එකට SAN storage එක local disk එකක් වගේ පෙනෙන්න පුළුවන්.

උදාහරණයක්:

```text
Server
│
├── /dev/sda
├── /dev/sdb
└── /dev/sdc
```

මෙතන `/dev/sdb` actually SAN storage එකක LUN එකක් වෙන්න පුළුවන්.

---

# 8. SAN Architecture

සාමාන්‍ය SAN architecture එක:

```text
                ┌───────────────┐
                │     Server 1  │
                └───────┬───────┘
                        │
                        │
                ┌───────▼───────┐
                │  SAN Switch   │
                └───────┬───────┘
                        │
                        │
                ┌───────▼───────┐
                │ Storage Array │
                │               │
                │ SSD / HDD     │
                └───────────────┘
```

Large datacenter එකක:

```text
 Server 1 ─┐
 Server 2 ─┤
 Server 3 ─┼── SAN Fabric ── Storage
 Server 4 ─┘
```

---

# 9. SAN වල LUN කියන්නේ මොකක්ද?

**LUN = Logical Unit Number**

SAN storage array එකේ storage space එකක් server එකකට assign කරනවා.

උදාහරණයක්:

```text
Storage Array
│
├── LUN 01 → 500 GB → Server 01
├── LUN 02 → 1 TB   → Server 02
└── LUN 03 → 2 TB   → Server 03
```

Server එකට:

```text
LUN 01
   ↓
/dev/sdb
```

වගේ පේන්න පුළුවන්.

ඊට පස්සේ server administratorට:

```bash
fdisk
mkfs
mount
```

වගේ normal disk operations කරන්න පුළුවන්.

---

# 10. SAN වල Block Storage

මේක තේරුම් ගැනීම ඉතා වැදගත්.

### File Storage

NAS:

```text
/storage
   │
   ├── file1.txt
   ├── photo.jpg
   └── backup.zip
```

NAS එක filesystem/file structure එක manage කරනවා.

---

### Block Storage

SAN:

```text
LUN
│
├── Block 1
├── Block 2
├── Block 3
├── Block 4
└── ...
```

Server operating system එක filesystem එක create කරගන්නවා.

උදාහරණයක්:

```text
SAN LUN
   ↓
Linux Server
   ↓
ext4 / XFS
   ↓
/data
```

---

# 11. Fibre Channel

![Image](https://images.openai.com/static-rsc-4/1i3gKOtifICKLynFovkz306iGQZowpS2XB8NWoHbn0oHP8I0uSheUN3kJzwOeSd3QURA-7IDVlobB8OwFMAzEqeUedYO6OrrbmUWtF5CXzHF8xNQ0QniA-k1Em_oOCaT505dCqxHFCKOIidoaH_Logvrj5WArmXrA4wFqOcua6P7Y89Pea1ROW0Jm-4CNjpj?purpose=fullsize)

![Image](https://images.openai.com/static-rsc-4/VI2bAPHeYVDBOw1RW0jX1VsEnLP901eSVYyn7Nue_747tH9ZvoUvGeuy3Gk9drBOEzrafHm0Z2Fm4MKAqqQzDBoBvSvxtLPQ3yHLYsWGdPwVL30aaY6ZPEdboraIoUIQOtYgJPrnl2a8focBWSYwmRE5Vm2ql2UPJUnt_sLYtRl3tlZ0Z4TVio321HB-a61s?purpose=fullsize)

![Image](https://images.openai.com/static-rsc-4/dwu-TLAgHbORPmFNvmvpSjhF7zRlINNakykyZKtjcDFj_1CTRXIY6s_3NY7pH-mtAJBZDEdVlgArUPOaZ10zrYujarr72v3B6RMQsAF_MKliZ18Vl0gKk2lLzKILcwo5-lDlw81WXfZaM4Q_q6YrXqXc_xCJNRhc8IwjghbXA9X_fi3g-eju3gQ5cbym1yte?purpose=fullsize)

![Image](https://images.openai.com/static-rsc-4/utAlHELfwKDMkFUIeIhHkcgeGjpZC7n2hMsBL_FpgpxPNyll9_Q6QroCQQ9J3AeIbBQsrzTVmB5B4Y4Np6NzWh0CkxaaDS_8NSpqVxEzXy-29ULAv3KLgDA98yYJEfEhe2jTnzSIVIOX6rGujBGmxmuXVWSKrZ5qf4q4c0QwzBxBhSBOb1yvZz4SF8NUGGJt?purpose=fullsize)

![Image](https://images.openai.com/static-rsc-4/Rhq4v7MBghXuXMchTMRR1x0o6_RNJfa-YnFb0b5xePfvTcXQsqUOsiKO6rb1OD3v3iA0pkLXUW3lAWKiSK0ybuFBL1r-LHLI3qniZZvT1PfgSmxrd29XK7jfyUmKix_y0Pk4aMMkjdDdvFqD2FgY5ovwK944Unrvh-2KA6u_EbTVXR3CjCbO6zfVwpSNFVEo?purpose=fullsize)

![Image](https://images.openai.com/static-rsc-4/hzs90GsgVxS4NQYrLc0q9qMpQAJgxs7hj2Ai8DOj7y7He4CptnHZZpLREjwTiTaaIesrbysXiv8NHMhQGTxcXuN_gZUYGuCBK3-TwQEwDJ7d8JJr-Gd8rojoUr32TsxAU10hG0KbtMWD9--r_T4ysKpwZ0HSJibKp-Qh7ShwU2UEP_OxZC5S4vy1FkcS31I8?purpose=fullsize)

**Fibre Channel (FC)** කියන්නේ high-speed storage networking technology එකක්.

SAN implement කරන්න Fibre Channel භාවිතා කරන්න පුළුවන්.

```text
Server
   │
 FC HBA
   │
   ▼
FC Switch
   │
   ▼
Storage Array
```

---

# 12. Fibre Channel HBA

**HBA = Host Bus Adapter**

Server එක Fibre Channel network එකට connect කරන්න HBA එකක් භාවිතා කරනවා.

Normal Ethernet:

```text
Server
  │
NIC
  │
Ethernet Switch
```

Fibre Channel:

```text
Server
  │
HBA
  │
FC Switch
```

---

# 13. Fibre Channel cables

Fibre Channel වල optical fibre commonly භාවිතා වෙනවා.

```text
Server
  │
HBA
  │
Fiber Cable
  │
  ▼
FC Switch
  │
Fiber Cable
  │
  ▼
Storage
```

High-speed datacenter storage environments වල මේක ඉතා common.

---

# 14. Fibre Channel වල main advantage

FC එකේ main advantage එක:

### High performance + low latency + dedicated storage network

Normal LAN:

```text
Users
Servers
Internet
VoIP
Printers
Storage
      │
      ▼
   Same LAN
```

SAN:

```text
              Production LAN
                   │
             Application
                traffic


              SAN Fabric
                   │
             Storage traffic
```

Storage traffic වෙනම network එකක යන්න පුළුවන්.

---

# 15. Fibre Channel Fabric

SAN එකේ "Fabric" කියන්නේ FC switches සහ connections වලින් හැදුණු storage network එක.

```text
        Server
       /      \
     HBA      HBA
      │        │
      ▼        ▼
  FC Switch A   FC Switch B
      │        │
      └───┬────┘
          │
       Storage
```

Enterprise SAN එකක redundancy සඳහා fabrics දෙකක් භාවිතා කරනවා.

```text
             Server
            /      \
           /        \
       Fabric A    Fabric B
          │            │
          ▼            ▼
       Switch A      Switch B
          │            │
          └─────┬──────┘
                │
             Storage
```

එක path එක fail වුණත් අනෙක් path එකෙන් storage access කරන්න පුළුවන්.

---

# 16. WWN / WWPN

Fibre Channel environment එකේ important identifiers දෙකක්:

### WWN

**World Wide Name**

Device identification සඳහා භාවිතා කරන unique identifier එකක්.

### WWPN

**World Wide Port Name**

Fibre Channel port එක identify කරන unique name එක.

උදාහරණයක්:

```text
Server HBA
   │
   └── WWPN: 10:00:xx:xx:xx:xx
```

SAN administrator කෙනෙක් zoning/mapping configure කරනකොට මේ identifiers වැදගත්.

---

# 17. Fibre Channel Zoning

**Zoning** කියන්නේ SAN fabric එකේ කුමන server එකට කුමන storage resource එක access කරන්න පුළුවන්ද කියලා control කිරීම.

Example:

```text
Server01 HBA
     │
     │ allowed
     ▼
Storage Port 01
```

නමුත්:

```text
Server02
     X
Storage Port 01
```

Security සහ management වලට zoning වැදගත්.

---

# 18. iSCSI

![Image](https://images.openai.com/static-rsc-4/dBNegV34jeYq3bnlGu83FDPHizeEqKWuM_YcAuB7bGYg1mbQ4ivF5T4HgUZNHu24pNvPIr0_M_dWQW5DtNH0OwduewcExEqI15ie9okXxnlifr9SdVtuPPxVJyQhByfUbyIr8ptWCiUyAKQT-vLrYOvSiW6EKNKraRoSuVq8xzQYyz3-u82aw7-IGBUSV2Nx?purpose=fullsize)

![Image](https://images.openai.com/static-rsc-4/WiyikDN2bebQxxiWQ9bQ2-4LNhO9u_dDc8NJvh9i23PQd0FtdcmSjU_ORj-R8QeGerigrVpd_Z5vUIE0LKBBnpTSouum800wEeeqfc60wZFEMgKxFTDZqd54lo49Qw3WXIh2tlKgGAxMpMbWmwoEwOL8iKOjHNTFPKm-chu_myGpnh2ShOKYejKRUs14HTsV?purpose=fullsize)

![Image](https://images.openai.com/static-rsc-4/y4ipAj5B3q8cmKiNvepnLBR08K9httjM8RhH6WpLiY46sXe4MUNTzoHtKRyost2l7oggq5qhAA4GxCcoac79_Uer1Wvh7VDAkYxOrX4uCM0Of_hlNH8-7jNxLseepQijIpvC7c9A0OPpTrzjToHfK5jBKtB6j2gMplr4zKuUiNTXnxUwFsvoNsGvUQeYVFMT?purpose=fullsize)

![Image](https://images.openai.com/static-rsc-4/_STpqugsmvr4t1DPdLzHDd8ViydNMx_KWFb--sgEr_gljdO2YFntX16RiaWNNKwCOio2UBlrJAv7yi8jzCzW-zqVQs0mok_nDjcSfWmcl6v62eVtnQyM2MkQ1fd4EluKO3Wa1-5gMwjvWZzkyHmlxrujhOptzoycaGq0_7k_NcN4_E010WYlMVjh1DIkJF72?purpose=fullsize)

![Image](https://images.openai.com/static-rsc-4/K9s6fkYkiHmlT7FwlXyPYlxA8kiwFfw00qWmiYUUz004Ow37mNp7oJvnAmYAXAux5IB6uEFwUpZ6mysrbsQOUdbRirWnGYIZDbXJfsq_zHj70bSr2JqhLmha12xPYLY5NM4tC2k74qcm5g8sjC-Yj2iHwXMLhXab12sLmY6T3nf2aqR-eHTvUf8ja9TUqamm?purpose=fullsize)

**iSCSI = Internet Small Computer Systems Interface**

iSCSI කියන්නේ **IP network එක හරහා SCSI block storage transport කරන technology එකක්.**

සරලව:

> **iSCSI = Ethernet/IP network එක භාවිතා කරලා SAN-like block storage ලබාදීම.**

---

# 19. iSCSI Architecture

```text
Linux Server
     │
    NIC
     │
 Ethernet
     │
     ▼
Network Switch
     │
     ▼
iSCSI Storage
     │
     ▼
    LUN
```

මෙතන:

### Initiator

Storage access කරන client/server එක.

```text
Server = iSCSI Initiator
```

### Target

Storage provide කරන system එක.

```text
Storage = iSCSI Target
```

---

# 20. iSCSI example

ඔයාට server එකක 2 TB storage එකක් තියෙනවා කියමු.

iSCSI target එකක් configure කරනවා:

```text
Storage
   │
   └── iSCSI Target
           │
           └── LUN 2 TB
```

Server එක:

```text
Linux Server
     │
iSCSI Initiator
     │
     ▼
iSCSI Target
     │
     ▼
2 TB LUN
```

Linux server එකට:

```bash
lsblk
```

කලාම:

```text
sda   100G
sdb     2T
```

වගේ පේන්න පුළුවන්.

---

# 21. iSCSI Port

iSCSI සාමාන්‍යයෙන්:

```text
TCP 3260
```

භාවිතා කරනවා.

Firewall එකේ ඒක allow කරන්න අවශ්‍ය වෙන්න පුළුවන්.

```bash
firewall-cmd --add-port=3260/tcp --permanent
firewall-cmd --reload
```

---

# 22. iSCSI Login

Linux server එක iSCSI target එක discover කරනවා.

Conceptually:

```text
Initiator
    │
    │ Discovery
    ▼
Target
    │
    ▼
LUN
```

Linux වල commonly `iscsiadm` command එක භාවිතා කරනවා.

Example:

```bash
iscsiadm -m discovery
```

Target එක discover කරලා login කරන්න පුළුවන්.

```bash
iscsiadm -m node --login
```

ඊට පස්සේ:

```bash
lsblk
```

කරලා newly attached block device එක බලන්න පුළුවන්.

---

# 23. iSCSI vs Fibre Channel

මේ දෙකම SAN technologies.

|Feature|Fibre Channel|iSCSI|
|---|---|---|
|Network|FC|Ethernet/IP|
|Storage|Block|Block|
|Main hardware|FC HBA|Ethernet NIC|
|Switch|FC Switch|Ethernet Switch|
|Protocol|Fibre Channel|TCP/IP|
|Default port|N/A|TCP 3260|
|Cost|සාමාන්‍යයෙන් වැඩි|සාමාන්‍යයෙන් අඩු|
|Setup|Complex|Relatively easier|
|Performance|Very high|High|
|Existing LAN|Separate fabric|Can use Ethernet|
|Enterprise SAN|Very common|Very common|

---

# 24. NAS vs SAN

මේක interview වල අනිවාර්යයෙන් අහන්න පුළුවන්.

|Feature|NAS|SAN|
|---|---|---|
|Storage type|File|Block|
|Access|File-level|Block-level|
|Protocol|SMB/NFS|FC/iSCSI|
|Server sees|Network share|Disk/LUN|
|Network|Normal LAN|Dedicated/isolated storage network|
|Setup|Easier|More complex|
|Cost|Lower|Higher|
|Use case|File sharing|Databases/VMs/Enterprise|
|Management|Easier|Advanced|
|Performance|Good|Usually higher|

---

# 25. Real-world example

Suppose company එකකට මේ infrastructure එක තියෙනවා:

```text
                 COMPANY NETWORK
                       │
          ┌────────────┼────────────┐
          │            │            │
       Users        Servers       Printers
                       │
                       │
                    NAS
                       │
                File Storage
```

NAS එකේ:

```text
/company
   │
   ├── HR
   ├── Finance
   ├── Projects
   └── Backups
```

මේක file sharing වලට perfect.

---

Enterprise virtualization environment එකක්:

```text
              VM Server 1
                   │
              VM Server 2
                   │
              VM Server 3
                   │
              VM Server 4
                   │
             ┌─────┴─────┐
             │ SAN Fabric │
             └─────┬─────┘
                   │
            ┌──────▼──────┐
            │ SAN Storage │
            │             │
            │ SSD / HDD   │
            └─────────────┘
```

SAN එකෙන් VMware/Proxmox servers වලට datastores/LUNs provide කරන්න පුළුවන්.

---

# 26. Proxmox + SAN example

ඔයා Proxmox environment එකක් manage කරනවා කියලා හිතමු.

```text
             Proxmox Cluster
       ┌────────┼────────┐
       │        │        │
    Node 1   Node 2   Node 3
       │        │        │
       └────────┼────────┘
                │
             Storage
                │
       ┌────────┴────────┐
       │                 │
      NFS              iSCSI
       │                 │
      NAS            SAN Storage
```

NFS:

```text
Proxmox
   │
   │ File-level
   ▼
NAS
```

iSCSI:

```text
Proxmox
   │
   │ Block-level
   ▼
iSCSI Target
   │
   ▼
LUN
```

---

# 27. NAS වලදී filesystem එක කොහෙද?

NAS:

```text
                 NAS
                  │
             Filesystem
                  │
             /storage
                  │
        ┌─────────┼─────────┐
        │         │         │
      file1     file2     file3
```

NAS itself filesystem manage කරනවා.

Client:

```text
Client
   │
   │ "Give me file1"
   ▼
 NAS
```

---

# 28. SAN වලදී filesystem එක කොහෙද?

SAN:

```text
             SAN Storage
                  │
                 LUN
                  │
                  ▼
              Server
                  │
             Filesystem
                  │
             /data
```

Server එක filesystem manage කරනවා.

උදාහරණයක්:

```text
SAN LUN
   ↓
/dev/sdb
   ↓
XFS
   ↓
/data
```

මේ difference එක **ඉතා වැදගත්**.

---

# 29. එකම LUN එක servers දෙකකට දුන්නොත්?

සාමාන්‍ය filesystem එකක් නම් මේක dangerous.

```text
             LUN
              │
       ┌──────┴──────┐
       │             │
    Server A      Server B
       │             │
     mount          mount
```

Servers දෙකම එකම filesystem එක independently modify කළොත් filesystem corruption වෙන්න පුළුවන්.

Cluster filesystem / application-level coordination අවශ්‍ය විය හැක.

ඒ නිසා SAN එකේ LUN presentation සහ access control carefully configure කරනවා.

---

# 30. Multipathing

Enterprise SAN වල තවත් වැදගත් concept එකක්.

Single path:

```text
Server
   │
   ▼
Switch
   │
   ▼
Storage
```

Switch/path fail වුණොත්:

```text
Server  X──── Storage
```

Multipath:

```text
             ┌── Switch A ──┐
Server ──────┤              ├── Storage
             └── Switch B ──┘
```

Path A fail:

```text
Server
   │
   └──────── Switch B ───── Storage
```

Storage access continue කරන්න පුළුවන්.

Linux වල `device-mapper-multipath` වගේ technologies භාවිතා කරනවා.

---

# 31. RAID සහ මේ concepts අතර relationship

මේවා confuse කරන්න එපා.

### RAID

Physical disks protect/aggregate කරන technology.

```text
HDD1
HDD2
HDD3
HDD4
 │
 ▼
RAID
 │
 ▼
Storage Pool
```

### NAS

Storage pool එක files ලෙස provide කරනවා.

```text
RAID
 ↓
NAS
 ↓
SMB/NFS
 ↓
Client
```

### SAN

Storage pool එක blocks/LUNs ලෙස provide කරනවා.

```text
RAID
 ↓
SAN
 ↓
LUN
 ↓
Server
```

---

# 32. Simple complete picture

```text
                         STORAGE
                            │
            ┌───────────────┴──────────────┐
            │                              │
        FILE STORAGE                  BLOCK STORAGE
            │                              │
            ▼                              ▼
           NAS                             SAN
            │                       ┌──────┴──────┐
       ┌────┴────┐                  │             │
       │         │                  │             │
      SMB       NFS                 FC           iSCSI
       │         │                  │             │
       │         │                  │             │
     Ethernet   Ethernet          FC Fabric    Ethernet/IP
       │         │                  │             │
       └────┬────┘                  └──────┬──────┘
            │                              │
          Files                          LUNs
                                            │
                                            ▼
                                         Server
                                            │
                                       Filesystem
                                            │
                                          /data
```

---

# 33. මතක තියාගන්න easiest method එක

මේ sentence 4 මතක තියාගන්න:

### NAS

> **NAS gives me files over the network.**

```text
NAS → FILE
```

### SAN

> **SAN gives me blocks/storage over a storage network.**

```text
SAN → BLOCK
```

### Fibre Channel

> **Fibre Channel is a high-performance networking technology commonly used to build SANs.**

```text
FC → SAN
```

### iSCSI

> **iSCSI carries SCSI block storage over TCP/IP/Ethernet.**

```text
iSCSI → SAN over Ethernet/IP
```

---

# 34. Interview එකේ අහන විදිහට

### Q: NAS සහ SAN අතර වෙනස?

**Answer:**

> NAS provides file-level storage over a network using protocols such as SMB and NFS, while SAN provides block-level storage using technologies such as Fibre Channel and iSCSI. A NAS share appears as a network filesystem, whereas SAN storage can appear to a server as a raw disk/LUN.

### Q: Fibre Channel කියන්නේ?

> Fibre Channel is a high-speed networking technology designed primarily for storage networking and is commonly used in enterprise SAN environments.

### Q: iSCSI කියන්නේ?

> iSCSI is a protocol that transports SCSI commands over TCP/IP networks, allowing servers to access remote block storage through Ethernet.

### Q: iSCSI Initiator සහ Target?

```text
Initiator → requests/accesses storage
Target    → provides storage
```

### Q: LUN කියන්නේ?

> LUN is a Logical Unit Number, representing a logical block-storage unit presented from a storage system to a server.

---

## ⭐ Final mental model

```text
                    STORAGE
                       │
          ┌────────────┴────────────┐
          │                         │
       FILE LEVEL                BLOCK LEVEL
          │                         │
         NAS                       SAN
          │                    ┌────┴────┐
       SMB / NFS               │         │
                             Fibre      iSCSI
                            Channel      │
                               │       TCP/IP
                               │         │
                               └────┬────┘
                                    │
                                   LUN
                                    │
                                  Server
                                    │
                                Filesystem
                                    │
                                  /data
```

**එක line එකකින්:**  
**NAS = files**, **SAN = blocks**, **Fibre Channel = dedicated high-performance storage networking**, **iSCSI = block storage over TCP/IP/Ethernet**.