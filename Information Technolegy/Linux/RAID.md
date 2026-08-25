

**RAID = Redundant Array of Independent Disks**

RAID කියන්නේ physical disks කිහිපයක් එකට combine කරලා **performance, redundancy, availability, හෝ ඒවායේ combination එකක්** ලබාගන්න storage technology එකක්.

සරලව:

```text
Disk 1 ─┐
Disk 2 ─┼──→ RAID Array ──→ Operating System
Disk 3 ─┤
Disk 4 ─┘
```

Linux server එකක RAID use කරනකොට වැදගත්ම දෙයක්:

> **RAID ≠ Backup**

RAID එක disk failure එකකට service availability/protection දෙන්න පුළුවන්. නමුත් accidental deletion, ransomware, corrupted data වගේ දේවල් වලින් backup එකක් වෙනුවට RAID භාවිතා කරන්න බැහැ.

---

# 1. RAID එක අවශ්‍ය ඇයි?

Suppose server එකේ එක disk එකක් විතරයි:

```text
/dev/sda
   │
   └── Application Data
```

Disk එක fail වුණොත්:

```text
Disk Failure
     ↓
Application DOWN
     ↓
Data unavailable
```

RAID භාවිතා කළොත් disks කිහිපයක් එකට use කරලා failure එකකට tolerate කරන්න පුළුවන්.

උදාහරණයක්:

```text
Disk 1 ──┐
Disk 2 ──┼── RAID 1
          │
          └── Same data
```

Disk එකක් fail වුණත් අනෙක් disk එකෙන් system එක continue වෙන්න පුළුවන්.

---

# 2. RAID වල ප්‍රධාන concepts

RAID තේරුම් ගන්න මේ concepts 3ක් හොඳට දැනගන්න.

### ① Striping

Data එක disks කිහිපයකට බෙදනවා.

```text
Data:

A B C D E F G H

Disk 1 → A C E G
Disk 2 → B D F H
```

මේකෙන් **performance** වැඩි කරන්න පුළුවන්.

---

### ② Mirroring

එකම data copy එක disks කිහිපයක තියාගන්නවා.

```text
Disk 1
A B C D

Disk 2
A B C D
```

Disk එකක් fail වුණත් අනෙක් copy එක තියෙනවා.

---

### ③ Parity

Data recovery සඳහා additional mathematical information store කරනවා.

```text
Disk 1 → Data
Disk 2 → Data
Disk 3 → Parity
```

එක් disk එකක් fail වුණාම parity භාවිතා කරලා missing data reconstruct කරන්න පුළුවන්.

---

# 3. RAID 0

**RAID 0 = Striping**

Minimum:

```text
2 disks
```

Data disks දෙකට split කරනවා.

```text
        DATA
          │
     ┌────┴────┐
     ↓         ↓
 Disk 1      Disk 2
 A C E G      B D F H
```

### Capacity

Disks:

```text
500 GB
500 GB
```

RAID 0:

```text
= 1000 GB
```

### Performance

🔥 Very good

### Redundancy

❌ None

Disk එකක් fail වුණොත්:

```text
RAID 0 → FAILED
```

### Use cases

- Temporary high-speed storage
    
- Scratch data
    
- Non-critical workloads
    

**Production critical data සඳහා generally avoid කරන්න.**

---

# 4. RAID 1

**RAID 1 = Mirroring**

Minimum:

```text
2 disks
```

Data එක disks දෙකේම duplicate වෙනවා.

```text
       DATA
         │
    ┌────┴────┐
    ↓         ↓
 Disk 1     Disk 2
 A B C D     A B C D
```

Disks:

```text
500 GB
500 GB
```

Usable capacity:

```text
500 GB
```

මොකද එක disk එකේ copy එකක් විතරක් usable data capacity ලෙස ගන්නවා.

### Redundancy

✅ Excellent

එක් disk එකක් fail වුණත්:

```text
Disk 1 → FAILED ❌

Disk 2 → Working ✅
```

System එක continue වෙන්න පුළුවන්.

### Use cases

- OS disks
    
- Critical servers
    
- Small databases
    
- Boot drives
    

---

# 5. RAID 5

**RAID 5 = Striping + Distributed Parity**

Minimum:

```text
3 disks
```

Example:

```text
Disk 1     Disk 2     Disk 3
-------    -------    -------
Data A     Data B     Parity
Data C     Parity     Data D
Parity     Data E     Data F
```

Parity එක එකම disk එකක නෙවෙයි; disks අතර distribute වෙනවා.

### Capacity

3 × 1 TB disks:

```text
Raw = 3 TB

Usable ≈ 2 TB
```

Formula:

```text
Usable = (N - 1) × smallest_disk
```

### Failure tolerance

**1 disk failure**

```text
Disk 1 ❌
Disk 2 ✅
Disk 3 ✅

Array → Can continue
```

### Performance

Read:

✅ Good

Write:

⚠️ More overhead because parity calculation is required.

---

# 6. RAID 6

RAID 6 = Striping + **Double Parity**

Minimum:

```text
4 disks
```

RAID 5 එකට සමානයි, නමුත් parity blocks දෙකක් තියෙනවා.

```text
Disk 1    Disk 2    Disk 3    Disk 4
-------   -------   -------   -------
Data      Data      P1        P2
Data      P1        P2        Data
P1        P2        Data      Data
```

### Failure tolerance

**2 disks can fail.**

```text
Disk 1 ❌
Disk 2 ❌

Disk 3 ✅
Disk 4 ✅

RAID 6 → Still operational
```

### Capacity

Formula:

```text
Usable = (N - 2) × smallest_disk
```

Example:

```text
4 × 2 TB

Usable ≈ 4 TB
```

### Use cases

- Large storage arrays
    
- NAS
    
- Backup storage
    
- Large data repositories
    

---

# 7. RAID 10

**RAID 10 = RAID 1 + RAID 0**

Minimum:

```text
4 disks
```

First mirroring:

```text
Disk 1 ── Disk 2
   Mirror
```

Another mirror:

```text
Disk 3 ── Disk 4
   Mirror
```

Then stripe across the mirrors:

```text
          RAID 10

     ┌───────────────┐
     │               │
 Mirror 1          Mirror 2
 D1 ↔ D2           D3 ↔ D4
     │               │
     └───────┬───────┘
             ↓
          Striping
```

### Capacity

4 × 1 TB:

```text
Raw = 4 TB

Usable ≈ 2 TB
```

### Performance

🔥 Excellent

### Redundancy

🔥 Excellent

Usually it can tolerate multiple disk failures **as long as both disks in the same mirror pair don't fail**.

Example:

```text
D1 ❌
D3 ❌

D2 ✅
D4 ✅

RAID 10 → Still works
```

But:

```text
D1 ❌
D2 ❌

Same mirror failed

RAID 10 → FAILED
```

---

# 8. RAID comparison

|RAID|Min Disks|Technique|Usable Capacity|Disk Failure|
|---|--:|---|--:|--:|
|RAID 0|2|Striping|100%|0|
|RAID 1|2|Mirroring|~50%|1|
|RAID 5|3|Striping + Parity|~N-1|1|
|RAID 6|4|Striping + Double Parity|~N-2|2|
|RAID 10|4|Mirror + Stripe|~50%|Depends on mirror pairs|

---

# 9. RAID 0 vs RAID 1

මේ දෙක confuse වෙන්න ඉඩ වැඩියි.

### RAID 0

```text
Data
 ↓
┌─────────┐
│         │
D1        D2
A C E     B D F
```

Goal:

> **Performance**

Protection:

> ❌ None

---

### RAID 1

```text
Data
 ↓
┌─────────┐
│         │
D1        D2
A B C     A B C
```

Goal:

> **Redundancy**

Performance:

> Good reads

---

# 10. RAID 5 vs RAID 6

### RAID 5

```text
Minimum = 3 disks
Failure = 1 disk
```

### RAID 6

```text
Minimum = 4 disks
Failure = 2 disks
```

Large modern storage environments වල RAID 6 useful වෙන්නේ rebuild time එක long වෙන්න පුළුවන් නිසා.

---

# 11. RAID 10 vs RAID 5

Suppose database server එකක්.

### RAID 5

```text
Capacity → Better
Write performance → Lower
Failure → 1 disk
```

### RAID 10

```text
Capacity → Lower
Write performance → Excellent
Failure → Depends on mirror layout
```

Database workloads වල **RAID 10** frequently preferred when performance and predictable write behavior are important.

---

# 12. Linux වල Software RAID

Linux වල software RAID manage කරන්න commonly භාවිතා කරන්නේ:

```bash
mdadm
```

**mdadm = Multiple Device Administration**

Check:

```bash
mdadm --detail
```

Available RAID arrays:

```bash
cat /proc/mdstat
```

මේ command එක especially useful.

```bash
cat /proc/mdstat
```

Example:

```text
Personalities : [raid1]

md0 : active raid1 sdb1[0] sdc1[1]
      1047552 blocks [2/2] [UU]
```

`[UU]` කියන්නේ දෙකම healthy.

```text
[UU]
```

= Disk 1 ✅ + Disk 2 ✅

```text
[U_]
```

= Disk 1 ✅ + Disk 2 ❌

---

# 13. Linux RAID 1 Practical

Suppose:

```text
/dev/sdb
/dev/sdc
```

disks දෙකක් තියෙනවා.

Partitions create කරලා:

```text
/dev/sdb1
/dev/sdc1
```

RAID 1 create:

```bash
mdadm --create /dev/md0 --level=1 --raid-devices=2 /dev/sdb1 /dev/sdc1
```

Check:

```bash
cat /proc/mdstat
```

Detailed:

```bash
mdadm --detail /dev/md0
```

Filesystem:

```bash
mkfs.xfs /dev/md0
```

Mount:

```bash
mkdir /data
mount /dev/md0 /data
```

Check:

```bash
df -h
```

---

# 14. RAID Disk Failure Simulation

මේක තමයි **real practical එකේ වැදගත්ම part එක**.

RAID 1:

```text
/dev/sdb1
/dev/sdc1
```

Suppose `/dev/sdc1` fail වුණා.

Check:

```bash
cat /proc/mdstat
```

ඔයාට:

```text
[U_]
```

වගේ status එකක් පේන්න පුළුවන්.

Detailed:

```bash
mdadm --detail /dev/md0
```

ඊට පස්සේ failed disk remove:

```bash
mdadm /dev/md0 --fail /dev/sdc1
```

Remove:

```bash
mdadm /dev/md0 --remove /dev/sdc1
```

New disk/partition එක add:

```bash
mdadm /dev/md0 --add /dev/sdc1
```

Rebuild වෙන්න පටන් ගන්නවා.

Monitor:

```bash
watch cat /proc/mdstat
```

මේ practical එකෙන් **RAID එකේ actual purpose එක** හොඳට තේරෙනවා.

---

# 15. RAID + LVM

ඔයා දැන් LVM ඉගෙනගත්ත නිසා මේ දෙක connect කරලා බලන්න.

Possible architecture:

```text
Physical Disks
      ↓
     RAID
      ↓
    /dev/md0
      ↓
     LVM
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
/dev/sdb ─┐
          ├── RAID 10 → /dev/md0
/dev/sdc ─┤
/dev/sdd ─┤
/dev/sde ─┘
                ↓
              LVM PV
                ↓
              VG data
                ↓
        ┌───────┴────────┐
        ↓                ↓
     lv_db            lv_files
        ↓                ↓
      XFS              XFS
        ↓                ↓
   /var/lib/db        /data
```

මේ architecture එක enterprise environments වල තේරුම් ගන්න **ගොඩක් වැදගත්**.

---

# 16. Hardware RAID vs Software RAID

### Software RAID

Operating system එක RAID manage කරනවා.

```text
Linux
 ↓
mdadm
 ↓
RAID
 ↓
Disks
```

Advantages:

- Flexible
    
- Low cost
    
- Linux integrated
    
- Easy automation
    

---

### Hardware RAID

Dedicated RAID controller එකක් RAID management කරනවා.

```text
Linux
 ↓
RAID Controller
 ↓
Disks
```

Advantages:

- Dedicated controller
    
- Controller-level management
    
- Enterprise features depending on hardware
    

But hardware RAID controller එක itself fail වුණොත් controller compatibility/replacement considerations තියෙනවා.

---

# 17. RAID එකේ biggest mistake

❌ මෙහෙම හිතන්න එපා:

```text
RAID = Backup
```

Correct:

```text
RAID
 ↓
Availability / Disk Failure Protection
```

Backup:

```text
Backup
 ↓
Data Recovery
```

උදාහරණයක්:

```text
RAID 1
Disk 1 → A B C D
Disk 2 → A B C D
```

ඔයා accidentally:

```bash
rm -rf /data/*
```

කළොත්...

RAID දෙකේම data delete වෙනවා.

ඒ නිසා:

```text
RAID + Backup + DR
```

තමයි proper production strategy එක.

---

# 🧠 මතක තියාගන්න shortcut එක

```text
RAID 0
→ SPEED
→ NO PROTECTION

RAID 1
→ MIRROR
→ 1 DISK FAILURE

RAID 5
→ PARITY
→ 1 DISK FAILURE

RAID 6
→ DOUBLE PARITY
→ 2 DISK FAILURES

RAID 10
→ SPEED + MIRROR
→ PERFORMANCE + REDUNDANCY
```

### ඔයාගේ Linux/System Engineer learning path එකට

RAID ඉගෙනගන්න order එක:

```text
1. RAID concepts
      ↓
2. RAID 0 / 1 / 5 / 6 / 10
      ↓
3. Hardware vs Software RAID
      ↓
4. mdadm
      ↓
5. RAID monitoring
      ↓
6. Disk failure
      ↓
7. Rebuild
      ↓
8. RAID + LVM
      ↓
9. RAID + Backup
      ↓
10. RAID + DR
```

**RAID practical එකට නම් මම recommend කරන්නේ VM එකේ virtual disks 4ක් දාලා `RAID 10` එකක් build කරලා, එක disk එක deliberately fail කරලා rebuild කරන lab එක.** ඒක කළොත් RAID ගැන interview-level එකට වඩා real System Engineer level understanding එකක් ලැබෙනවා.