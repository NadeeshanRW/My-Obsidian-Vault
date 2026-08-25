
**ACL (Access Control List)** කියන්නේ Linux වල තියෙන **advanced permission management mechanism** එකක්. සාමාන්‍ය Linux permissions වල තියෙන **Owner / Group / Others** කියන තුනට වඩා වැඩි control එකක් ACL මගින් ලබාගන්න පුළුවන්.

System Engineer කෙනෙක් විදිහට ACL වැදගත් වෙන්නේ:

- Shared directories
    
- Application servers
    
- Web servers
    
- File servers
    
- Deployment servers
    
- Multi-user environments
    
- Production servers
    

වගේ environments වලදී.

---

# 1. මුලින්ම සාමාන්‍ය Linux Permissions තේරුම් ගන්න

සාමාන්‍යයෙන් Linux file එකකට permissions categories 3ක් තියෙනවා:

```text
             File
              │
      ┌───────┼────────┐
      │       │        │
    Owner    Group   Others
      │       │        │
     rwx     rwx      rwx
```

උදාහරණයක්:

```bash
ls -l report.txt
```

Output:

```text
-rw-r----- 1 alice developers 2048 Aug 24 report.txt
```

මෙහි permissions:

```text
-rw-r-----
 │ │   │
 │ │   └── Others: r--
 │ └────── Group:  r--
 └──────── Owner: rw-
```

ඒ කියන්නේ:

|User type|Permission|
|---|---|
|Owner (`alice`)|`rw-`|
|Group (`developers`)|`r--`|
|Others|`---`|

නමුත් මෙහි limitation එකක් තියෙනවා.

සාමාන්‍ය Linux permission system එකෙන් අපිට **එක owner කෙනෙක් සහ එක group එකක්** තමයි define කරන්න පුළුවන්.

---

# 2. ACL කියන්නේ මොකක්ද?

ACL කියන්නේ:

> **Access Control List**

එමගින් file එකකට හෝ directory එකකට **විශේෂ user කෙනෙක්ට හෝ group එකකට වෙනම permissions** දෙන්න පුළුවන්.

උදාහරණයක්:

```text
File
 │
 ├── Owner → rwx
 ├── Group → rwx
 ├── Others → ---
 │
 ├── User Alice → rwx
 ├── User Bob → r--
 └── Group testers → r-x
```

ඒ කියන්නේ සාමාන්‍ය:

```text
Owner
Group
Others
```

වලට අමතරව:

```text
Alice
Bob
Testers group
```

වගේ specific users/groups වලටත් permissions දෙන්න පුළුවන්.

---

# 3. ACL අවශ්‍ය වෙන්නේ ඇයි?

හිතන්න:

```text
/project
```

කියලා directory එකක් තියෙනවා.

Users:

```text
Alice
Bob
Charlie
```

ඔයාට ඕන:

```text
Alice   → read + write
Bob     → read only
Charlie → no access
```

සාමාන්‍ය Linux permissions වලින් මේක පහසුවෙන් කරන්න බැහැ.

ACL භාවිතා කරලා:

```text
Alice   → rw-
Bob     → r--
Charlie → ---
```

වගේ permissions වෙන වෙනම දෙන්න පුළුවන්.

---

# 4. Traditional Permissions vs ACL

### Traditional Linux Permissions

ප්‍රධාන categories:

```text
Owner
Group
Others
```

එනම්:

```text
u
g
o
```

---

### ACL

ACL එකක මෙවැනි entries තියෙන්න පුළුවන්:

```text
user::rwx
user:alice:rwx
user:bob:r--
group::r-x
group:developers:rwx
mask::rwx
other::---
```

ඒ නිසා ACL එක traditional permissions වලට වඩා **fine-grained**.

---

# 5. ACL වල වැදගත් Terms

## 5.1 Owner Entry

```text
user::rwx
```

මේක file එකේ owner represent කරනවා.

---

## 5.2 Named User

```text
user:alice:rwx
```

මේකෙන් Alice ට specific permission එකක් ලබා දෙනවා.

---

## 5.3 Owning Group

```text
group::r-x
```

මේක file එකේ owning group එක represent කරනවා.

---

## 5.4 Named Group

```text
group:developers:rwx
```

මෙයින් `developers` group එකට specific permission එකක් ලබා දෙනවා.

---

## 5.5 ACL Mask

```text
mask::rwx
```

මේක ACL වල ඉතාම වැදගත් concept එකක්.

ACL mask එකෙන් පහත entries වල **maximum effective permissions** එක control කරනවා:

```text
Named users
Named groups
Owning group
```

මේක පස්සේ detail එකෙන් බලමු.

---

## 5.6 Other

```text
other::---
```

කිසිම ACL entry එකකට match නොවන අනෙක් users මෙයට අයත් වෙනවා.

---

# 6. ACL එක බලන්නේ කොහොමද?

මේකට:

```bash
getfacl filename
```

භාවිතා කරනවා.

උදාහරණයක්:

```bash
getfacl report.txt
```

Output:

```text
# file: report.txt
# owner: alice
# group: developers
user::rw-
user:bob:r--
group::r--
mask::r--
other::---
```

මෙය තේරුම් ගමු.

```text
user::rw-
```

Owner ට:

```text
rw-
```

permission තියෙනවා.

---

```text
user:bob:r--
```

Bob ට:

```text
r--
```

permission තියෙනවා.

---

```text
group::r--
```

Owning group එකට:

```text
r--
```

තියෙනවා.

---

```text
mask::r--
```

Maximum effective permission:

```text
r--
```

---

```text
other::---
```

අනෙක් users:

```text
---
```

---

# 7. `setfacl`

ACL create/modify කරන්න ප්‍රධාන command එක:

```bash
setfacl
```

Basic syntax:

```bash
setfacl [options] permissions file
```

---

# 8. User කෙනෙක්ට Read Permission දෙන්නේ කොහොමද?

හිතන්න:

```text
File = report.txt
User = bob
```

Bob ට read permission දෙන්න:

```bash
setfacl -m u:bob:r report.txt
```

මෙහි:

```text
-m
```

කියන්නේ:

> ACL එක modify කරන්න.

```text
u:bob:r
```

කියන්නේ:

```text
u    → user
bob  → username
r    → read
```

ඊට පස්සේ:

```bash
getfacl report.txt
```

කලාම:

```text
user::rw-
user:bob:r--
group::r--
mask::r--
other::---
```

වගේ result එකක් ලැබෙයි.

---

# 9. User කෙනෙක්ට Read + Write දෙන්න

```bash
setfacl -m u:bob:rw report.txt
```

දැන් Bob ට:

```text
Read
Write
```

කරන්න පුළුවන්.

Execute permission නැහැ.

---

# 10. User කෙනෙක්ට Full Permission දෙන්න

```bash
setfacl -m u:bob:rwx report.txt
```

Bob ට:

```text
Read
Write
Execute
```

permissions ලැබෙනවා.

---

# 11. User කෙනෙක්ගේ ACL Permission Remove කිරීම

Bob ට ACL එකක් දීලා තියෙනවා කියලා හිතමු:

```bash
setfacl -m u:bob:rw report.txt
```

ඒක remove කරන්න:

```bash
setfacl -x u:bob report.txt
```

මෙහි:

```text
-x
```

කියන්නේ ACL entry එක remove කිරීම.

---

# 12. Group එකකට ACL Permission දෙන්නේ කොහොමද?

හිතන්න group එක:

```text
developers
```

developers group එකට read/write දෙන්න:

```bash
setfacl -m g:developers:rw report.txt
```

මෙහි:

```text
g       → group
developers → group name
rw      → read + write
```

---

# 13. Group ACL එක Remove කිරීම

```bash
setfacl -x g:developers report.txt
```

---

# 14. Directory එකකට ACL

ACL directories වලදී ඉතාමත් useful.

උදාහරණයක්:

```text
/project
```

Alice ට full access දෙන්න:

```bash
setfacl -m u:alice:rwx /project
```

Alice ට directory එක:

```text
enter
access
create
delete
modify
```

වගේ operations කරන්න පුළුවන්, directory සහ ඇතුළත files වල permissions අනුව.

---

# 15. Directory එකක `x` Permission එක වැදගත් ඇයි?

මේක ACL ඉගෙන ගන්නකොට **අනිවාර්යයෙන්ම තේරුම් ගන්න ඕන concept එකක්.**

File එකකට:

```text
r = read file
w = modify file
x = execute file
```

නමුත් Directory එකකට:

```text
r = directory contents list කරන්න
w = files create/delete/rename කරන්න
x = directory එකට enter/traverse කරන්න
```

උදාහරණයක්:

```bash
setfacl -m u:bob:r /project
```

Bob ට directory listing එක බලන්න පුළුවන් වුණත් `x` නැති නිසා directory එක හරහා files access කරන්න problem ඇතිවෙන්න පුළුවන්.

සාමාන්‍යයෙන් directory access සඳහා:

```text
r-x
```

භාවිතා කරනවා.

උදාහරණය:

```bash
setfacl -m u:bob:rx /project
```

---

# 16. ACL Inheritance

දැන් වැදගත් concept එකක්.

හිතන්න:

```text
/project
```

ඇතුළේ:

```text
app
config
logs
uploads
```

තියෙනවා.

ඔයා Alice ට:

```text
rwx
```

permission දුන්නා.

නමුත් `/project` ඇතුළේ අලුතින් file එකක් create කළාම ඒ file එකටත් Alice ට permission automatically ලැබෙන්න ඕන නම් **Default ACL** භාවිතා කරනවා.

---

# 17. Default ACL

Default ACL කියන්නේ:

> Directory එකක් ඇතුළේ අනාගතයේ create කරන files/directories වලට ACL inheritance ලබාදීම.

උදාහරණය:

```bash
setfacl -d -m u:alice:rwx /project
```

මෙහි:

```text
-d
```

කියන්නේ:

> Default ACL

දැන්:

```bash
getfacl /project
```

කලාම මෙවැනි output එකක් ලැබෙන්න පුළුවන්:

```text
user::rwx
group::r-x
other::---
default:user::rwx
default:user:alice:rwx
default:group::r-x
default:mask::rwx
default:other::---
```

---

# 18. Access ACL vs Default ACL

මේ දෙක confuse කරගන්න එපා.

### Access ACL

දැනට තියෙන file/directory එක control කරනවා.

```bash
setfacl -m u:alice:rwx /project
```

එනම්:

> දැනට `/project` එකට Alice ට permission දෙන්න.

---

### Default ACL

අලුතින් create කරන objects වලට inheritance ලබාදෙනවා.

```bash
setfacl -d -m u:alice:rwx /project
```

එනම්:

> `/project` ඇතුළේ අලුතින් create කරන files/directories වලටත් Alice ට permission ලබාදෙන්න.

මතක තියාගන්න:

```text
Access ACL
    ↓
Current object


Default ACL
    ↓
Future children
```

---

# 19. ACL Mask — ඉතාම වැදගත්

හිතන්න:

```bash
setfacl -m u:bob:rwx report.txt
```

ඒත් `getfacl` output එකේ:

```text
user:bob:rwx
mask::r--
```

කියලා තියෙනවා.

එතකොට Bob ගේ **effective permission** එක:

```text
r--
```

වෙයි.

`rwx` නෙවෙයි.

Concept එක:

```text
Requested ACL permission
          +
ACL Mask
          =
Effective permission
```

උදාහරණය:

```text
Bob requested → rwx
Mask          → r--
-----------------
Effective     → r--
```

---

# 20. ACL Mask එකේ Purpose එක

ACL mask එකෙන් maximum permission එක define කරනවා:

```text
Named users
Named groups
Owning group
```

සඳහා.

උදාහරණයක්:

```text
user::rwx
user:bob:rwx
group::r-x
group:developers:rwx
mask::r--
other::---
```

Effective permissions:

```text
Owner          → rwx
Bob            → r--
Developers     → r--
Owning group   → r--
Others         → ---
```

හේතුව:

```text
mask::r--
```

නිසා.

---

# 21. ACL Mask එක වෙනස් කිරීම

Mask එක:

```text
rwx
```

කරන්න:

```bash
setfacl -m m:rwx report.txt
```

හෝ:

```bash
setfacl -m mask:rwx report.txt
```

ඊට පස්සේ:

```bash
getfacl report.txt
```

කරන්න.

දැන් Bob ට තිබුණු:

```text
user:bob:rwx
```

permission එක effective වෙන්න පුළුවන්.

---

# 22. ACL Permission Notation

ACL වලත් සාමාන්‍ය Linux permissions වගේ:

```text
r
w
x
```

භාවිතා කරනවා.

උදාහරණ:

```text
r--
-w-
--x
rw-
r-x
-wx
rwx
---
```

---

# 23. Numeric ACL Permissions

Numeric values භාවිතා කරන්නත් පුළුවන්:

```text
4 = read
2 = write
1 = execute
```

ඒ අනුව:

```text
7 = rwx
6 = rw-
5 = r-x
4 = r--
3 = -wx
2 = -w-
1 = --x
0 = ---
```

උදාහරණය:

```bash
setfacl -m u:bob:7 file.txt
```

එනම්:

```text
Bob = rwx
```

නමුත් ACL වලදී මම recommend කරන්නේ symbolic format එක:

```bash
setfacl -m u:bob:rwx file.txt
```

භාවිතා කරන එක.

ඒක තේරුම් ගන්න ලේසියි.

---

# 24. ACL Entries සියල්ල Remove කිරීම

Extended ACL entries remove කරන්න:

```bash
setfacl -b file.txt
```

`-b` කියන්නේ:

> Extended ACL entries සියල්ල remove කරන්න.

උදාහරණයක්:

Before:

```text
user::rw-
user:bob:rw-
group::r--
group:developers:rwx
mask::rwx
other::---
```

After:

```text
user::rw-
group::r--
other::---
```

---

# 25. Recursive ACL

Directory එකක් ඇතුළේ files සහ subdirectories ගොඩක් තියෙනවා කියලා හිතමු:

```text
/project
├── app
├── config
├── logs
└── uploads
```

Alice ට හැම එකකටම permission දෙන්න:

```bash
setfacl -R -m u:alice:rwx /project
```

මෙහි:

```text
-R
```

කියන්නේ:

> Recursive

එනම් `/project` ඇතුළේ තියෙන subdirectories/files වලටත් apply වෙනවා.

**Production server එකක recursive ACL changes කරනකොට සැලකිලිමත් වෙන්න.**

---

# 26. Recursive Default ACL

Directory එකක් ඇතුළේ තියෙන directories වලටත් default ACL දාන්න ඕන නම් advanced command එකක් භාවිතා කරන්න පුළුවන්:

```bash
find /project -type d -exec setfacl -d -m u:alice:rwx {} \;
```

මේක `/project` යටතේ තියෙන directories වලට default ACL set කරනවා.

---

# 27. ACL Backup කිරීම

ACL information save කරන්න:

```bash
getfacl file1 > acl.txt
```

පසුව restore කරන්න:

```bash
setfacl --restore=acl.txt
```

මේක useful වෙන්නේ:

- Server migration
    
- Backup
    
- Disaster recovery
    
- Server rebuild
    
- Permission migration
    

වගේ අවස්ථාවලදී.

---

# 28. `ls -l` වල `+` ලකුණ

මේකත් මතක තියාගන්න.

```bash
ls -l file.txt
```

කළාම:

```text
-rw-rw-r--+ 1 root developers 1024 report.txt
```

වගේ පේන්න පුළුවන්.

මෙහි:

```text
+
```

කියන්නේ:

> මේ file එකට extended ACL තියෙනවා.

Normal:

```text
-rw-r--r--
```

ACL තියෙනවා:

```text
-rw-r--r--+
```

ඒ `+` එක දැක්කාම:

```bash
getfacl file.txt
```

කරලා බලන්න.

---

# 29. ACL සහ `chmod`

ACL සහ `chmod` එකිනෙකට සම්බන්ධයි.

උදාහරණයක්:

```bash
setfacl -m u:bob:rwx file.txt
```

ඊට පස්සේ:

```bash
chmod 640 file.txt
```

කළොත් ACL mask එකට බලපෑමක් වෙන්න පුළුවන්.

ඒ නිසා ACL තියෙන file එකකට `chmod` කරනකොට permissions carefully check කරන්න.

---

# 30. ACL vs chmod

### `chmod`

Simple permissions සඳහා:

```text
Owner
Group
Others
```

උදාහරණය:

```bash
chmod 750 /project
```

එනම්:

```text
Owner  → rwx
Group  → r-x
Others → ---
```

---

### ACL

Specific users/groups සඳහා:

```text
Alice   → rwx
Bob     → rw-
Charlie → r-x
Dev     → rwx
QA      → r-x
Others  → ---
```

ඒ නිසා:

```text
chmod = Basic permissions
ACL   = Fine-grained permissions
```

---

# 31. ACL vs Groups

හොඳ Linux administration practice එකක්:

> **Broad access සඳහා Groups භාවිතා කරන්න. Exceptions / Fine-grained access සඳහා ACL භාවිතා කරන්න.**

උදාහරණයක්:

```text
/project

developers → rwx
testers    → r-x
```

ඊට අමතරව Alice ට special permission එකක් ඕන:

```bash
setfacl -m u:alice:rwx /project
```

මේක ACL වල practical use case එකක්.

---

# 32. ACL Authentication නෙවෙයි

ACL කියන්නේ authentication system එකක් නෙවෙයි.

ACL එකෙන් අහන්නේ:

> **"මේ user ට මොනවද කරන්න පුළුවන්?"**

Authentication එකෙන් අහන්නේ:

> **"මේ user කවුද?"**

Concept එක:

```text
Authentication
      ↓
Who are you?
      ↓
Alice
      ↓
Authorization
      ↓
ACL
      ↓
What can Alice access?
```

---

# 33. ACL සහ SELinux

System Engineer කෙනෙක් විදිහට මේක අනිවාර්යයෙන්ම දැනගන්න.

**ACL සහ SELinux එකම දෙයක් නෙවෙයි.**

ACL:

> User/group permissions control කරනවා.

SELinux:

> Mandatory Access Control policy එකක් implement කරනවා.

ඒ නිසා ACL එකෙන් access allow කරලා තිබුණත් SELinux එකෙන් deny කරන්න පුළුවන්.

Concept එක:

```text
User
 ↓
Linux Permissions
 ↓
ACL
 ↓
SELinux
 ↓
Access Decision
```

උදාහරණයක්:

```text
ACL      → ALLOW
SELinux  → DENY

Result   → ACCESS DENIED
```

SELinux context බලන්න:

```bash
ls -Z file.txt
```

SELinux status බලන්න:

```bash
getenforce
```

---

# 34. ACL සහ Docker

Docker mounted directories වලත් ACL useful වෙන්න පුළුවන්.

Host එකේ:

```text
/opt/application/uploads
```

Container එකේ:

```text
/app/uploads
```

Host user කෙනෙක්ට access දෙන්න:

```bash
setfacl -m u:developer:rwx /opt/application/uploads
```

නමුත් Docker වලදී වැදගත් දෙයක් තියෙනවා.

Container එකේ process එක run වෙන්නේ specific:

```text
UID
GID
```

එකක් යටතේ වෙන්න පුළුවන්.

ඒ නිසා ACL දාන්න කලින් actual UID/GID එක check කරන එක වැදගත්.

---

# 35. Web Server එකක ACL

උදාහරණයක්:

```text
/var/www/myapp
```

Users:

```text
nginx
developer
deploy
```

ඔයාට ඕන:

```text
nginx     → read
developer → read/write
deploy    → read/write/execute
```

ACL භාවිතා කරන්න පුළුවන්:

```bash
setfacl -m u:developer:rwx /var/www/myapp
setfacl -m u:nginx:r-x /var/www/myapp
```

මේකෙන් primary ownership model එක වෙනස් නොකර specific access ලබාදෙන්න පුළුවන්.

---

# 36. Deployment Server එකක ACL

ඔයාගේ deployment environment එකක් තියෙනවා කියලා හිතමු:

```text
/opt/apps/myapp
```

Users:

```text
deploy
developer
appuser
```

Deployment user:

```text
deploy
```

ට access දෙන්න:

```bash
setfacl -m u:deploy:rwx /opt/apps/myapp
```

ඊට පස්සේ:

```bash
getfacl /opt/apps/myapp
```

කරලා verify කරන්න.

CI/CD environments වලදී මේක useful.

නමුත් ACL භාවිතා කරන්නේ **හොඳ ownership/group design එකකට substitute එකක් විදිහට නොවෙයි**.

---

# 37. Log Directory එකක ACL

උදාහරණයක්:

```text
/var/log/myapp
```

Application user:

```text
appuser
```

Monitoring user:

```text
monitor
```

Requirements:

```text
appuser  → rwx
monitor  → r-x
```

Monitor user ට:

```bash
setfacl -m u:monitor:rx /var/log/myapp
```

දෙන්න පුළුවන්.

මේකෙන් logs global users හැමෝටම readable කරන එක වැළකෙනවා.

---

# 38. Complete Practical ACL Example

මුලින් directory එකක් හදමු:

```bash
mkdir /shared
```

File එකක් හදමු:

```bash
touch /shared/report.txt
```

Permissions බලමු:

```bash
ls -l /shared/report.txt
```

දැන් Bob ට read/write දෙමු:

```bash
setfacl -m u:bob:rw /shared/report.txt
```

Check කරන්න:

```bash
getfacl /shared/report.txt
```

Result එක මෙවැනි වෙන්න පුළුවන්:

```text
# file: report.txt
# owner: root
# group: root
user::rw-
user:bob:rw-
group::r--
mask::rw-
other::---
```

දැන් Bob owner නොවුනත්:

```text
read
write
```

permissions තියෙනවා.

---

# 39. Directory Practical Example

Directory එකක්:

```bash
mkdir /shared/project
```

Bob ට full access දෙන්න:

```bash
setfacl -m u:bob:rwx /shared/project
```

Check:

```bash
getfacl /shared/project
```

Bob ට directory එක:

```text
enter
list
create
delete
modify
```

කරන්න පුළුවන්, ඇතුළත files වල permissions අනුව.

---

# 40. Inheritance එක Add කිරීම

අලුතින් create කරන objects වලටත් Bob ට permission ඕන නම්:

```bash
setfacl -d -m u:bob:rwx /shared/project
```

ඊට පස්සේ:

```bash
touch /shared/project/test.txt
```

ACL බලන්න:

```bash
getfacl /shared/project/test.txt
```

Bob සඳහා ACL entry එක inherited වෙලා තියෙනවාද බලන්න පුළුවන්.

---

# 41. ACL Troubleshooting

User කෙනෙක් කියනවා:

> "මට permission තියෙනවා. ඒත් file එක access කරන්න බැහැ."

මේ වෙලාවේ මේ order එකට check කරන්න.

### 1. Basic permissions

```bash
ls -l file.txt
```

### 2. ACL

```bash
getfacl file.txt
```

### 3. Parent directories

```bash
namei -l /path/to/file.txt
```

`namei` command එක ඉතාම useful.

User ට file එක access කරන්න නම් parent directories හැම එකකම අවශ්‍ය `x` / traverse permission තිබිය යුතුයි.

### 4. SELinux

```bash
ls -Z file.txt
```

### 5. User identity

```bash
id username
```

### 6. Filesystem / mount

```bash
mount
```

---

# 42. ACL වල වැදගත් Commands

|Command|භාවිතය|
|---|---|
|`getfacl file`|ACL බලන්න|
|`setfacl -m ... file`|ACL modify කරන්න|
|`setfacl -x ... file`|ACL entry එකක් remove කරන්න|
|`setfacl -b file`|Extended ACL remove කරන්න|
|`setfacl -d -m ... dir`|Default ACL set කරන්න|
|`setfacl -R ... dir`|Recursive ACL|
|`getfacl -R dir`|Recursive ACL බලන්න|
|`getfacl file > acl.txt`|ACL backup කරන්න|
|`setfacl --restore=acl.txt`|ACL restore කරන්න|
|`ls -l`|`+` තිබේද බලන්න|

---

# 43. ACL Structure එක මතක තියාගන්න

මේ structure එක **හොඳට memorize කරගන්න**:

```text
ACL
│
├── user::        → Owner
├── user:name     → Specific User
│
├── group::       → Owning Group
├── group:name    → Specific Group
│
├── mask::        → Maximum Effective Permission
│
└── other::       → Others
```

උදාහරණයක්:

```text
user::rwx
user:alice:rwx
user:bob:rw-
group::r-x
group:developers:rwx
mask::rwx
other::---
```

---

# 44. ACL vs chmod vs chown vs SELinux

System Engineer කෙනෙක් විදිහට මේ හතර වෙන වෙනම තේරුම් ගන්න.

|Command / Technology|ප්‍රධාන භාවිතය|
|---|---|
|`chown`|Owner / Group වෙනස් කිරීම|
|`chmod`|Basic permissions වෙනස් කිරීම|
|`setfacl`|Fine-grained permissions ලබාදීම|
|SELinux|Mandatory Access Control / Security Policy|

### `chown`

```bash
chown appuser:appgroup app.txt
```

අර්ථය:

> මේ file එකේ owner/group කවුද?

### `chmod`

```bash
chmod 640 app.txt
```

අර්ථය:

> Owner / Group / Others මොනවා කරන්න පුළුවන්ද?

### `setfacl`

```bash
setfacl -m u:john:rw app.txt
```

අර්ථය:

> John ට special permission එකක් දෙන්න.

### SELinux

```bash
ls -Z app.txt
```

අර්ථය:

> මේ file එකේ SELinux security context එක මොකක්ද?

---

# 45. ACL ඉගෙනගන්න ඕන හොඳම Order එක

Linux Administration ඉගෙන ගන්නවා නම් ACL එක මේ order එකට ඉගෙනගන්න:

```text
Linux rwx Permissions
        ↓
chmod
        ↓
chown / Groups
        ↓
ACL
        ↓
ACL Mask
        ↓
Default ACL / Inheritance
        ↓
SELinux
        ↓
ACL + SELinux Troubleshooting
```

---

# 46. අන්තිමට මතක තියාගන්න ඕන වැදගත්ම දේවල් 5

### 1. ACL = Fine-Grained Linux Permissions

```text
chmod → Owner / Group / Others
ACL   → Specific Users / Groups
```

### 2. ACL බලන්න

```bash
getfacl file
```

### 3. User කෙනෙක්ට permission දෙන්න

```bash
setfacl -m u:username:rwx file
```

### 4. User ACL එක remove කරන්න

```bash
setfacl -x u:username file
```

### 5. Default ACL = Inheritance

```bash
setfacl -d -m u:username:rwx directory
```

---

## ⭐ ACL එකේ Core Concept එක

මේක තමයි හොඳටම මතක තියාගන්න ඕන:

```text
ACL Permission
      +
ACL MASK
      ↓
Effective Permission
```

උදාහරණයක්:

```text
User Bob        → rwx
ACL Mask        → r--
---------------------
Effective       → r--
```

ඒ නිසා ACL ඉගෙනගන්නකොට **`mask::` කියන එක ignore කරන්න එපා.**

---

### 🔥 Practical System Engineer Example

Production server එකක:

```text
/opt/tiikmeve-apps/aatm
```

වගේ application directory එකක් තියෙනවා කියලා හිතන්න.

Users:

```text
appuser
deploy
developer
```

ඔයාට:

```text
appuser   → Application access
deploy    → Deployment access
developer → Development access
others    → No access
```

වගේ controlled permission model එකක් හදන්න ඕන නම් ACL එක භාවිතා කරන්න පුළුවන්.

**`chmod -R 777` දානවා වෙනුවට**, අවශ්‍ය users/groups වලට විතරක් permissions දෙන්න ACL භාවිතා කරන එක security පැත්තෙන් වඩා හොඳ approach එකක්.