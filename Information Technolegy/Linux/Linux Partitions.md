ඔව්. **Linux Partitions** කියන්නේ Linux system administration වල ඉතා වැදගත් topic එකක්. Server manage කරනකොට disk partitioning හරියට තේරුම් ගැනීම අත්‍යවශ්‍යයි.

මම මේක **basic → intermediate → advanced** විදිහට, practical commands එක්ක explain කරන්නම්.

---

# 1. Partition කියන්නේ මොකක්ද?

Physical disk එකක් තිබුණා කියලා හිතන්න:

```text
/dev/sda
```

ඒ disk එක සම්පූර්ණයෙන්ම එකම කොටසක් විදිහට භාවිතා නොකර, කොටස් කිහිපයකට බෙදන්න පුළුවන්.

උදාහරණයක්:

```text
/dev/sda
│
├── /dev/sda1    → 1 GB
├── /dev/sda2    → 50 GB
└── /dev/sda3    → 100 GB
```

මෙවැනි disk කොටස් වලට **partitions** කියනවා.

උදාහරණයක්:

```text
Physical Disk
     150 GB
       │
       ├───────────────┐
       │               │
   Partition 1     Partition 2
      50 GB           100 GB
```

Linux වල partition එකක් සාමාන්‍යයෙන් filesystem එකක් සඳහා භාවිතා කරනවා.

---

# 2. Disk එක සහ Partition එක අතර වෙනස

මේ දෙක confuse කරගන්න එපා.

### Disk

Physical storage device එක:

```text
/dev/sda
/dev/sdb
/dev/nvme0n1
```

### Partition

Disk එකේ logical කොටස:

```text
/dev/sda1
/dev/sda2
/dev/sda3
```

උදාහරණයක්:

```text
/dev/sda
│
├── /dev/sda1
├── /dev/sda2
└── /dev/sda3
```

මෙහි:

```text
sda  = disk
sda1 = partition
sda2 = partition
sda3 = partition
```

---

# 3. Linux Disk Naming

Linux එකේ disk naming scheme එක තේරුම් ගැනීම වැදගත්.

## SATA / SCSI disks

```text
/dev/sda
/dev/sdb
/dev/sdc
```

පළවෙනි disk:

```text
/dev/sda
```

දෙවෙනි disk:

```text
/dev/sdb
```

තුන්වෙනි:

```text
/dev/sdc
```

Partition:

```text
/dev/sda1
/dev/sda2
/dev/sda3
```

---

# 4. NVMe Disk

Modern servers සහ laptops වල NVMe තියෙන්න පුළුවන්.

උදාහරණ:

```text
/dev/nvme0n1
```

Partitions:

```text
/dev/nvme0n1p1
/dev/nvme0n1p2
/dev/nvme0n1p3
```

මෙතන `p` එක වැදගත්.

```text
/dev/nvme0n1
           │
           └── disk

/dev/nvme0n1p1
             │
             └── partition
```

---

# 5. Partition Table කියන්නේ මොකක්ද?

Disk එකේ partitions කොහෙද තියෙන්නේ, ඒවායේ size එක කීයද, type එක මොකක්ද වගේ information තබාගන්න structure එකක් තියෙනවා.

ප්‍රධාන partition table types දෙක:

```text
MBR
GPT
```

---

# 6. MBR

**MBR = Master Boot Record**

පරණ partitioning system එක.

Main limitations:

- Maximum disk size ≈ 2 TB
    
- Maximum 4 primary partitions
    
- Legacy BIOS systems සමඟ බහුලව භාවිතා වුණා
    

උදාහරණයක්:

```text
/dev/sda
│
├── Primary 1
├── Primary 2
├── Primary 3
└── Primary 4
```

---

# 7. GPT

**GPT = GUID Partition Table**

Modern systems වල standard එක.

Advantages:

- 2 TB limitation එක practically නැහැ
    
- Partitions ගොඩක් support කරනවා
    
- Modern UEFI systems සඳහා ideal
    
- Partition table එකේ backup copy එකක් තබාගන්නවා
    
- Modern servers වල recommended
    

සාමාන්‍යයෙන් 2026 වෙනකොට අලුත් server එකක් setup කරනවා නම් **GPT + UEFI** තමයි සාමාන්‍ය choice එක.

---

# 8. MBR vs GPT

|Feature|MBR|GPT|
|---|---|---|
|Age|Old|Modern|
|Max practical disk size|~2 TB|Very large|
|Primary partitions|4|Many|
|Boot firmware|BIOS|UEFI|
|Backup partition table|No|Yes|
|Modern server|Not preferred|Recommended|

---

# 9. Linux Installation එකේ Common Partitions

Linux install කරනකොට system එකේ architecture එක අනුව partitions කිහිපයක් දකින්න පුළුවන්.

Typical UEFI Linux installation:

```text
/dev/nvme0n1
│
├── EFI System Partition
├── /
└── swap
```

උදාහරණයක්:

```text
/dev/nvme0n1p1    512 MB    /boot/efi
/dev/nvme0n1p2     50 GB    /
/dev/nvme0n1p3      8 GB    swap
```

---

# 10. `/` Root Partition

Linux filesystem එකේ main root directory එක:

```text
/
```

මේක Windows වල:

```text
C:\
```

වගේ concept එකක්.

Root filesystem එක ඇතුළේ:

```text
/
├── bin
├── boot
├── dev
├── etc
├── home
├── opt
├── root
├── tmp
├── usr
└── var
```

උදාහරණ:

```text
/dev/sda2 → /
```

---

# 11. `/home` Partition

User data සඳහා වෙනම partition එකක් දාන්න පුළුවන්.

```text
/dev/sda3 → /home
```

එතකොට:

```text
/
├── etc
├── usr
├── var
└── home
      ├── user1
      └── user2
```

Advantage එක:

OS එක reinstall කළත් `/home` partition එක වෙනම තියාගන්න පුළුවන්.

---

# 12. `/boot`

Boot-related files සඳහා:

```text
/boot
```

උදාහරණ:

```text
/dev/sda2 → /boot
```

මෙහි Linux kernel සහ bootloader-related files තියෙන්න පුළුවන්.

---

# 13. `/boot/efi`

UEFI systems වල:

```text
/boot/efi
```

මෙය **EFI System Partition (ESP)** එක mount කරන location එක.

සාමාන්‍ය size:

```text
260 MB
512 MB
1 GB
```

වගේ.

Filesystem එක සාමාන්‍යයෙන්:

```text
FAT32
```

---

# 14. Swap Partition

Swap කියන්නේ RAM එකට අමතර virtual memory space එකක්.

උදාහරණයක්:

```text
RAM = 8 GB
Swap = 4 GB
```

Swap partition:

```text
/dev/sda3
```

හෝ swap file එකක් වශයෙන්:

```text
/swapfile
```

ද භාවිතා කරන්න පුළුවන්.

---

# 15. Swap Partition vs Swap File

### Swap partition

```text
/dev/sda3
```

### Swap file

```text
/swapfile
```

Modern Linux systems වල swap file එක practical සහ flexible.

උදාහරණ:

```bash
swapon --show
```

Output:

```text
NAME      TYPE SIZE USED PRIO
/swapfile file   4G   0B   -2
```

---

# 16. `/var` වෙනම Partition කිරීම

Server administration වල මේක වැදගත්.

`/var` තුළ:

```text
/var/log
/var/lib
/var/cache
```

වගේ data තියෙනවා.

විශේෂයෙන්:

```text
/var/log
```

logs store කරනවා.

Server එකක logs වැඩි වුණොත්:

```text
/var
```

filesystem එක full වෙන්න පුළුවන්.

ඒ නිසා සමහර server architectures වල:

```text
/dev/sda2 → /
/dev/sda3 → /var
```

වගේ වෙනම partition කරනවා.

---

# 17. `/var/log` වෙනම Partition කිරීම

Logging-heavy server එකක:

```text
/
│
├── etc
├── usr
├── opt
└── var
      └── log
```

`/var/log` වෙනම partition එකකට mount කළොත්:

```text
/dev/sdb1 → /var/log
```

logs වැඩි වුණත් root filesystem එක protect කරන්න පුළුවන්.

---

# 18. `/tmp` වෙනම Partition

`/tmp` temporary files සඳහා.

```text
/dev/sdb2 → /tmp
```

Security සහ resource management reasons නිසා enterprise systems වල වෙනම `/tmp` filesystem එකක් භාවිතා කරන අවස්ථා තියෙනවා.

---

# 19. `/opt`

Third-party applications සඳහා:

```text
/opt
```

උදාහරණ:

```text
/opt/application
/opt/software
```

විශාල application deployments තියෙන server එකක `/opt` වෙනම filesystem එකක් කරන්න පුළුවන්.

---

# 20. Filesystem එක කියන්නේ මොකක්ද?

Partition එක සහ filesystem එක එකම දෙයක් නෙමෙයි.

මේක ඉතා වැදගත්.

```text
Disk
 ↓
Partition
 ↓
Filesystem
 ↓
Mount point
```

උදාහරණ:

```text
/dev/sdb1
    ↓
    ext4
    ↓
    /data
```

මෙතන:

```text
/dev/sdb1 = partition
ext4      = filesystem
/data     = mount point
```

---

# 21. Common Linux Filesystems

Linux වල common filesystems:

```text
ext4
XFS
Btrfs
ext3
ext2
```

Enterprise Linux distributions වල:

```text
XFS
```

ගොඩක් common.

Ubuntu/Debian systems වල:

```text
ext4
```

ගොඩක් common.

---

# 22. Partition + Filesystem Example

ඔයාට 500 GB disk එකක් තියෙනවා කියලා හිතන්න.

```text
/dev/sdb
```

Partition කරනවා:

```text
/dev/sdb1 = 200 GB
/dev/sdb2 = 300 GB
```

Filesystem දානවා:

```text
/dev/sdb1 → XFS
/dev/sdb2 → ext4
```

Mount කරනවා:

```text
/dev/sdb1 → /data
/dev/sdb2 → /backup
```

Final structure:

```text
/dev/sdb
│
├── sdb1 → XFS  → /data
│
└── sdb2 → ext4 → /backup
```

---

# 23. Disk එක බලන්නේ කොහොමද?

## `lsblk`

පළවෙනි command එකක් විදිහට මතක තියාගන්න:

```bash
lsblk
```

උදාහරණ:

```text
NAME        SIZE TYPE MOUNTPOINT
sda         100G disk
├─sda1        1G part /boot
├─sda2       70G part /
└─sda3       29G part /home
sdb         500G disk
└─sdb1      500G part /data
```

---

# 24. `lsblk -f`

Filesystem details බලන්න:

```bash
lsblk -f
```

උදාහරණ:

```text
NAME   FSTYPE FSVER LABEL UUID                                 MOUNTPOINT
sda
├─sda1 xfs          boot  xxxx                                 /boot
├─sda2 xfs          root  xxxx                                 /
└─sda3 xfs          home  xxxx                                 /home
```

මේ command එක **ඉතා වැදගත්**.

---

# 25. `df -h`

Mounted filesystem usage බලන්න:

```bash
df -h
```

උදාහරණ:

```text
Filesystem      Size  Used Avail Use% Mounted on
/dev/sda2        50G   20G   30G  40% /
/dev/sda1       1.0G  200M  824M  20% /boot
/dev/sda3       100G   50G   50G  50% /home
```

මෙහි:

```text
Size = total
Used = used
Avail = available
Use% = percentage used
```

---

# 26. `du`

Directory එකක් කොච්චර space use කරනවාද බලන්න:

```bash
du -sh /var
```

Specific directory:

```bash
du -sh /var/log
```

Top directories:

```bash
du -h --max-depth=1 /var
```

---

# 27. `fdisk`

Partition table inspect කරන්න:

```bash
sudo fdisk -l
```

උදාහරණ:

```text
Disk /dev/sda: 100 GiB
Device       Start      End  Sectors  Size Type
/dev/sda1     2048  1050623  1048576  512M Linux filesystem
/dev/sda2  1050624  ...       ...      80G Linux filesystem
```

---

# 28. `parted`

Modern partition management සඳහා:

```bash
sudo parted -l
```

GPT information බලන්නත් useful.

---

# 29. Partition එකක් create කරන Basic Process

Suppose new disk එක:

```text
/dev/sdb
```

මුලින් check:

```bash
lsblk
```

ඊට පස්සේ partition:

```bash
sudo fdisk /dev/sdb
```

Inside `fdisk`:

```text
n
```

new partition.

ඊට පස්සේ size specify කරලා:

```text
w
```

write changes.

**ඉතා වැදගත්:** `fdisk` වල wrong disk එක select කළොත් existing data නැතිවෙන්න පුළුවන්.

---

# 30. Partition එකට Filesystem එකක් දාන එක

Partition එක:

```text
/dev/sdb1
```

XFS filesystem:

```bash
sudo mkfs.xfs /dev/sdb1
```

ext4:

```bash
sudo mkfs.ext4 /dev/sdb1
```

⚠️ `mkfs` existing partition එකේ data destroy කළ හැකියි. Production disk එකක execute කිරීමට කලින් device එක verify කරන්න.

---

# 31. Mount Point එක හදන එක

```bash
sudo mkdir /data
```

Mount:

```bash
sudo mount /dev/sdb1 /data
```

Check:

```bash
df -h
```

හෝ:

```bash
lsblk -f
```

---

# 32. Mount කියන්නේ මොකක්ද?

Filesystem එක Linux directory tree එකට attach කිරීම:

```text
/dev/sdb1
     │
     │ mount
     ↓
   /data
```

ඒකෙන්:

```bash
cd /data
```

කරලා disk එකේ data access කරන්න පුළුවන්.

---

# 33. `/etc/fstab`

Temporary mount එකක්:

```bash
mount /dev/sdb1 /data
```

Server reboot කළොත් mount එක නැතිවෙන්න පුළුවන්.

Permanent mount එකකට:

```text
/etc/fstab
```

භාවිතා කරනවා.

---

# 34. `/etc/fstab` Example

```text
UUID=xxxx-xxxx  /data  xfs  defaults  0 0
```

Structure:

```text
UUID
 ↓
mount point
 ↓
filesystem
 ↓
options
 ↓
dump
 ↓
fsck
```

උදාහරණය:

```text
UUID=abc123  /data  xfs  defaults  0 0
```

---

# 35. UUID කියන්නේ ඇයි?

Device name:

```text
/dev/sdb1
```

හැමවිටම stable නොවෙන්න පුළුවන්.

System එකේ disk ordering වෙනස් වුණොත්:

```text
/dev/sdb
```

වෙනත් disk එකක් වෙන්න පුළුවන්.

UUID එක unique.

UUID බලන්න:

```bash
blkid
```

උදාහරණ:

```text
/dev/sdb1: UUID="abc123" TYPE="xfs"
```

ඒ නිසා `/etc/fstab` වල UUID භාවිතා කිරීම best practice.

---

# 36. LVM

Linux partitioning ගැන deeply යනකොට **LVM** අනිවාර්යයෙන්ම දැනගන්න ඕන.

**LVM = Logical Volume Manager**

Traditional:

```text
Disk
 ↓
Partition
 ↓
Filesystem
```

LVM:

```text
Disk
 ↓
Physical Volume
 ↓
Volume Group
 ↓
Logical Volume
 ↓
Filesystem
 ↓
Mount point
```

---

# 37. LVM Components

### PV

Physical Volume:

```text
/dev/sdb1
```

### VG

Volume Group:

```text
vg_data
```

### LV

Logical Volume:

```text
lv_database
```

Example:

```text
/dev/sdb1
    ↓
   PV
    ↓
 vg_data
    ↓
 lv_database
    ↓
  XFS
    ↓
 /database
```

---

# 38. LVM එකේ ලොකු advantage එක

Suppose:

```text
/data = 100 GB
```

පස්සේ data වැඩිවෙලා:

```text
100 GB → 150 GB
```

Traditional partition එකක් නම් resize process එක depending on filesystem/layout can be more complicated.

LVM තියෙනවා නම්:

```text
lvextend
```

කරලා Logical Volume එක expand කරන්න පුළුවන්.

---

# 39. LVM Example

Physical disk:

```text
/dev/sdb = 500 GB
```

Partition:

```text
/dev/sdb1 = 500 GB
```

PV:

```bash
pvcreate /dev/sdb1
```

VG:

```bash
vgcreate vg_data /dev/sdb1
```

LV:

```bash
lvcreate -L 200G -n lv_data vg_data
```

Filesystem:

```bash
mkfs.xfs /dev/vg_data/lv_data
```

Mount:

```bash
mkdir /data
mount /dev/vg_data/lv_data /data
```

Final:

```text
/dev/sdb
   │
   └── sdb1
        │
        └── PV
             │
             └── VG: vg_data
                    │
                    └── LV: lv_data
                           │
                           └── XFS
                                │
                                └── /data
```

---

# 40. LVM Commands

PV:

```bash
pvs
pvdisplay
```

VG:

```bash
vgs
vgdisplay
```

LV:

```bash
lvs
lvdisplay
```

මේ commands 6ක් මතක තියාගන්න:

```text
pvs
pvdisplay

vgs
vgdisplay

lvs
lvdisplay
```

---

# 41. Partition vs LVM

### Traditional

```text
/dev/sdb1
   ↓
 XFS
   ↓
 /data
```

### LVM

```text
/dev/sdb1
   ↓
 PV
   ↓
 VG
   ↓
 LV
   ↓
 XFS
   ↓
 /data
```

Server environments වල LVM ගොඩක් useful.

---

# 42. RAID සහ Partition අතර වෙනස

මේ දෙකත් confuse වෙනවා.

### Partition

Disk එක logical කොටස් වලට බෙදනවා.

```text
Disk
├── Partition 1
├── Partition 2
└── Partition 3
```

### RAID

Multiple disks combine කරලා redundancy/performance ලබාදෙන technology එකක්.

```text
Disk 1 ─┐
Disk 2 ─┼── RAID
Disk 3 ─┘
```

RAID සහ partitioning එකට use කරන්න පුළුවන්.

---

# 43. Practical Server Architecture

Production server එකක් example එකක්:

```text
2 TB SSD
     │
     ↓
   GPT
     │
     ├── EFI       1 GB
     ├── /boot     2 GB
     │
     └── LVM
          │
          └── VG: vg0
                │
                ├── LV root      50 GB
                ├── LV var       50 GB
                ├── LV log       100 GB
                ├── LV docker    500 GB
                └── LV data      remaining
```

මෙවැනි architecture එක server එකේ workload එක අනුව design කරන්න පුළුවන්.

---

# 44. Docker Server එකක Partitions

ඔයා Docker / Jenkins / Graylog වගේ systems manage කරන නිසා මේක විශේෂයෙන් වැදගත්.

Docker data:

```text
/var/lib/docker
```

Docker images, containers, layers, volumes නිසා disk usage ඉක්මනින් වැඩි වෙන්න පුළුවන්.

ඒ නිසා architecture එක:

```text
/
├── OS
├── etc
├── usr
└── var

/var/lib/docker
        ↓
separate filesystem
```

වගේ කරන්න පුළුවන්.

උදාහරණ:

```text
/dev/mapper/vg0-docker → /var/lib/docker
```

එතකොට Docker storage root filesystem එක පුරවන්නේ නැතිව manage කරන්න පහසුයි.

---

# 45. Graylog Server Example

Logging server එකක:

```text
OS
│
├── /
│
├── /var/log
│
└── Data disk
       │
       └── Elasticsearch/OpenSearch data
```

Graylog/OpenSearch data ගොඩක් වැඩි නම් dedicated storage එකක් use කිරීම හොඳ architecture එකක්.

උදාහරණ:

```text
/dev/sda → OS
/dev/sdb → OpenSearch data
/dev/sdc → Backup
```

---

# 46. Disk Full වුණාම මොකද වෙන්නේ?

උදාහරණ:

```text
/dev/sda2  100G  100G  0G  100% /
```

Root filesystem 100% නම්:

- Applications fail වෙන්න පුළුවන්
    
- Logs write වෙන්නේ නැතිවෙන්න පුළුවන්
    
- Database problems ඇතිවෙන්න පුළුවන්
    
- Docker containers fail වෙන්න පුළුවන්
    
- Package installation fail වෙන්න පුළුවන්
    
- SSH/login issues පවා ඇතිවෙන්න පුළුවන්
    

Check:

```bash
df -h
```

ඊට පස්සේ:

```bash
du -xh --max-depth=1 /
```

---

# 47. Partition Full vs Inode Full

මේක advanced but very important.

`df -h`:

```bash
df -h
```

Disk blocks usage.

නමුත් files ගොඩක් තිබුණොත් disk space තිබුණත් inode full වෙන්න පුළුවන්.

Check:

```bash
df -i
```

Example:

```text
Filesystem     Inodes  IUsed   IFree IUse%
/dev/sda2      10M     10M       0  100%
```

එතකොට:

```text
Disk space available
BUT
Inodes exhausted
```

Files create කරන්න බැහැ.

---

# 48. Partition Resize

Partition size increase කිරීම advanced operation එකක්.

Architecture:

```text
Disk
 ↓
Partition
 ↓
Filesystem
```

Resize කිරීමේදී මේ layers තුනම consider කරන්න ඕන.

LVM:

```text
LV
 ↓
Filesystem
```

උදාහරණ:

```bash
lvextend
```

ඊට පස්සේ filesystem එක resize කිරීම.

XFS:

```bash
xfs_growfs
```

ext4:

```bash
resize2fs
```

---

# 49. XFS vs ext4 Resize

### XFS

Grow:

```bash
xfs_growfs /data
```

XFS generally cannot be shrunk in-place.

### ext4

Grow:

```bash
resize2fs /dev/sdb1
```

ext4 can generally be shrunk, but this requires careful procedure and usually offline filesystem operations.

---

# 50. Partition එකක් Delete කළොත්?

Suppose:

```text
/dev/sdb1 → /data
```

එකේ data තියෙනවා.

Partition delete කළොත්:

```bash
fdisk /dev/sdb
```

and delete partition:

```text
d
```

**filesystem metadata/data access can be lost.**

ඒ නිසා:

> Partition delete කිරීම ≠ data delete නොවෙයි.

Partition table entry එක remove කළත් underlying sectors වල data තවම තිබෙන්න පුළුවන්, නමුත් filesystem එකට normal access නැති වෙනවා. Recovery එක complicated.

---

# 51. Important Commands Cheat Sheet

### Disks / partitions

```bash
lsblk
lsblk -f
fdisk -l
parted -l
blkid
```

### Disk usage

```bash
df -h
df -i
du -sh /path
du -h --max-depth=1 /path
```

### Mount

```bash
mount
mount /dev/sdb1 /data
umount /data
```

### Filesystem

```bash
mkfs.ext4 /dev/sdb1
mkfs.xfs /dev/sdb1
```

### LVM

```bash
pvs
vgs
lvs

pvdisplay
vgdisplay
lvdisplay
```

### Swap

```bash
swapon --show
free -h
```

---

# 52. මේක Full Architecture එකක් විදිහට මතක තියාගන්න

Linux storage architecture එක මේ chain එක විදිහට තේරුම් ගන්න:

```text
                 PHYSICAL DISK
                      │
                      ▼
              Partition Table
                 │         │
              GPT/MBR     │
                 │
                 ▼
               Partition
                 │
          ┌──────┴───────┐
          │              │
      Filesystem         LVM
      ext4/XFS            │
          │               ▼
          │              PV
          │               │
          │               ▼
          │              VG
          │               │
          │               ▼
          │              LV
          │               │
          └──────┬────────┘
                 ▼
             Filesystem
                 │
                 ▼
            Mount Point
                 │
                 ▼
             /data
```

---

# 53. ඔයා System Engineer කෙනෙක් විදිහට අනිවාර්යයෙන් දැනගන්න ඕන කොටස්

මම priority එකක් දෙනවා නම්:

### Level 1 — Must Know

```text
Disk
Partition
Filesystem
Mount
Mount point
UUID
/etc/fstab
df -h
du -sh
lsblk
blkid
```

### Level 2 — Server Administration

```text
GPT
MBR
EFI
/boot
/
/home
/var
/var/log
swap
```

### Level 3 — Enterprise

```text
LVM
PV
VG
LV
XFS
ext4
Filesystem resize
Disk expansion
```

### Level 4 — Advanced

```text
RAID
LVM + RAID
LVM snapshots
Thin provisioning
Filesystem tuning
Inodes
Disk I/O
iostat
mount options
fstab failure recovery
Storage monitoring
```

**විශේෂයෙන් ඔයා Docker, Jenkins, Graylog/OpenSearch වගේ server workloads එක්ක වැඩ කරන නිසා `LVM + XFS/ext4 + /etc/fstab + disk expansion + filesystem resize +` /var/lib/docker `storage management` කියන කොටස practical විදිහට ඉගෙනගන්න එක ගොඩක් වටිනවා.**