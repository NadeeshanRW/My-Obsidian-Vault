හරි. මේ diagram එක **Debian/Ubuntu Linux system එකේ components එකිනෙකට සම්බන්ධ වෙන්නේ කොහොමද** කියලා පෙන්වන simplified view එකක්.

```text
Linux Kernel
    │
    ├── systemd
    │
    ├── APT
    │
    ├── DPKG
    │
    ├── GNU tools
    │
    └── Debian packages
          └── .deb
```

හැබැයි මෙතන **වැදගත් correction එකක්** තියෙනවා: `systemd`, `APT`, `dpkg`, GNU tools කියන්නේ **Kernel එකේ children/direct parts නෙවෙයි**. ඒවා Kernel එක උඩින් run වෙන userspace components/tools. ඒක තේරුම් ගත්තොත් Linux architecture එක ලේසියෙන් තේරෙනවා.

## 1. Linux Kernel 🧠

**Kernel කියන්නේ Linux system එකේ core එක.**

Hardware එකත් software එකත් අතර main bridge එක වගේ වැඩ කරනවා.

```text
Applications
     ↓
Userspace
     ↓
Linux Kernel
     ↓
Hardware
```

Kernel එක handle කරන දේවල්:

- CPU
    
- RAM / Memory
    
- Disk / Storage
    
- Network
    
- Processes
    
- Devices
    
- System calls
    
- Security permissions
    

උදාහරණයක්:

ඔයා command එකක් දුන්නා:

```bash
cat file.txt
```

`cat` program එකට disk එකෙන් data directly hardware-level access කරන්න බැහැ. Kernel එක හරහා file system / storage access කරනවා.

---

# 2. systemd ⚙️

`systemd` කියන්නේ Linux **userspace system/service manager** එකක්.

Linux boot වෙනකොට system එකේ services start කිරීම වගේ වැඩ ගොඩක් manage කරනවා.

```text
BIOS/UEFI
   ↓
Bootloader
   ↓
Linux Kernel
   ↓
systemd
   ↓
Services
```

උදාහරණ:

```bash
systemctl status nginx
```

මේකෙන් `nginx` service එකේ status එක බලනවා.

```bash
systemctl start nginx
systemctl stop nginx
systemctl restart nginx
```

ඒ නිසා:

> **Kernel = core resource manager**  
> **systemd = system/service manager**

---

# 3. APT 📦

`APT` = **Advanced Package Tool**

Ubuntu/Debian වල software packages install/update/remove කරන්න භාවිතා කරන **high-level package management tool** එක.

උදාහරණ:

```bash
sudo apt update
```

Repository එකෙන් available package information update කරනවා.

```bash
sudo apt install nginx
```

Nginx install කරනවා.

APT එකේ වැදගත් feature එකක් තමයි **dependencies handle කිරීම**.

උදාහරණයක්:

```text
nginx
 ├── dependency A
 ├── dependency B
 └── dependency C
```

ඔයා:

```bash
apt install nginx
```

කළොත් APT අවශ්‍ය dependencies හඳුනාගෙන install කරනවා.

---

# 4. DPKG

`dpkg` = **Debian Package Manager**

මේක APT වලට වඩා **lower-level package management tool** එකක්.

විශේෂයෙන් `.deb` packages directly manage කරනවා.

උදාහරණයක්:

```bash
sudo dpkg -i nginx.deb
```

මෙතන `.deb` file එක directly `dpkg` හරහා install කරනවා.

Package එකේ information බලන්න:

```bash
dpkg -l
```

---

# 5. APT සහ DPKG අතර relationship එක

මේක තමයි **මතක තියාගන්න ඕනේ main concept එක**.

```text
        APT
         │
         │ uses
         ↓
       DPKG
         │
         ↓
      .deb
```

### `apt`

High-level:

```bash
sudo apt install nginx
```

APT:

1. Repository එක check කරනවා
    
2. Package එක හොයනවා
    
3. Dependencies හොයනවා
    
4. Packages download කරනවා
    
5. `dpkg` හරහා packages install කරනවා
    

### `dpkg`

Low-level:

```bash
sudo dpkg -i nginx.deb
```

එක `.deb` package එක direct install/manage කරනවා.

---

# 6. GNU Tools 🛠️

GNU tools කියන්නේ Linux system එකක frequently භාවිතා කරන command-line utilities ගොඩක්.

උදාහරණ:

```bash
ls
cp
mv
rm
cat
grep
find
chmod
chown
```

උදාහරණයක්:

```bash
ls -la
```

මෙතන `ls` කියන්නේ GNU core utilities වලින් එකක්.

තවත්:

```bash
grep "error" app.log
```

`grep` text search කරන්න භාවිතා කරන tool එකක්.

**Important:** GNU සහ Linux එක එක දෙයක් නෙවෙයි.

```text
Linux
  = Kernel

GNU
  = Many user-space tools
```

ඒ නිසා "Linux" කියන complete operating environment එකේ Linux kernel + GNU tools + other userspace components තියෙන්න පුළුවන්.

---

# 7. Debian Packages 📦

Ubuntu/Debian systems වල software distribution කරන standard package format එක තමයි `.deb`.

උදාහරණ:

```text
nginx_1.xx.x_amd64.deb
```

`.deb` package එක ඇතුළේ සාමාන්‍යයෙන් තියෙන්නේ:

```text
.deb
 ├── Program files
 ├── Configuration files
 ├── Metadata
 ├── Dependencies information
 └── Installation scripts
```

ඒක basically **software එක install කරන්න අවශ්‍ය package එකක්**.

---

# 8. මේ ඔක්කොම එකට connect කරලා බලමු

ඔයා Ubuntu server එකකට Nginx install කරනවා කියලා හිතන්න.

ඔයා command එක දෙනවා:

```bash
sudo apt install nginx
```

### Step 1 — APT

APT repository එකෙන් Nginx package එක හොයනවා.

```text
APT
 ↓
Repository
 ↓
nginx package
```

### Step 2 — `.deb`

APT `.deb` package එක download කරනවා.

```text
nginx
   ↓
nginx.deb
```

### Step 3 — dpkg

APT package installation එක සඳහා `dpkg` භාවිතා කරනවා.

```text
APT
 ↓
DPKG
 ↓
nginx.deb
 ↓
Installed files
```

### Step 4 — systemd

Nginx install වුණාට පස්සේ service එක `systemd` හරහා manage කරන්න පුළුවන්.

```bash
sudo systemctl start nginx
```

```text
systemd
   ↓
nginx service
   ↓
nginx process
   ↓
Linux Kernel
   ↓
CPU / RAM / Network
```

---

# 🧠 Complete picture

මේක තමයි ඔයාට මතක තියාගන්න හොඳම diagram එක:

```text
                    USER
                     │
                     ↓
              Linux Commands
             ls / grep / cp / etc.
                     │
                     ↓
                 USERSPACE
        ┌────────────┼─────────────┐
        │            │             │
      systemd       APT        GNU Tools
        │            │
        │          DPKG
        │            │
        │          .deb
        │
        └────────────┬─────────────┘
                     ↓
                Linux Kernel
                     │
        ┌────────────┼────────────┐
        ↓            ↓            ↓
       CPU          RAM        Storage
                                  │
                               Network
```

### 🔑 එක වාක්‍යයෙන්

**Linux Kernel** → Hardware/resources control කරන core එක.

**systemd** → Services සහ system lifecycle manage කරන එක.

**APT** → Software packages install/update/manage කරන high-level tool එක.

**dpkg** → `.deb` packages low-level එකෙන් install/manage කරන tool එක.

**GNU tools** → Linux userspace එකේ daily commands/utilities.

**`.deb`** → Debian/Ubuntu software package file format එක.

ඔයා Linux Administration ඉගෙන ගන්නවා නම්, ඊළඟට **`apt → dpkg → systemd → process → kernel → hardware`** කියන chain එක practically commands එක්ක ඉගෙන ගන්න එක තමයි හොඳම way එක.