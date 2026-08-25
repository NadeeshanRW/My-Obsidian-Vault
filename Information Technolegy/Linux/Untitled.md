# LVM in Linux — සම්පූර්ණයෙන් තේරුම් ගමු

**LVM = Logical Volume Manager**

Linux server එකක disk storage එක **flexible විදිහට manage කරන්න** භාවිතා කරන storage-management system එකක්.

සාමාන්‍ය partitioning එකේ:

```text
Disk
 │
 ├── /dev/sda1
 ├── /dev/sda2
 └── /dev/sda3
```

Partition එකක් full වුණොත් size වැඩි කිරීම හෝ disk එකක් එකතු කිරීම සමහර අවස්ථාවල inconvenient.

LVM වලදී storage එක abstraction layers කිහිපයකට බෙදනවා:

```text
Physical Disk / Partition
        ↓
Physical Volume (PV)
        ↓
Volume Group (VG)
        ↓
Logical Volume (LV)
        ↓
Filesystem
        ↓
Mount Point
```

මේ hierarchy එක **LVM වල අනිවාර්යයෙන්ම මතක තියාගන්න**.

---

# 1. LVM Architecture

උදාහරණයක් ගමු.

Server එකේ:

```text
/dev/sda
/dev/sdb
```

කියලා disks දෙකක් තියෙනවා.

අපි ඒවා LVM වලට දානවා:

```text
/dev/sda1 ──┐
            ├── Physical Volumes
/dev/sdb1 ──┘
                 ↓
             Volume Group
                 ↓
              vg_data
                 ↓
        ┌────────┼────────┐
        ↓        ↓        ↓
      lv_web   lv_db    lv_backup
```

ඊට පස්සේ එක් එක් LV එකට filesystem එකක් හදනවා:

```text
lv_web
  ↓
XFS
  ↓
/var/www

lv_db
  ↓
XFS
  ↓
/var/lib/mysql
```

---

# 2. PV — Physical Volume

**PV = Physical Volume**

LVM එකට storage එකක් introduce කරන first layer එක.

PV වෙන්න පුළුවන්:

```text
/dev/sda1
/dev/sdb1
/dev/nvme0n1p3
```

උදාහරණය:

```bash
pvcreate /dev/sdb1
```

දැන් `/dev/sdb1` LVM Physical Volume එකක්.

Check කරන්න:

```bash
pvs
```

හෝ:

```bash
pvdisplay
```

---

# 3. VG — Volume Group

**VG = Volume Group**

Physical Volumes එකතු කරලා **එක storage pool එකක්** හදනවා.

උදාහරණය:

```text
PV1 = 100 GB
PV2 = 200 GB

        ↓

VG = 300 GB
```

Create:

```bash
vgcreate vg_data /dev/sda1 /dev/sdb1
```

Check:

```bash
vgs
```

Detailed:

```bash
vgdisplay
```

---

# 4. LV — Logical Volume

**LV = Logical Volume**

Volume Group එකෙන් storage එකක් allocate කරලා Logical Volume එකක් හදනවා.

උදාහරණය:

```text
VG = 300 GB

├── LV web     = 50 GB
├── LV database = 100 GB
└── Free       = 150 GB
```

Create:

```bash
lvcreate -L 50G -n lv_web vg_data
```

Check:

```bash
lvs
```

Detailed:

```bash
lvdisplay
```

---

# 5. Filesystem

LV එක හදලා විතරක් ඒක use කරන්න බැහැ.

Filesystem එකක් create කරන්න ඕන.

උදාහරණයක්:

```bash
mkfs.xfs /dev/vg_data/lv_web
```

හෝ:

```bash
mkfs.ext4 /dev/vg_data/lv_web
```

දැන්:

```text
/dev/vg_data/lv_web
        ↓
      XFS
```

---

# 6. Mount කිරීම

Directory එකක් create කරන්න:

```bash
mkdir /web
```

Mount:

```bash
mount /dev/vg_data/lv_web /web
```

Check:

```bash
df -h
```

ඔයාට වගේ result එකක් එන්න පුළුවන්:

```text
Filesystem                  Size  Used Avail Use%
/dev/mapper/vg_data-lv_web   50G   1G   49G   2%
```

---

# 7. සම්පූර්ණ flow එක

LVM එකේ මේක **අනිවාර්යයෙන් මතක තියාගන්න**:

```text
Physical Disk
     ↓
Partition
     ↓
PV
     ↓
VG
     ↓
LV
     ↓
Filesystem
     ↓
Mount Point
```

Example:

```text
/dev/sdb
   ↓
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
/var/lib/mysql
```

---

# 8. LVM වල ලොකුම advantage එක

Suppose:

```text
vg_data = 500 GB

lv_database = 100 GB
```

Database එකට space මදි වුණා.

Traditional partition එකක නම් resize process එක complicated වෙන්න පුළුවන්.

LVM වල:

```bash
lvextend
```

භාවිතා කරලා LV එක expand කරන්න පුළුවන්.

---

# 9. LV එක වැඩි කිරීම

Suppose:

```text
Current LV = 100 GB
```

තව 50 GB add කරන්න:

```bash
lvextend -L +50G /dev/vg_data/lv_database
```

හෝ:

```bash
lvextend -L 150G /dev/vg_data/lv_database
```

දැන් **වැදගත් point එකක්**:

LV එක expand කළා කියලා filesystem එක automatically හැම filesystem එකකම expand වෙන්නේ නැහැ.

---

# 10. XFS resize

XFS filesystem එකක් නම්:

```bash
xfs_growfs /mount_point
```

උදාහරණය:

```bash
xfs_growfs /var/lib/mysql
```

---

# 11. ext4 resize

ext4 නම්:

```bash
resize2fs /dev/vg_data/lv_database
```

ඒ නිසා:

```text
LV Resize
   ≠
Filesystem Resize
```

මේක System Engineer කෙනෙක්ට **ගොඩක් වැදගත් concept එකක්**.

---

# 12. Disk එකක් add කරලා VG එක expand කිරීම

Suppose:

```text
vg_data = 500 GB
```

New disk:

```text
/dev/sdc
```

Partition එක හදලා:

```text
/dev/sdc1
```

PV create:

```bash
pvcreate /dev/sdc1
```

VG එකට add:

```bash
vgextend vg_data /dev/sdc1
```

දැන්:

```text
Old PV
  │
  ├── 500 GB
  │
  └────┐
       │
       ▼
    vg_data
       ▲
       │
  New PV
  │
  └── 500 GB
```

Total:

```text
VG = 1 TB
```

---

# 13. Free Space concept එක

Suppose:

```text
VG = 1 TB

LV1 = 200 GB
LV2 = 300 GB

Free = 500 GB
```

Check:

```bash
vgs
```

ඔයාට:

```text
VG       VSize   VFree
vg_data  1.00t   500g
```

වගේ result එකක් පේන්න පුළුවන්.

මේ **VFree** එක future LV එකකට allocate කරන්න පුළුවන්.

---

# 14. LVM Snapshot

LVM වල තවත් powerful feature එකක් තමයි **snapshot**.

Suppose:

```text
lv_database
```

database backup/testing සඳහා snapshot එකක් ගන්නවා:

```bash
lvcreate -L 10G -s -n db_snapshot /dev/vg_data/lv_database
```

Structure:

```text
Original LV
    │
    ├───────────────┐
    │               │
    ▼               ▼
Database LV      Snapshot
```

Snapshot එකෙන් particular point-in-time state එකක් capture කරගන්න පුළුවන්.

**හැබැයි snapshot එක backup එකකට full replacement එකක් නෙවෙයි.**

---

# 15. LVM Thin Provisioning

Advanced LVM feature එකක්.

Traditional:

```text
VG = 1 TB

LV = 500 GB

500 GB එක allocate
```

Thin provisioning වලදී logical size එක physical allocation එකට වඩා වෙනස් වෙන්න පුළුවන්.

Example:

```text
Physical available = 500 GB

Thin LV
Logical size = 2 TB
```

මේක virtualization environments වල useful.

හැබැයි monitoring අවශ්‍යයි. Physical pool එක full වුණොත් problems එන්න පුළුවන්.

---

# 16. LVM Commands Cheat Sheet

### Physical Volume

```bash
pvs
pvdisplay
pvcreate
pvremove
```

### Volume Group

```bash
vgs
vgdisplay
vgcreate
vgextend
vgreduce
vgremove
```

### Logical Volume

```bash
lvs
lvdisplay
lvcreate
lvextend
lvreduce
lvremove
lvrename
```

### Filesystem

```bash
mkfs.xfs
mkfs.ext4
xfs_growfs
resize2fs
```

### Disk / filesystem information

```bash
lsblk
blkid
df -h
du -sh
```

---

# 17. Real Server Example

Suppose company server එකක:

```text
/dev/sda = OS disk
/dev/sdb = 500 GB
/dev/sdc = 500 GB
```

We need:

```text
/var/www       = 200 GB
/database      = 400 GB
/backup        = 300 GB
```

Architecture:

```text
/dev/sdb1 ─────────┐
                   │
                   ├── VG: vg_data
                   │
/dev/sdc1 ─────────┘
                         │
             ┌───────────┼───────────┐
             ↓           ↓           ↓
          lv_web      lv_database   lv_backup
           200G          400G         300G
             ↓             ↓            ↓
            XFS           XFS          XFS
             ↓             ↓            ↓
          /var/www      /database     /backup
```

මේක තමයි **real-world LVM architecture එකක්**.

---

# 18. LVM vs Normal Partition

|Feature|Normal Partition|LVM|
|---|---|---|
|Flexible resize|Limited|✅|
|Multiple disks as pool|❌|✅|
|Easy expansion|Limited|✅|
|Snapshots|❌|✅|
|Storage abstraction|❌|✅|
|Server environments|Good|⭐ Excellent|
|Complexity|Low|Medium|

---

# 19. ⚠️ `lvreduce` ගැන විශේෂයෙන්

`lvextend` සාමාන්‍යයෙන් easy.

```bash
lvextend
```

ඒත්:

```bash
lvreduce
```

**dangerous.**

Reason:

```text
Filesystem
   ↓
LV
```

LV එක filesystem එකට වඩා අඩු කළොත් filesystem data destroy වෙන්න පුළුවන්.

විශේෂයෙන් XFS filesystem එක **shrink කරන්න බැහැ**. සාමාන්‍යයෙන් alternative migration/recreate approach එකක් අවශ්‍ය වෙනවා.

ඒ නිසා production server එකක blindly:

```bash
lvreduce
```

run කරන්න එපා.

---

# 🧪 ඔයාට Practical එක මෙහෙම කරන්න

ඔයා කලින් කිව්වා වගේ **එක දවසේ theory + practical දෙකම දාන්නේ නැතුව**, LVM වෙනම practical session එකක් විදිහට කරන්න නම් මේ lab එක හොඳයි:

```text
LVM PRACTICAL LAB

VM
 │
 ├── Disk 1 → OS
 ├── Disk 2 → 10 GB
 └── Disk 3 → 10 GB

        ↓

Create partitions
        ↓
Create PVs
        ↓
Create VG
        ↓
Create LVs
        ↓
Create XFS filesystem
        ↓
Mount
        ↓
Configure /etc/fstab
        ↓
Reboot
        ↓
Verify mounts
        ↓
Extend VG
        ↓
Extend LV
        ↓
Grow filesystem
        ↓
Verify with df -h
```

**මේ practical එක කරලා ඉවර වුණාම LVM ගැන ඇත්තටම වැඩ කරන්න පුළුවන් level එකකට යනවා.**