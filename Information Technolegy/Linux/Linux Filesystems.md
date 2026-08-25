<<<<<<< HEAD

Linux වල **Filesystem** කියන්නේ files save කරන ක්‍රමයක් විතරක් නෙවෙයි. Disk එකේ තියෙන raw storage එක Linux OS එකට **files, directories, metadata, permissions, ownership, links, timestamps, free space** වගේ දේවල් manage කරන්න පුළුවන් විදිහට organize කරන system එක.

System Engineer කෙනෙක් විදිහට Linux Filesystems හොඳට දැනගන්න නම් මේ flow එක තේරුම් ගන්න එක වැදගත්:

```text
Physical Disk
     ↓
Partition
     ↓
Filesystem
     ↓
Mount Point
     ↓
Directory
     ↓
Files
```

ඒක එකින් එක බලමු.

---

# 1. Filesystem කියන්නේ මොකක්ද?

සරලව:

> **Filesystem එකක් කියන්නේ storage device එකක data organize, store, retrieve සහ manage කරන structure එක.**

උදාහරණයක්:

ඔයාට 1 TB HDD එකක් තියෙනවා.

Raw disk එක:

```text
/dev/sdb
```

මේක Linux එකට තියෙන raw block storage device එකක්.

Filesystem එකක් නැත්නම් Linux එකට මේකේ:

```text
file1.txt
photo.jpg
backup.zip
database.sql
```

වගේ files manage කරන්න පුළුවන් structured environment එකක් නැහැ.

Filesystem එකක් create කළාම:

```text
/dev/sdb
   ↓
Filesystem
   ↓
Mount
   ↓
/data
   ↓
files
```

වගේ use කරන්න පුළුවන්.

---

# 2. Filesystem එකක් අවශ්‍ය ඇයි?

Disk එකේ data bytes විතරයි තියෙන්නේ.

Filesystem එක Linux වලට කියනවා:

- මේ data එක file එකක්
    
- මේක directory එකක්
    
- මේ file එකේ owner කවුද
    
- permissions මොනවාද
    
- file size එක කීයද
    
- file එක create කළේ කවදාද
    
- data blocks කොහෙද තියෙන්නේ
    
- free space කොච්චරද
    
- deleted files වල blocks reuse කරන්න පුළුවන්ද
    

වගේ දේවල්.

ඒ නිසා:

```text
Disk
 ↓
Raw blocks
 ↓
Filesystem
 ↓
Files + Directories + Metadata
```

---

# 3. Linux Filesystem එකේ Basic Structure

Linux වල filesystem hierarchy එක root `/` වලින් පටන් ගන්නවා.

```text
/
├── bin
├── boot
├── dev
├── etc
├── home
├── lib
├── media
├── mnt
├── opt
├── proc
├── root
├── run
├── sbin
├── srv
├── sys
├── tmp
├── usr
└── var
```

මේක **Filesystem Hierarchy Standard (FHS)** concept එකට සම්බන්ධයි.

---

# 4. `/` — Root Filesystem

Linux filesystem එකේ top-level directory එක:

```text
/
```

මේකට:

> **Root directory**

කියනවා.

උදාහරණ:

```text
/etc
/home
/var
/usr
/opt
/tmp
```

මේ හැම එකක්ම `/` යටතේ තියෙනවා.

---

# 5. `/home`

සාමාන්‍ය users ලගේ home directories මෙහි තියෙනවා.

```text
/home
├── alice
├── bob
└── developer
```

උදාහරණ:

```text
/home/alice
```

Alice ගේ personal files:

```text
/home/alice/Documents
/home/alice/Downloads
/home/alice/scripts
```

---

# 6. `/root`

`root` userගේ home directory එක:

```text
/root
```

මේක:

```text
/
```

සහ:

```text
/root
```

එකම දෙයක් නෙවෙයි.

`/` = root filesystem directory

`/root` = root userගේ home directory.

---

# 7. `/etc`

System configuration files.

උදාහරණ:

```text
/etc/hosts
/etc/fstab
/etc/passwd
/etc/group
/etc/ssh/sshd_config
```

System administration වල `/etc` ඉතාම වැදගත්.

---

# 8. `/var`

Frequently changing data:

```text
/var/log
/var/cache
/var/lib
/var/tmp
```

උදාහරණ:

```text
/var/log/syslog
/var/log/messages
/var/lib/docker
```

Docker data, logs, package data වගේ දේවල් `/var` යටතේ තියෙන්න පුළුවන්.

---

# 9. `/var/log`

System/application logs.

උදාහරණ:

```text
/var/log/syslog
/var/log/messages
/var/log/auth.log
```

RHEL/Rocky/AlmaLinux:

```text
/var/log/secure
/var/log/messages
```

---

# 10. `/usr`

Installed applications, binaries, libraries, documentation වගේ system resources ගොඩක් මෙහි තියෙනවා.

උදාහරණ:

```text
/usr/bin
/usr/sbin
/usr/lib
/usr/share
```

---

# 11. `/bin`

Essential user commands සඳහා binaries.

Modern Linux distributions වල `/bin` බොහෝ විට `/usr/bin` වෙත symbolic link එකක් විය හැකියි.

---

# 12. `/sbin`

System administration binaries.

උදාහරණ:

```text
mount
fsck
ip
```

Modern distributions වල `/sbin` → `/usr/sbin` merge කරලා තියෙන්න පුළුවන්.

---

# 13. `/boot`

Boot files.

උදාහරණ:

```text
/boot/vmlinuz...
/boot/initramfs...
/boot/grub2
```

මෙහි Linux kernel සහ bootloader-related files තියෙන්න පුළුවන්.

---

# 14. `/dev`

Device files.

Linux වල:

> **Everything is treated as a file**

කියන concept එක තියෙනවා.

උදාහරණ:

```text
/dev/sda
/dev/sdb
/dev/nvme0n1
/dev/null
/dev/random
```

Disk එක:

```text
/dev/sda
```

Partition:

```text
/dev/sda1
/dev/sda2
```

---

# 15. `/proc`

`proc` කියන්නේ virtual filesystem එකක්.

මෙය disk එකේ normal files වගේ permanently store කරපු data නෙවෙයි.

Kernel එක runtime එකේ information provide කරනවා.

උදාහරණ:

```text
/proc/cpuinfo
/proc/meminfo
/proc/loadavg
/proc/uptime
```

බලන්න:

```bash
cat /proc/cpuinfo
```

---

# 16. `/sys`

`sysfs` කියන්නේ kernel සහ hardware/device information expose කරන virtual filesystem එකක්.

උදාහරණ:

```text
/sys/class
/sys/block
/sys/devices
```

Hardware/device management වලදී useful.

---

# 17. `/run`

Runtime data.

System boot එකෙන් පස්සේ runtime information සඳහා භාවිතා කරනවා.

උදාහරණ:

```text
/run
/run/lock
/run/user
```

---

# 18. `/tmp`

Temporary files.

```text
/tmp
```

Applications සහ users temporary data store කරන්න භාවිතා කරනවා.

⚠️ `/tmp` data permanent storage එකක් ලෙස use කරන්න එපා.

---

# 19. `/opt`

Optional/additional software සඳහා.

උදාහරණයක්:

```text
/opt/myapp
/opt/oracle
/opt/company-app
```

Third-party applications සඳහා common location එකක්.

---

# 20. `/mnt`

Temporary/manual mounts සඳහා historically common directory එක.

උදාහරණ:

```bash
mount /dev/sdb1 /mnt
```

---

# 21. `/media`

Removable media සඳහා.

උදාහරණ:

```text
/media/user/USB
```

USB/CD/DVD වගේ removable devices mount වෙන්න පුළුවන්.

---

# 22. `/srv`

Services විසින් serve කරන data සඳහා.

උදාහරණ:

```text
/srv/www
/srv/ftp
```

---

# 23. Linux Filesystem එකේ Physical Structure

දැන් actual storage side එක බලමු.

ඔයාට disk එකක් තියෙනවා:

```text
/dev/sda
```

ඒක partition කරලා:

```text
/dev/sda1
/dev/sda2
/dev/sda3
```

වගේ හදාගන්න පුළුවන්.

ඊට පස්සේ partition එකකට filesystem එකක් create කරනවා:

```text
/dev/sda1
     ↓
ext4
```

ඊට පස්සේ mount කරනවා:

```text
/dev/sda1
     ↓
/data
```

එතකොට:

```text
/data
```

directory එකෙන් disk එක access කරන්න පුළුවන්.

---

# 24. Partition සහ Filesystem අතර වෙනස

මේ දෙක confuse කරන්න එපා.

### Partition

Disk එක logical sections වලට divide කිරීම.

```text
/dev/sda
│
├── /dev/sda1
├── /dev/sda2
└── /dev/sda3
```

### Filesystem

Partition එක ඇතුළේ files organize කරන structure එක.

```text
/dev/sda1
     ↓
ext4
```

ඒ නිසා:

```text
Disk
 ↓
Partition
 ↓
Filesystem
 ↓
Mount Point
 ↓
Files
```

---

# 25. Filesystem Create කිරීම

උදාහරණයක්:

```bash
mkfs.ext4 /dev/sdb1
```

මේකෙන් `/dev/sdb1` මත ext4 filesystem එකක් create කරනවා.

⚠️ **මේ command එකේදී device එක වැරදිව select කළොත් data destroy වෙන්න පුළුවන්.**

Production server එකක `mkfs` run කිරීමට පෙර device එක verify කරන්න.

---

# 26. Filesystem Mount කිරීම

Filesystem එක create කළාට පස්සේ use කරන්න mount කරන්න ඕන.

Directory එකක් හදමු:

```bash
mkdir /data
```

Mount:

```bash
mount /dev/sdb1 /data
```

දැන්:

```text
/dev/sdb1
    ↓
/data
```

---

# 27. Mount කියන්නේ මොකක්ද?

Mount කියන්නේ:

> Filesystem එකක් Linux directory tree එකකට attach කිරීම.

උදාහරණ:

```text
/dev/sdb1
    │
    │ mount
    ↓
/data
```

දැන්:

```bash
cd /data
```

කරලා disk එකේ files access කරන්න පුළුවන්.

---

# 28. Mount Point

Filesystem එක attach කරන directory එක:

> **Mount Point**

කියනවා.

උදාහරණ:

```text
/dev/sdb1 → /data
```

මෙහි:

```text
/dev/sdb1 = Device
/data      = Mount Point
```

---

# 29. Mounted Filesystems බලන්න

```bash
mount
```

හෝ:

```bash
findmnt
```

හෝ:

```bash
df -h
```

`findmnt` command එක filesystem mount relationships තේරුම් ගන්න ඉතාම useful.

---

# 30. `df -h`

Disk usage බලන්න ප්‍රධාන command එක:

```bash
df -h
```

Output:

```text
Filesystem      Size  Used Avail Use% Mounted on
/dev/sda2        50G   30G   18G  63% /
/dev/sdb1       500G  200G  275G  43% /data
```

මෙහි:

|Column|Meaning|
|---|---|
|Filesystem|Device/filesystem|
|Size|Total size|
|Used|Used space|
|Avail|Available space|
|Use%|Usage percentage|
|Mounted on|Mount point|

---

# 31. `du`

`df` කියන්නේ filesystem level එකේ usage.

`du` කියන්නේ directory/file level එකේ usage.

```bash
du -sh /var/log
```

උදාහරණ:

```text
2.4G    /var/log
```

Top directories බලන්න:

```bash
du -sh /var/* | sort -h
```

මේක disk space troubleshooting වලදී ඉතාම useful.

---

# 32. `lsblk`

Block devices බලන්න:

```bash
lsblk
```

Example:

```text
NAME   SIZE TYPE MOUNTPOINT
sda    100G disk
├─sda1  1G  part /boot
└─sda2 99G  part /
sdb    500G disk
└─sdb1 500G part /data
```

මේකෙන්:

```text
Disk
 ↓
Partition
 ↓
Mount
```

structure එක ඉක්මනින් බලන්න පුළුවන්.

---

# 33. `blkid`

Filesystem type සහ UUID බලන්න:

```bash
blkid
```

Example:

```text
/dev/sda2: UUID="abc123" TYPE="ext4"
/dev/sdb1: UUID="xyz789" TYPE="xfs"
```

---

# 34. Filesystem Type බලන්නේ කොහොමද?

```bash
df -Th
```

Example:

```text
Filesystem     Type  Size  Used Avail Use% Mounted on
/dev/sda2      ext4   50G   30G   18G  63% /
/dev/sdb1      xfs   500G  200G  300G  40% /data
```

`T`:

> Filesystem type

`h`:

> Human-readable

---

# 35. Linux වල ප්‍රධාන Filesystem Types

Linux වල filesystem types ගොඩක් තියෙනවා.

වැදගත් ඒවා:

```text
ext4
XFS
Btrfs
ext3
ext2
ZFS
NFS
tmpfs
FAT32
exFAT
NTFS
```

Server administration වලදී විශේෂයෙන්:

```text
ext4
XFS
Btrfs
NFS
tmpfs
```

ගැන දැනගන්න වැදගත්.

---

# 36. ext4

**ext4 = Fourth Extended Filesystem**

Linux වල ඉතාම ජනප්‍රිය filesystem එකක්.

Features:

- Journaling
    
- Large filesystem support
    
- Large files
    
- Extents
    
- Good performance
    
- Reliable
    
- Mature
    

Ubuntu/Debian වගේ distributions වල බහුලව දකින්න පුළුවන්.

Check:

```bash
df -Th
```

---

# 37. ext3

ext2 වලට journaling එක add කරපු filesystem එක.

```text
ext2
 ↓
ext3
 ↓
ext4
```

ext3 දැන් modern systems වල ext4 තරම් common නෑ.

---

# 38. ext2

Older Linux filesystem එකක්.

ප්‍රධාන limitation එක:

> Journaling නැහැ.

ඒ නිසා modern production systems වල ext4/XFS වඩා common.

---

# 39. XFS

XFS කියන්නේ high-performance journaling filesystem එකක්.

RHEL ecosystem එකේ ඉතාම common:

```text
RHEL
Rocky Linux
AlmaLinux
```

වගේ systems වල.

XFS හොඳ වෙන්නේ:

- Large files
    
- Large filesystems
    
- High-performance workloads
    
- Server environments
    
- Parallel I/O
    

වගේ scenarios වල.

---

# 40. ext4 vs XFS

|Feature|ext4|XFS|
|---|---|---|
|Linux support|Excellent|Excellent|
|Journaling|Yes|Yes|
|General purpose|Excellent|Excellent|
|Large filesystem|Good|Excellent|
|RHEL environments|Common|Very common|
|Shrink filesystem|Supported offline|Not supported|
|Mature|Yes|Yes|

විශේෂයෙන් මතක තියාගන්න:

> **XFS filesystem එක shrink කරන්න බැහැ.**

XFS resize කිරීමේදී growth support තියෙනවා, නමුත් shrinking සඳහා recreate/migrate වගේ approach එකක් අවශ්‍ය වෙනවා.

---

# 41. Btrfs

Btrfs = **B-tree filesystem**

Modern advanced filesystem එකක්.

Features:

- Copy-on-write
    
- Snapshots
    
- Subvolumes
    
- Checksums
    
- Compression
    
- Send/receive
    
- Online resize
    

උදාහරණයක්:

```text
Filesystem
   ↓
Snapshot
   ↓
Backup / Rollback
```

Virtualization සහ snapshot-heavy environments වල useful.

---

# 42. ZFS

ZFS කියන්නේ advanced storage/filesystem technology එකක්.

ප්‍රධාන features:

- Storage pools
    
- RAID-like redundancy
    
- Checksums
    
- Snapshots
    
- Compression
    
- Data integrity
    
- Self-healing capabilities with redundancy
    
- Large storage management
    

Proxmox, NAS, storage environments වගේ places වල ZFS ගැන අනිවාර්යයෙන්ම අහන්න පුළුවන්.

---

# 43. NFS

NFS = **Network File System**

Remote server එකක filesystem එක network එක හරහා mount කරන්න.

උදාහරණ:

```text
Server A
/data
   ↓
Network
   ↓
Server B
/mnt/data
```

Server B එකෙන්:

```bash
mount serverA:/data /mnt/data
```

ඒකෙන් remote storage එක local directory එකක් වගේ access කරන්න පුළුවන්.

---

# 44. tmpfs

`tmpfs` RAM සහ swap backed temporary filesystem එකක්.

Check:

```bash
df -Th
```

`/run` වගේ locations වල tmpfs භාවිතා වෙනවා.

Characteristics:

- Fast
    
- Temporary
    
- Reboot එකෙන් data නැතිවෙන්න පුළුවන්
    
- Memory pressure එකට අනුව RAM/swap භාවිතා කළ හැක
    

---

# 45. Virtual Filesystems

Linux වල හැම filesystem එකක්ම physical disk එකක stored වෙලා නැහැ.

උදාහරණ:

```text
/proc
/sys
/dev
/run
```

මේවා kernel/system runtime information expose කරන virtual/pseudo filesystems.

ඒ නිසා:

```text
Filesystem ≠ Always physical disk
```

---

# 46. inode කියන්නේ මොකක්ද?

මේක Linux filesystem වල **ඉතාම වැදගත් concept එකක්**.

inode කියන්නේ file එකේ **metadata** store කරන data structure එකක්.

සාමාන්‍යයෙන් inode එකේ:

- File type
    
- Permissions
    
- Owner UID
    
- Group GID
    
- File size
    
- Timestamps
    
- Link count
    
- Data blocks වල locations
    

වගේ information තියෙනවා.

---

# 47. inode එකේ file name තියෙනවද?

**සාමාන්‍යයෙන් inode එකේ filename එක store වෙන්නේ නැහැ.**

Directory එක:

```text
filename → inode number
```

mapping එකක් maintain කරනවා.

Conceptually:

```text
Directory
│
├── report.txt → inode 12345
├── image.jpg  → inode 12346
└── app.log    → inode 12347
```

inode `12345` එකේ:

```text
permissions
owner
size
timestamps
data block pointers
```

වගේ metadata තියෙනවා.

---

# 48. inode Number බලන්නේ කොහොමද?

```bash
ls -i
```

Example:

```text
12345 report.txt
12346 image.jpg
```

inode number එක:

```text
12345
```

---

# 49. inode Exhaustion

Filesystem එකේ disk space තියෙනවා.

නමුත්:

```text
No space left on device
```

error එක එනවා.

ඒක inode exhaustion වෙන්න පුළුවන්.

Check:

```bash
df -i
```

Example:

```text
Filesystem      Inodes   IUsed   IFree IUse%
/dev/sda2      1000000 1000000       0  100%
```

Disk space:

```bash
df -h
```

100% නොවුණත් inode 100% වෙලා නම් new files create කරන්න බැරි වෙන්න පුළුවන්.

---

# 50. Hard Link

Hard link එක inode එක share කරනවා.

```text
file1
  ↓
inode 12345
  ↑
file2
```

Create:

```bash
ln file1 file2
```

Check:

```bash
ls -li
```

Files දෙකේ inode number එක same වෙයි.

---

# 51. Symbolic Link

Symbolic link එක වෙනත් path එකකට pointer එකක් වගේ.

```bash
ln -s /var/log/app.log /tmp/app.log
```

Concept:

```text
/tmp/app.log
      ↓
/var/log/app.log
```

Check:

```bash
ls -l
```

---

# 52. Hard Link vs Symbolic Link

|Feature|Hard Link|Symbolic Link|
|---|---|---|
|Same inode|Yes|No|
|Points to|inode|Path|
|Can cross filesystem|No|Yes|
|Directory hard links|Normally no|Yes|
|Original delete කළොත්|Data remains|Link breaks|

---

# 53. Filesystem Journaling

Modern filesystems like:

```text
ext4
XFS
```

journaling support කරනවා.

Journaling කියන්නේ filesystem changes ගැන journal/log එකක් maintain කිරීම.

Concept:

```text
Application
   ↓
Filesystem change
   ↓
Journal
   ↓
Actual metadata/data changes
```

Power failure/crash එකක් වුණාම filesystem recovery පහසු වෙනවා.

---

# 54. Why Journaling is Important?

හිතන්න server එකේ:

```text
File update වෙමින් තියෙනවා
       ↓
Power failure
       ↓
Server shutdown
```

Journaling filesystem එකට incomplete filesystem operations recover කරන්න mechanism එකක් තියෙනවා.

එය:

> Data backup එකක් නෙවෙයි.

Journaling කියන්නේ **filesystem consistency/recovery** සඳහා.

---

# 55. Mount Options

Filesystem එක mount කරනකොට options දෙන්න පුළුවන්.

Example:

```bash
mount -o ro /dev/sdb1 /data
```

`ro`:

> Read-only

`rw`:

> Read-write

තවත් options:

```text
noexec
nosuid
nodev
defaults
relatime
```

Security hardening වලදී මේවා වැදගත්.

---

# 56. `ro` — Read Only

```bash
mount -o ro /dev/sdb1 /data
```

User ට filesystem එකේ data read කරන්න පුළුවන්.

Write කරන්න බැහැ.

Useful:

- Investigation
    
- Recovery
    
- Forensics
    
- Protecting data
    

---

# 57. `noexec`

```text
noexec
```

mount option එකෙන් filesystem එකේ binaries execute කිරීම restrict කරන්න පුළුවන්.

Temporary/upload directories වගේ locations වල security hardening සඳහා use කරන්න පුළුවන්.

---

# 58. `nosuid`

`nosuid` option එකෙන් setuid/setgid behavior restrict කරන්න පුළුවන්.

Security hardening වලදී useful.

---

# 59. `nodev`

Filesystem එකේ device nodes interpret කිරීම prevent කිරීමට use කරන mount option එක.

Security hardening වලදී:

```text
/tmp
/data
```

වගේ mount points සඳහා context එක අනුව use කරන්න පුළුවන්.

---

# 60. `/etc/fstab`

Reboot කළාට පස්සේ manually:

```bash
mount /dev/sdb1 /data
```

run කරන්න ඕන නැතිව automatic mount කරන්න:

```text
/etc/fstab
```

භාවිතා කරනවා.

Example:

```text
UUID=abc123  /data  ext4  defaults  0  2
```

Meaning:

```text
UUID      → Filesystem identifier
/data     → Mount point
ext4      → Filesystem type
defaults  → Mount options
0         → dump
2         → fsck order
```

---

# 61. UUID කියන්නේ මොකක්ද?

Filesystem එකකට unique identifier එකක්.

බලන්න:

```bash
blkid
```

Example:

```text
/dev/sdb1: UUID="abc123-def456" TYPE="ext4"
```

`/etc/fstab` එකේ:

```text
UUID=abc123-def456 /data ext4 defaults 0 2
```

භාවිතා කරන එක:

```text
/dev/sdb1
```

වලට වඩා stable වෙන්න පුළුවන්.

---

# 62. `/etc/fstab` වෙනස් කළාට පස්සේ

Reboot කරන්න කලින් test කරන්න:

```bash
mount -a
```

මේකෙන් `/etc/fstab` entries mount කරන්න try කරනවා.

⚠️ Production server එකක `fstab` වැරදියට edit කළොත් boot problems ඇතිවෙන්න පුළුවන්. ඒ නිසා:

```bash
mount -a
```

run කරලා errors නැද්ද බලන්න.

---

# 63. Unmount

Filesystem එක unmount කරන්න:

```bash
umount /data
```

හෝ:

```bash
umount /dev/sdb1
```

Unmount වෙන්නේ නැත්නම්:

```text
target is busy
```

වගේ error එකක් එන්න පුළුවන්.

---

# 64. "Target is busy" Troubleshooting

මුලින් බලන්න:

```bash
lsof /data
```

හෝ:

```bash
fuser -vm /data
```

උදාහරණ:

```text
PID
USER
COMMAND
```

වගේ information ලැබෙනවා.

හේතුව:

- User එක `/data` directory එකේ
    
- Application එක file එක use කරනවා
    
- Process එක directory එක access කරනවා
    

වෙන්න පුළුවන්.

---

# 65. Filesystem Check — `fsck`

Filesystem errors check/repair කරන්න:

```bash
fsck
```

Ext4 සඳහා:

```bash
fsck.ext4 /dev/sdb1
```

හෝ:

```bash
e2fsck /dev/sdb1
```

⚠️ **Mounted, active filesystem එකකට blindly `fsck` run කරන්න එපා.**

සාමාන්‍යයෙන් filesystem එක unmounted වෙලා තියෙන්න ඕන.

---

# 66. XFS Filesystem Check

XFS සඳහා:

```bash
xfs_repair /dev/sdb1
```

XFS-specific tools භාවිතා කරනවා.

---

# 67. Filesystem Resize

Disk එක:

```text
100 GB
```

වෙලා පස්සේ:

```text
200 GB
```

කරලා expand කළා කියලා හිතන්න.

Filesystem එක automatically 200 GB use කරනවා කියලා assume කරන්න බැහැ.

Flow එක:

```text
Disk
 ↓
Partition/LV expanded
 ↓
Filesystem expanded
 ↓
New space available
```

Ext4 සඳහා:

```bash
resize2fs
```

XFS සඳහා:

```bash
xfs_growfs
```

වගේ tools භාවිතා කරනවා.

---

# 68. LVM සහ Filesystems

System Engineer කෙනෙක්ට Filesystem + LVM එක together තේරුම් ගන්න එක ගොඩක් වැදගත්.

සාමාන්‍ය structure:

```text
Physical Disk
      ↓
Partition
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

උදාහරණ:

```text
/dev/sdb
   ↓
PV
   ↓
VG: vg_data
   ↓
LV: lv_app
   ↓
XFS
   ↓
/data
```

---

# 69. Filesystem + LVM Practical Example

හිතන්න:

```text
/dev/sdb = 500 GB
```

LVM:

```text
/dev/sdb
    ↓
PV
    ↓
vg_data
    ↓
lv_app = 300G
    ↓
XFS
    ↓
/data
```

මේ architecture එක production Linux servers වල ඉතාම common.

---

# 70. Filesystem + RAID

තවත් layer එකක්:

```text
Physical Disks
       ↓
RAID
       ↓
Partition / LVM
       ↓
Filesystem
       ↓
Mount Point
```

උදාහරණ:

```text
Disk1 ─┐
Disk2 ─┼→ RAID
Disk3 ─┘
          ↓
        LVM
          ↓
       Filesystem
          ↓
         /data
```

RAID සහ filesystem එක එකම දෙයක් නෙවෙයි.

---

# 71. Disk Space Troubleshooting

Server එකේ:

```text
Disk Full
```

කියලා alert එකක් ආවා කියලා හිතන්න.

මුලින්:

```bash
df -h
```

බලන්න.

ඊට පස්සේ:

```bash
du -sh /var/*
```

බලන්න.

ඊට පස්සේ:

```bash
du -sh /var/log/*
```

බලන්න.

Docker server එකක් නම්:

```bash
docker system df
```

බලන්න.

inode issue එකක් සැක නම්:

```bash
df -i
```

---

# 72. Deleted File එකක් තිබුණත් Disk Full?

Linux වල interesting case එකක් තියෙනවා.

Application එකක් large log file එකක් open කරගෙන ඉන්නවා:

```text
app.log = 20 GB
```

ඒක delete කළා:

```bash
rm app.log
```

නමුත් application එක තවම file descriptor එක open කරගෙන ඉන්නවා.

එතකොට:

```bash
df -h
```

වල space free වෙලා නැතිව පේන්න පුළුවන්.

Check:

```bash
lsof +L1
```

මේකෙන් deleted-but-open files හොයාගන්න පුළුවන්.

System Engineer කෙනෙක්ට මේක **ඉතාම වැදගත් troubleshooting scenario එකක්**.

---

# 73. Filesystem Permissions

Filesystem එක file permissions manage කරනවා.

උදාහරණ:

```bash
ls -l file.txt
```

```text
-rw-r--r--
```

මේකට:

```text
Owner
Group
Others
```

permissions තියෙනවා.

ACL භාවිතා කළොත්:

```bash
getfacl file.txt
```

වඩා detailed permissions බලන්න පුළුවන්.

---

# 74. Filesystem Security Layers

Linux file access එක සරලව මෙහෙම හිතන්න:

```text
User
 ↓
UID/GID
 ↓
File Permissions
 ↓
ACL
 ↓
SELinux/AppArmor
 ↓
Filesystem
 ↓
Storage Device
```

මෙතන එක් එක් layer එකට වෙනම role එකක් තියෙනවා.

---

# 75. Filesystem එකක් කියන්නේ Disk එකමද?

**නැහැ.**

මේක වැදගත්.

```text
Disk ≠ Partition ≠ Filesystem ≠ Mount Point
```

උදාහරණයක්:

```text
/dev/sdb
   ↓
Disk

/dev/sdb1
   ↓
Partition

ext4
   ↓
Filesystem

/data
   ↓
Mount Point
```

---

# 76. මේ සම්පූර්ණ Flow එක හොඳට මතක තියාගන්න

```text
Physical Disk
      │
      ↓
Partition Table
      │
      ↓
Partition
      │
      ↓
Filesystem
      │
      ↓
Mount
      │
      ↓
Directory
      │
      ↓
Files
```

LVM තිබුණොත්:

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
      ↓
Files
```

---

# 77. System Engineer කෙනෙක්ට අනිවාර්ය Commands

මේ commands ටික practical එකේ හොඳට දැනගෙන ඉන්න:

```bash
lsblk
```

→ Block devices / partitions

```bash
blkid
```

→ UUID / filesystem type

```bash
df -h
```

→ Filesystem disk usage

```bash
df -Th
```

→ Filesystem type + usage

```bash
df -i
```

→ inode usage

```bash
du -sh /path
```

→ Directory usage

```bash
findmnt
```

→ Mount information

```bash
mount
```

→ Mounted filesystems

```bash
umount /data
```

→ Unmount

```bash
ls -i
```

→ inode numbers

```bash
lsof
```

→ Open files/processes

```bash
fsck
```

→ Filesystem check

```bash
xfs_repair
```

→ XFS repair

```bash
blkid
```

→ UUID + filesystem type

---

# 78. Filesystem ගැන අනිවාර්යයෙන්ම දැනගන්න Concepts

Linux System Engineer level එකට යනවා නම් මේ topics ටික cover කරන්න:

```text
Filesystem
│
├── Linux Filesystem Hierarchy
│
├── Partition
│
├── Mount / Unmount
│
├── Mount Points
│
├── /etc/fstab
│
├── UUID
│
├── ext4
├── XFS
├── Btrfs
├── ZFS
│
├── inode
├── Hard Link
├── Symbolic Link
│
├── Journaling
│
├── Filesystem Permissions
├── ACL
├── SELinux
│
├── Disk Usage
│   ├── df
│   └── du
│
├── Filesystem Check
│   ├── fsck
│   └── xfs_repair
│
├── Filesystem Resize
│
├── LVM
│
├── RAID
│
├── NFS
│
└── Virtual Filesystems
    ├── proc
    ├── sysfs
    ├── tmpfs
    └── devtmpfs
```

---

# ⭐ අවසාන Summary එක

Linux Filesystem එක තේරුම් ගන්න ප්‍රධාන concepts:

### 1. Disk

```text
/dev/sda
```

Physical/virtual storage device එක.

### 2. Partition

```text
/dev/sda1
/dev/sda2
```

Disk එකේ sections.

### 3. Filesystem

```text
ext4
XFS
Btrfs
```

Files organize කරන structure එක.

### 4. Mount Point

```text
/data
```

Filesystem එක Linux directory tree එකට attach කරන location එක.

### 5. inode

File metadata සහ data location information manage කරන structure එක.

### 6. `/etc/fstab`

Boot වෙද්දී filesystems automatically mount කිරීමට configuration.

### 7. `df`

```bash
df -h
```

Filesystem level disk usage.

### 8. `du`

```bash
du -sh /var/log
```

Specific directory/file එක කොච්චර space use කරනවද.

### 9. `lsblk`

```bash
lsblk
```

Disks / partitions / mount points.

### 10. `blkid`

```bash
blkid
```

UUID සහ filesystem type.

---

## 🔥 System Engineer කෙනෙක් විදිහට මේ Architecture එක memorize කරන්න

```text
                 Linux Storage Architecture

Physical Disk
      │
      ├── Partition Table
      │
      ↓
  Partition
      │
      │
      ├───────────────┐
      ↓               ↓
  Filesystem         LVM
  ext4/XFS            │
                      ↓
                     PV
                      ↓
                     VG
                      ↓
                     LV
                      ↓
                  Filesystem
                      │
                      ↓
                  Mount Point
                      │
                      ↓
                  Directory
                      │
                      ↓
                    Files
```

ඊට අමතරව filesystem එකේ files වලට:

```text
File
 │
 ├── inode
 ├── permissions
 ├── owner/group
 ├── ACL
 ├── timestamps
 └── data blocks
```

වගේ information manage වෙනවා.

=======

Linux වල **Filesystem** කියන්නේ files save කරන ක්‍රමයක් විතරක් නෙවෙයි. Disk එකේ තියෙන raw storage එක Linux OS එකට **files, directories, metadata, permissions, ownership, links, timestamps, free space** වගේ දේවල් manage කරන්න පුළුවන් විදිහට organize කරන system එක.

System Engineer කෙනෙක් විදිහට Linux Filesystems හොඳට දැනගන්න නම් මේ flow එක තේරුම් ගන්න එක වැදගත්:

```text
Physical Disk
     ↓
Partition
     ↓
Filesystem
     ↓
Mount Point
     ↓
Directory
     ↓
Files
```

ඒක එකින් එක බලමු.

---

# 1. Filesystem කියන්නේ මොකක්ද?

සරලව:

> **Filesystem එකක් කියන්නේ storage device එකක data organize, store, retrieve සහ manage කරන structure එක.**

උදාහරණයක්:

ඔයාට 1 TB HDD එකක් තියෙනවා.

Raw disk එක:

```text
/dev/sdb
```

මේක Linux එකට තියෙන raw block storage device එකක්.

Filesystem එකක් නැත්නම් Linux එකට මේකේ:

```text
file1.txt
photo.jpg
backup.zip
database.sql
```

වගේ files manage කරන්න පුළුවන් structured environment එකක් නැහැ.

Filesystem එකක් create කළාම:

```text
/dev/sdb
   ↓
Filesystem
   ↓
Mount
   ↓
/data
   ↓
files
```

වගේ use කරන්න පුළුවන්.

---

# 2. Filesystem එකක් අවශ්‍ය ඇයි?

Disk එකේ data bytes විතරයි තියෙන්නේ.

Filesystem එක Linux වලට කියනවා:

- මේ data එක file එකක්
    
- මේක directory එකක්
    
- මේ file එකේ owner කවුද
    
- permissions මොනවාද
    
- file size එක කීයද
    
- file එක create කළේ කවදාද
    
- data blocks කොහෙද තියෙන්නේ
    
- free space කොච්චරද
    
- deleted files වල blocks reuse කරන්න පුළුවන්ද
    

වගේ දේවල්.

ඒ නිසා:

```text
Disk
 ↓
Raw blocks
 ↓
Filesystem
 ↓
Files + Directories + Metadata
```

---

# 3. Linux Filesystem එකේ Basic Structure

Linux වල filesystem hierarchy එක root `/` වලින් පටන් ගන්නවා.

```text
/
├── bin
├── boot
├── dev
├── etc
├── home
├── lib
├── media
├── mnt
├── opt
├── proc
├── root
├── run
├── sbin
├── srv
├── sys
├── tmp
├── usr
└── var
```

මේක **Filesystem Hierarchy Standard (FHS)** concept එකට සම්බන්ධයි.

---

# 4. `/` — Root Filesystem

Linux filesystem එකේ top-level directory එක:

```text
/
```

මේකට:

> **Root directory**

කියනවා.

උදාහරණ:

```text
/etc
/home
/var
/usr
/opt
/tmp
```

මේ හැම එකක්ම `/` යටතේ තියෙනවා.

---

# 5. `/home`

සාමාන්‍ය users ලගේ home directories මෙහි තියෙනවා.

```text
/home
├── alice
├── bob
└── developer
```

උදාහරණ:

```text
/home/alice
```

Alice ගේ personal files:

```text
/home/alice/Documents
/home/alice/Downloads
/home/alice/scripts
```

---

# 6. `/root`

`root` userගේ home directory එක:

```text
/root
```

මේක:

```text
/
```

සහ:

```text
/root
```

එකම දෙයක් නෙවෙයි.

`/` = root filesystem directory

`/root` = root userගේ home directory.

---

# 7. `/etc`

System configuration files.

උදාහරණ:

```text
/etc/hosts
/etc/fstab
/etc/passwd
/etc/group
/etc/ssh/sshd_config
```

System administration වල `/etc` ඉතාම වැදගත්.

---

# 8. `/var`

Frequently changing data:

```text
/var/log
/var/cache
/var/lib
/var/tmp
```

උදාහරණ:

```text
/var/log/syslog
/var/log/messages
/var/lib/docker
```

Docker data, logs, package data වගේ දේවල් `/var` යටතේ තියෙන්න පුළුවන්.

---

# 9. `/var/log`

System/application logs.

උදාහරණ:

```text
/var/log/syslog
/var/log/messages
/var/log/auth.log
```

RHEL/Rocky/AlmaLinux:

```text
/var/log/secure
/var/log/messages
```

---

# 10. `/usr`

Installed applications, binaries, libraries, documentation වගේ system resources ගොඩක් මෙහි තියෙනවා.

උදාහරණ:

```text
/usr/bin
/usr/sbin
/usr/lib
/usr/share
```

---

# 11. `/bin`

Essential user commands සඳහා binaries.

Modern Linux distributions වල `/bin` බොහෝ විට `/usr/bin` වෙත symbolic link එකක් විය හැකියි.

---

# 12. `/sbin`

System administration binaries.

උදාහරණ:

```text
mount
fsck
ip
```

Modern distributions වල `/sbin` → `/usr/sbin` merge කරලා තියෙන්න පුළුවන්.

---

# 13. `/boot`

Boot files.

උදාහරණ:

```text
/boot/vmlinuz...
/boot/initramfs...
/boot/grub2
```

මෙහි Linux kernel සහ bootloader-related files තියෙන්න පුළුවන්.

---

# 14. `/dev`

Device files.

Linux වල:

> **Everything is treated as a file**

කියන concept එක තියෙනවා.

උදාහරණ:

```text
/dev/sda
/dev/sdb
/dev/nvme0n1
/dev/null
/dev/random
```

Disk එක:

```text
/dev/sda
```

Partition:

```text
/dev/sda1
/dev/sda2
```

---

# 15. `/proc`

`proc` කියන්නේ virtual filesystem එකක්.

මෙය disk එකේ normal files වගේ permanently store කරපු data නෙවෙයි.

Kernel එක runtime එකේ information provide කරනවා.

උදාහරණ:

```text
/proc/cpuinfo
/proc/meminfo
/proc/loadavg
/proc/uptime
```

බලන්න:

```bash
cat /proc/cpuinfo
```

---

# 16. `/sys`

`sysfs` කියන්නේ kernel සහ hardware/device information expose කරන virtual filesystem එකක්.

උදාහරණ:

```text
/sys/class
/sys/block
/sys/devices
```

Hardware/device management වලදී useful.

---

# 17. `/run`

Runtime data.

System boot එකෙන් පස්සේ runtime information සඳහා භාවිතා කරනවා.

උදාහරණ:

```text
/run
/run/lock
/run/user
```

---

# 18. `/tmp`

Temporary files.

```text
/tmp
```

Applications සහ users temporary data store කරන්න භාවිතා කරනවා.

⚠️ `/tmp` data permanent storage එකක් ලෙස use කරන්න එපා.

---

# 19. `/opt`

Optional/additional software සඳහා.

උදාහරණයක්:

```text
/opt/myapp
/opt/oracle
/opt/company-app
```

Third-party applications සඳහා common location එකක්.

---

# 20. `/mnt`

Temporary/manual mounts සඳහා historically common directory එක.

උදාහරණ:

```bash
mount /dev/sdb1 /mnt
```

---

# 21. `/media`

Removable media සඳහා.

උදාහරණ:

```text
/media/user/USB
```

USB/CD/DVD වගේ removable devices mount වෙන්න පුළුවන්.

---

# 22. `/srv`

Services විසින් serve කරන data සඳහා.

උදාහරණ:

```text
/srv/www
/srv/ftp
```

---

# 23. Linux Filesystem එකේ Physical Structure

දැන් actual storage side එක බලමු.

ඔයාට disk එකක් තියෙනවා:

```text
/dev/sda
```

ඒක partition කරලා:

```text
/dev/sda1
/dev/sda2
/dev/sda3
```

වගේ හදාගන්න පුළුවන්.

ඊට පස්සේ partition එකකට filesystem එකක් create කරනවා:

```text
/dev/sda1
     ↓
ext4
```

ඊට පස්සේ mount කරනවා:

```text
/dev/sda1
     ↓
/data
```

එතකොට:

```text
/data
```

directory එකෙන් disk එක access කරන්න පුළුවන්.

---

# 24. Partition සහ Filesystem අතර වෙනස

මේ දෙක confuse කරන්න එපා.

### Partition

Disk එක logical sections වලට divide කිරීම.

```text
/dev/sda
│
├── /dev/sda1
├── /dev/sda2
└── /dev/sda3
```

### Filesystem

Partition එක ඇතුළේ files organize කරන structure එක.

```text
/dev/sda1
     ↓
ext4
```

ඒ නිසා:

```text
Disk
 ↓
Partition
 ↓
Filesystem
 ↓
Mount Point
 ↓
Files
```

---

# 25. Filesystem Create කිරීම

උදාහරණයක්:

```bash
mkfs.ext4 /dev/sdb1
```

මේකෙන් `/dev/sdb1` මත ext4 filesystem එකක් create කරනවා.

⚠️ **මේ command එකේදී device එක වැරදිව select කළොත් data destroy වෙන්න පුළුවන්.**

Production server එකක `mkfs` run කිරීමට පෙර device එක verify කරන්න.

---

# 26. Filesystem Mount කිරීම

Filesystem එක create කළාට පස්සේ use කරන්න mount කරන්න ඕන.

Directory එකක් හදමු:

```bash
mkdir /data
```

Mount:

```bash
mount /dev/sdb1 /data
```

දැන්:

```text
/dev/sdb1
    ↓
/data
```

---

# 27. Mount කියන්නේ මොකක්ද?

Mount කියන්නේ:

> Filesystem එකක් Linux directory tree එකකට attach කිරීම.

උදාහරණ:

```text
/dev/sdb1
    │
    │ mount
    ↓
/data
```

දැන්:

```bash
cd /data
```

කරලා disk එකේ files access කරන්න පුළුවන්.

---

# 28. Mount Point

Filesystem එක attach කරන directory එක:

> **Mount Point**

කියනවා.

උදාහරණ:

```text
/dev/sdb1 → /data
```

මෙහි:

```text
/dev/sdb1 = Device
/data      = Mount Point
```

---

# 29. Mounted Filesystems බලන්න

```bash
mount
```

හෝ:

```bash
findmnt
```

හෝ:

```bash
df -h
```

`findmnt` command එක filesystem mount relationships තේරුම් ගන්න ඉතාම useful.

---

# 30. `df -h`

Disk usage බලන්න ප්‍රධාන command එක:

```bash
df -h
```

Output:

```text
Filesystem      Size  Used Avail Use% Mounted on
/dev/sda2        50G   30G   18G  63% /
/dev/sdb1       500G  200G  275G  43% /data
```

මෙහි:

|Column|Meaning|
|---|---|
|Filesystem|Device/filesystem|
|Size|Total size|
|Used|Used space|
|Avail|Available space|
|Use%|Usage percentage|
|Mounted on|Mount point|

---

# 31. `du`

`df` කියන්නේ filesystem level එකේ usage.

`du` කියන්නේ directory/file level එකේ usage.

```bash
du -sh /var/log
```

උදාහරණ:

```text
2.4G    /var/log
```

Top directories බලන්න:

```bash
du -sh /var/* | sort -h
```

මේක disk space troubleshooting වලදී ඉතාම useful.

---

# 32. `lsblk`

Block devices බලන්න:

```bash
lsblk
```

Example:

```text
NAME   SIZE TYPE MOUNTPOINT
sda    100G disk
├─sda1  1G  part /boot
└─sda2 99G  part /
sdb    500G disk
└─sdb1 500G part /data
```

මේකෙන්:

```text
Disk
 ↓
Partition
 ↓
Mount
```

structure එක ඉක්මනින් බලන්න පුළුවන්.

---

# 33. `blkid`

Filesystem type සහ UUID බලන්න:

```bash
blkid
```

Example:

```text
/dev/sda2: UUID="abc123" TYPE="ext4"
/dev/sdb1: UUID="xyz789" TYPE="xfs"
```

---

# 34. Filesystem Type බලන්නේ කොහොමද?

```bash
df -Th
```

Example:

```text
Filesystem     Type  Size  Used Avail Use% Mounted on
/dev/sda2      ext4   50G   30G   18G  63% /
/dev/sdb1      xfs   500G  200G  300G  40% /data
```

`T`:

> Filesystem type

`h`:

> Human-readable

---

# 35. Linux වල ප්‍රධාන Filesystem Types

Linux වල filesystem types ගොඩක් තියෙනවා.

වැදගත් ඒවා:

```text
ext4
XFS
Btrfs
ext3
ext2
ZFS
NFS
tmpfs
FAT32
exFAT
NTFS
```

Server administration වලදී විශේෂයෙන්:

```text
ext4
XFS
Btrfs
NFS
tmpfs
```

ගැන දැනගන්න වැදගත්.

---

# 36. ext4

**ext4 = Fourth Extended Filesystem**

Linux වල ඉතාම ජනප්‍රිය filesystem එකක්.

Features:

- Journaling
    
- Large filesystem support
    
- Large files
    
- Extents
    
- Good performance
    
- Reliable
    
- Mature
    

Ubuntu/Debian වගේ distributions වල බහුලව දකින්න පුළුවන්.

Check:

```bash
df -Th
```

---

# 37. ext3

ext2 වලට journaling එක add කරපු filesystem එක.

```text
ext2
 ↓
ext3
 ↓
ext4
```

ext3 දැන් modern systems වල ext4 තරම් common නෑ.

---

# 38. ext2

Older Linux filesystem එකක්.

ප්‍රධාන limitation එක:

> Journaling නැහැ.

ඒ නිසා modern production systems වල ext4/XFS වඩා common.

---

# 39. XFS

XFS කියන්නේ high-performance journaling filesystem එකක්.

RHEL ecosystem එකේ ඉතාම common:

```text
RHEL
Rocky Linux
AlmaLinux
```

වගේ systems වල.

XFS හොඳ වෙන්නේ:

- Large files
    
- Large filesystems
    
- High-performance workloads
    
- Server environments
    
- Parallel I/O
    

වගේ scenarios වල.

---

# 40. ext4 vs XFS

|Feature|ext4|XFS|
|---|---|---|
|Linux support|Excellent|Excellent|
|Journaling|Yes|Yes|
|General purpose|Excellent|Excellent|
|Large filesystem|Good|Excellent|
|RHEL environments|Common|Very common|
|Shrink filesystem|Supported offline|Not supported|
|Mature|Yes|Yes|

විශේෂයෙන් මතක තියාගන්න:

> **XFS filesystem එක shrink කරන්න බැහැ.**

XFS resize කිරීමේදී growth support තියෙනවා, නමුත් shrinking සඳහා recreate/migrate වගේ approach එකක් අවශ්‍ය වෙනවා.

---

# 41. Btrfs

Btrfs = **B-tree filesystem**

Modern advanced filesystem එකක්.

Features:

- Copy-on-write
    
- Snapshots
    
- Subvolumes
    
- Checksums
    
- Compression
    
- Send/receive
    
- Online resize
    

උදාහරණයක්:

```text
Filesystem
   ↓
Snapshot
   ↓
Backup / Rollback
```

Virtualization සහ snapshot-heavy environments වල useful.

---

# 42. ZFS

ZFS කියන්නේ advanced storage/filesystem technology එකක්.

ප්‍රධාන features:

- Storage pools
    
- RAID-like redundancy
    
- Checksums
    
- Snapshots
    
- Compression
    
- Data integrity
    
- Self-healing capabilities with redundancy
    
- Large storage management
    

Proxmox, NAS, storage environments වගේ places වල ZFS ගැන අනිවාර්යයෙන්ම අහන්න පුළුවන්.

---

# 43. NFS

NFS = **Network File System**

Remote server එකක filesystem එක network එක හරහා mount කරන්න.

උදාහරණ:

```text
Server A
/data
   ↓
Network
   ↓
Server B
/mnt/data
```

Server B එකෙන්:

```bash
mount serverA:/data /mnt/data
```

ඒකෙන් remote storage එක local directory එකක් වගේ access කරන්න පුළුවන්.

---

# 44. tmpfs

`tmpfs` RAM සහ swap backed temporary filesystem එකක්.

Check:

```bash
df -Th
```

`/run` වගේ locations වල tmpfs භාවිතා වෙනවා.

Characteristics:

- Fast
    
- Temporary
    
- Reboot එකෙන් data නැතිවෙන්න පුළුවන්
    
- Memory pressure එකට අනුව RAM/swap භාවිතා කළ හැක
    

---

# 45. Virtual Filesystems

Linux වල හැම filesystem එකක්ම physical disk එකක stored වෙලා නැහැ.

උදාහරණ:

```text
/proc
/sys
/dev
/run
```

මේවා kernel/system runtime information expose කරන virtual/pseudo filesystems.

ඒ නිසා:

```text
Filesystem ≠ Always physical disk
```

---

# 46. inode කියන්නේ මොකක්ද?

මේක Linux filesystem වල **ඉතාම වැදගත් concept එකක්**.

inode කියන්නේ file එකේ **metadata** store කරන data structure එකක්.

සාමාන්‍යයෙන් inode එකේ:

- File type
    
- Permissions
    
- Owner UID
    
- Group GID
    
- File size
    
- Timestamps
    
- Link count
    
- Data blocks වල locations
    

වගේ information තියෙනවා.

---

# 47. inode එකේ file name තියෙනවද?

**සාමාන්‍යයෙන් inode එකේ filename එක store වෙන්නේ නැහැ.**

Directory එක:

```text
filename → inode number
```

mapping එකක් maintain කරනවා.

Conceptually:

```text
Directory
│
├── report.txt → inode 12345
├── image.jpg  → inode 12346
└── app.log    → inode 12347
```

inode `12345` එකේ:

```text
permissions
owner
size
timestamps
data block pointers
```

වගේ metadata තියෙනවා.

---

# 48. inode Number බලන්නේ කොහොමද?

```bash
ls -i
```

Example:

```text
12345 report.txt
12346 image.jpg
```

inode number එක:

```text
12345
```

---

# 49. inode Exhaustion

Filesystem එකේ disk space තියෙනවා.

නමුත්:

```text
No space left on device
```

error එක එනවා.

ඒක inode exhaustion වෙන්න පුළුවන්.

Check:

```bash
df -i
```

Example:

```text
Filesystem      Inodes   IUsed   IFree IUse%
/dev/sda2      1000000 1000000       0  100%
```

Disk space:

```bash
df -h
```

100% නොවුණත් inode 100% වෙලා නම් new files create කරන්න බැරි වෙන්න පුළුවන්.

---

# 50. Hard Link

Hard link එක inode එක share කරනවා.

```text
file1
  ↓
inode 12345
  ↑
file2
```

Create:

```bash
ln file1 file2
```

Check:

```bash
ls -li
```

Files දෙකේ inode number එක same වෙයි.

---

# 51. Symbolic Link

Symbolic link එක වෙනත් path එකකට pointer එකක් වගේ.

```bash
ln -s /var/log/app.log /tmp/app.log
```

Concept:

```text
/tmp/app.log
      ↓
/var/log/app.log
```

Check:

```bash
ls -l
```

---

# 52. Hard Link vs Symbolic Link

|Feature|Hard Link|Symbolic Link|
|---|---|---|
|Same inode|Yes|No|
|Points to|inode|Path|
|Can cross filesystem|No|Yes|
|Directory hard links|Normally no|Yes|
|Original delete කළොත්|Data remains|Link breaks|

---

# 53. Filesystem Journaling

Modern filesystems like:

```text
ext4
XFS
```

journaling support කරනවා.

Journaling කියන්නේ filesystem changes ගැන journal/log එකක් maintain කිරීම.

Concept:

```text
Application
   ↓
Filesystem change
   ↓
Journal
   ↓
Actual metadata/data changes
```

Power failure/crash එකක් වුණාම filesystem recovery පහසු වෙනවා.

---

# 54. Why Journaling is Important?

හිතන්න server එකේ:

```text
File update වෙමින් තියෙනවා
       ↓
Power failure
       ↓
Server shutdown
```

Journaling filesystem එකට incomplete filesystem operations recover කරන්න mechanism එකක් තියෙනවා.

එය:

> Data backup එකක් නෙවෙයි.

Journaling කියන්නේ **filesystem consistency/recovery** සඳහා.

---

# 55. Mount Options

Filesystem එක mount කරනකොට options දෙන්න පුළුවන්.

Example:

```bash
mount -o ro /dev/sdb1 /data
```

`ro`:

> Read-only

`rw`:

> Read-write

තවත් options:

```text
noexec
nosuid
nodev
defaults
relatime
```

Security hardening වලදී මේවා වැදගත්.

---

# 56. `ro` — Read Only

```bash
mount -o ro /dev/sdb1 /data
```

User ට filesystem එකේ data read කරන්න පුළුවන්.

Write කරන්න බැහැ.

Useful:

- Investigation
    
- Recovery
    
- Forensics
    
- Protecting data
    

---

# 57. `noexec`

```text
noexec
```

mount option එකෙන් filesystem එකේ binaries execute කිරීම restrict කරන්න පුළුවන්.

Temporary/upload directories වගේ locations වල security hardening සඳහා use කරන්න පුළුවන්.

---

# 58. `nosuid`

`nosuid` option එකෙන් setuid/setgid behavior restrict කරන්න පුළුවන්.

Security hardening වලදී useful.

---

# 59. `nodev`

Filesystem එකේ device nodes interpret කිරීම prevent කිරීමට use කරන mount option එක.

Security hardening වලදී:

```text
/tmp
/data
```

වගේ mount points සඳහා context එක අනුව use කරන්න පුළුවන්.

---

# 60. `/etc/fstab`

Reboot කළාට පස්සේ manually:

```bash
mount /dev/sdb1 /data
```

run කරන්න ඕන නැතිව automatic mount කරන්න:

```text
/etc/fstab
```

භාවිතා කරනවා.

Example:

```text
UUID=abc123  /data  ext4  defaults  0  2
```

Meaning:

```text
UUID      → Filesystem identifier
/data     → Mount point
ext4      → Filesystem type
defaults  → Mount options
0         → dump
2         → fsck order
```

---

# 61. UUID කියන්නේ මොකක්ද?

Filesystem එකකට unique identifier එකක්.

බලන්න:

```bash
blkid
```

Example:

```text
/dev/sdb1: UUID="abc123-def456" TYPE="ext4"
```

`/etc/fstab` එකේ:

```text
UUID=abc123-def456 /data ext4 defaults 0 2
```

භාවිතා කරන එක:

```text
/dev/sdb1
```

වලට වඩා stable වෙන්න පුළුවන්.

---

# 62. `/etc/fstab` වෙනස් කළාට පස්සේ

Reboot කරන්න කලින් test කරන්න:

```bash
mount -a
```

මේකෙන් `/etc/fstab` entries mount කරන්න try කරනවා.

⚠️ Production server එකක `fstab` වැරදියට edit කළොත් boot problems ඇතිවෙන්න පුළුවන්. ඒ නිසා:

```bash
mount -a
```

run කරලා errors නැද්ද බලන්න.

---

# 63. Unmount

Filesystem එක unmount කරන්න:

```bash
umount /data
```

හෝ:

```bash
umount /dev/sdb1
```

Unmount වෙන්නේ නැත්නම්:

```text
target is busy
```

වගේ error එකක් එන්න පුළුවන්.

---

# 64. "Target is busy" Troubleshooting

මුලින් බලන්න:

```bash
lsof /data
```

හෝ:

```bash
fuser -vm /data
```

උදාහරණ:

```text
PID
USER
COMMAND
```

වගේ information ලැබෙනවා.

හේතුව:

- User එක `/data` directory එකේ
    
- Application එක file එක use කරනවා
    
- Process එක directory එක access කරනවා
    

වෙන්න පුළුවන්.

---

# 65. Filesystem Check — `fsck`

Filesystem errors check/repair කරන්න:

```bash
fsck
```

Ext4 සඳහා:

```bash
fsck.ext4 /dev/sdb1
```

හෝ:

```bash
e2fsck /dev/sdb1
```

⚠️ **Mounted, active filesystem එකකට blindly `fsck` run කරන්න එපා.**

සාමාන්‍යයෙන් filesystem එක unmounted වෙලා තියෙන්න ඕන.

---

# 66. XFS Filesystem Check

XFS සඳහා:

```bash
xfs_repair /dev/sdb1
```

XFS-specific tools භාවිතා කරනවා.

---

# 67. Filesystem Resize

Disk එක:

```text
100 GB
```

වෙලා පස්සේ:

```text
200 GB
```

කරලා expand කළා කියලා හිතන්න.

Filesystem එක automatically 200 GB use කරනවා කියලා assume කරන්න බැහැ.

Flow එක:

```text
Disk
 ↓
Partition/LV expanded
 ↓
Filesystem expanded
 ↓
New space available
```

Ext4 සඳහා:

```bash
resize2fs
```

XFS සඳහා:

```bash
xfs_growfs
```

වගේ tools භාවිතා කරනවා.

---

# 68. LVM සහ Filesystems

System Engineer කෙනෙක්ට Filesystem + LVM එක together තේරුම් ගන්න එක ගොඩක් වැදගත්.

සාමාන්‍ය structure:

```text
Physical Disk
      ↓
Partition
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

උදාහරණ:

```text
/dev/sdb
   ↓
PV
   ↓
VG: vg_data
   ↓
LV: lv_app
   ↓
XFS
   ↓
/data
```

---

# 69. Filesystem + LVM Practical Example

හිතන්න:

```text
/dev/sdb = 500 GB
```

LVM:

```text
/dev/sdb
    ↓
PV
    ↓
vg_data
    ↓
lv_app = 300G
    ↓
XFS
    ↓
/data
```

මේ architecture එක production Linux servers වල ඉතාම common.

---

# 70. Filesystem + RAID

තවත් layer එකක්:

```text
Physical Disks
       ↓
RAID
       ↓
Partition / LVM
       ↓
Filesystem
       ↓
Mount Point
```

උදාහරණ:

```text
Disk1 ─┐
Disk2 ─┼→ RAID
Disk3 ─┘
          ↓
        LVM
          ↓
       Filesystem
          ↓
         /data
```

RAID සහ filesystem එක එකම දෙයක් නෙවෙයි.

---

# 71. Disk Space Troubleshooting

Server එකේ:

```text
Disk Full
```

කියලා alert එකක් ආවා කියලා හිතන්න.

මුලින්:

```bash
df -h
```

බලන්න.

ඊට පස්සේ:

```bash
du -sh /var/*
```

බලන්න.

ඊට පස්සේ:

```bash
du -sh /var/log/*
```

බලන්න.

Docker server එකක් නම්:

```bash
docker system df
```

බලන්න.

inode issue එකක් සැක නම්:

```bash
df -i
```

---

# 72. Deleted File එකක් තිබුණත් Disk Full?

Linux වල interesting case එකක් තියෙනවා.

Application එකක් large log file එකක් open කරගෙන ඉන්නවා:

```text
app.log = 20 GB
```

ඒක delete කළා:

```bash
rm app.log
```

නමුත් application එක තවම file descriptor එක open කරගෙන ඉන්නවා.

එතකොට:

```bash
df -h
```

වල space free වෙලා නැතිව පේන්න පුළුවන්.

Check:

```bash
lsof +L1
```

මේකෙන් deleted-but-open files හොයාගන්න පුළුවන්.

System Engineer කෙනෙක්ට මේක **ඉතාම වැදගත් troubleshooting scenario එකක්**.

---

# 73. Filesystem Permissions

Filesystem එක file permissions manage කරනවා.

උදාහරණ:

```bash
ls -l file.txt
```

```text
-rw-r--r--
```

මේකට:

```text
Owner
Group
Others
```

permissions තියෙනවා.

ACL භාවිතා කළොත්:

```bash
getfacl file.txt
```

වඩා detailed permissions බලන්න පුළුවන්.

---

# 74. Filesystem Security Layers

Linux file access එක සරලව මෙහෙම හිතන්න:

```text
User
 ↓
UID/GID
 ↓
File Permissions
 ↓
ACL
 ↓
SELinux/AppArmor
 ↓
Filesystem
 ↓
Storage Device
```

මෙතන එක් එක් layer එකට වෙනම role එකක් තියෙනවා.

---

# 75. Filesystem එකක් කියන්නේ Disk එකමද?

**නැහැ.**

මේක වැදගත්.

```text
Disk ≠ Partition ≠ Filesystem ≠ Mount Point
```

උදාහරණයක්:

```text
/dev/sdb
   ↓
Disk

/dev/sdb1
   ↓
Partition

ext4
   ↓
Filesystem

/data
   ↓
Mount Point
```

---

# 76. මේ සම්පූර්ණ Flow එක හොඳට මතක තියාගන්න

```text
Physical Disk
      │
      ↓
Partition Table
      │
      ↓
Partition
      │
      ↓
Filesystem
      │
      ↓
Mount
      │
      ↓
Directory
      │
      ↓
Files
```

LVM තිබුණොත්:

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
      ↓
Files
```

---

# 77. System Engineer කෙනෙක්ට අනිවාර්ය Commands

මේ commands ටික practical එකේ හොඳට දැනගෙන ඉන්න:

```bash
lsblk
```

→ Block devices / partitions

```bash
blkid
```

→ UUID / filesystem type

```bash
df -h
```

→ Filesystem disk usage

```bash
df -Th
```

→ Filesystem type + usage

```bash
df -i
```

→ inode usage

```bash
du -sh /path
```

→ Directory usage

```bash
findmnt
```

→ Mount information

```bash
mount
```

→ Mounted filesystems

```bash
umount /data
```

→ Unmount

```bash
ls -i
```

→ inode numbers

```bash
lsof
```

→ Open files/processes

```bash
fsck
```

→ Filesystem check

```bash
xfs_repair
```

→ XFS repair

```bash
blkid
```

→ UUID + filesystem type

---

# 78. Filesystem ගැන අනිවාර්යයෙන්ම දැනගන්න Concepts

Linux System Engineer level එකට යනවා නම් මේ topics ටික cover කරන්න:

```text
Filesystem
│
├── Linux Filesystem Hierarchy
│
├── Partition
│
├── Mount / Unmount
│
├── Mount Points
│
├── /etc/fstab
│
├── UUID
│
├── ext4
├── XFS
├── Btrfs
├── ZFS
│
├── inode
├── Hard Link
├── Symbolic Link
│
├── Journaling
│
├── Filesystem Permissions
├── ACL
├── SELinux
│
├── Disk Usage
│   ├── df
│   └── du
│
├── Filesystem Check
│   ├── fsck
│   └── xfs_repair
│
├── Filesystem Resize
│
├── LVM
│
├── RAID
│
├── NFS
│
└── Virtual Filesystems
    ├── proc
    ├── sysfs
    ├── tmpfs
    └── devtmpfs
```

---

# ⭐ අවසාන Summary එක

Linux Filesystem එක තේරුම් ගන්න ප්‍රධාන concepts:

### 1. Disk

```text
/dev/sda
```

Physical/virtual storage device එක.

### 2. Partition

```text
/dev/sda1
/dev/sda2
```

Disk එකේ sections.

### 3. Filesystem

```text
ext4
XFS
Btrfs
```

Files organize කරන structure එක.

### 4. Mount Point

```text
/data
```

Filesystem එක Linux directory tree එකට attach කරන location එක.

### 5. inode

File metadata සහ data location information manage කරන structure එක.

### 6. `/etc/fstab`

Boot වෙද්දී filesystems automatically mount කිරීමට configuration.

### 7. `df`

```bash
df -h
```

Filesystem level disk usage.

### 8. `du`

```bash
du -sh /var/log
```

Specific directory/file එක කොච්චර space use කරනවද.

### 9. `lsblk`

```bash
lsblk
```

Disks / partitions / mount points.

### 10. `blkid`

```bash
blkid
```

UUID සහ filesystem type.

---

## 🔥 System Engineer කෙනෙක් විදිහට මේ Architecture එක memorize කරන්න

```text
                 Linux Storage Architecture

Physical Disk
      │
      ├── Partition Table
      │
      ↓
  Partition
      │
      │
      ├───────────────┐
      ↓               ↓
  Filesystem         LVM
  ext4/XFS            │
                      ↓
                     PV
                      ↓
                     VG
                      ↓
                     LV
                      ↓
                  Filesystem
                      │
                      ↓
                  Mount Point
                      │
                      ↓
                  Directory
                      │
                      ↓
                    Files
```

ඊට අමතරව filesystem එකේ files වලට:

```text
File
 │
 ├── inode
 ├── permissions
 ├── owner/group
 ├── ACL
 ├── timestamps
 └── data blocks
```

වගේ information manage වෙනවා.

>>>>>>> a159bce7962d6af920654f254fefef8e27f23cd4
**ඔයාට Linux System Engineer level එකට මේ topic එක master කරන්න නම් ඊළඟ practical step එක `Disk → Partition → Filesystem → Mount → /etc/fstab → LVM → Resize → Troubleshooting` කියන flow එක actual Linux commands වලින් practice කිරීමයි.**