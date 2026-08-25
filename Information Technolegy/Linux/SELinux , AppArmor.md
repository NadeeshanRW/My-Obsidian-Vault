<<<<<<< HEAD
ඔව්. මේ දෙකම Linux security වල **Mandatory Access Control (MAC)** concepts. හැබැයි **SELinux** සහ **AppArmor** වැඩ කරන model එක වෙනස්. Linux administration / security architecture ඉගෙන ගන්නවා නම් මේ difference එක හොඳට තේරුම් ගන්න ඕන.

---

# 1. මුලින්ම — Traditional Linux Permissions vs MAC

Linux වල සාමාන්‍ය permissions:

```text
User
 │
 ├── Owner
 ├── Group
 └── Others
       │
       ├── read
       ├── write
       └── execute
```

උදාහරණයක්:

```bash
ls -l /var/www/html/index.html
```

Output එක conceptually:

```text
-rw-r--r--  www-data www-data  index.html
```

මේකෙන් කියන්නේ:

- owner → read/write
    
- group → read
    
- others → read
    

හැබැයි මෙතන problem එකක් තියෙනවා.

Application එක compromise වුණොත්?

උදාහරණයක්:

```text
Internet
   ↓
Nginx
   ↓
Vulnerability
   ↓
Attacker controls Nginx
```

Nginx user එකට access තියෙන files/resources තවමත් access කරන්න පුළුවන්.

මෙතන තමයි **SELinux / AppArmor** වගේ MAC systems වැදගත් වෙන්නේ.

---

# 2. MAC කියන්නේ මොකක්ද?

MAC = **Mandatory Access Control**

සරලව:

> Linux permissions කියන්නේ "මේ user ට මේ file එක access කරන්න පුළුවන්ද?" කියන එක.

MAC කියන්නේ:

> **"මේ process එකට මේ resource එක access කරන්න policy එකෙන් allow කරලා තියෙනවද?"**

ඒ කියන්නේ user permission pass වුණත් MAC policy එකෙන් deny කරන්න පුළුවන්.

```text
User
  ↓
Linux DAC permissions
  ↓
Allowed?
  ↓
MAC policy
  ↓
Allowed?
  ↓
Resource
```

මෙතන:

**DAC** = Discretionary Access Control  
**MAC** = Mandatory Access Control

SELinux සහ AppArmor දෙකම මේ MAC layer එකට belong වෙනවා.

---

# 3. SELinux 🔐

SELinux = **Security-Enhanced Linux**

Originally developed from work by the NSA and others, and now maintained as an open-source Linux security technology.

RHEL ecosystem එකේ මේක **extremely important**.

Commonly:

- RHEL
    
- Rocky Linux
    
- AlmaLinux
    
- Fedora
    

වගේ systems වල SELinux important.

---

# 4. SELinux එකේ main idea එක

SELinux කියන්නේ:

> **Process එකකට resource එක access කරන්න පුළුවන් ද කියන එක labels + security policy මත තීරණය කරන system එකක්.**

ඒ නිසා SELinux එකේ **labels** concept එක ඉතා වැදගත්.

උදාහරණයක්:

```text
Nginx Process
     │
     ↓
Security Context
     │
     ↓
Policy
     │
     ↓
File Context
     │
     ↓
Allow / Deny
```

---

# 5. SELinux Security Context

SELinux වල file/process එකකට security context එකක් තියෙනවා.

බලන්න:

```bash
ls -Z
```

උදාහරණයක්:

```text
-rw-r--r--. root root system_u:object_r:httpd_sys_content_t:s0 index.html
```

මෙතන:

```text
system_u:object_r:httpd_sys_content_t:s0
```

තමයි SELinux context එක.

මේකේ important parts:

```text
system_u
   ↓
user

object_r
   ↓
role

httpd_sys_content_t
   ↓
type

s0
   ↓
level
```

මුලින්ම deep focus කරන්න ඕනේ:

> **Type**

---

# 6. SELinux Type එක

Suppose Nginx/Apache web server එකට files serve කරන්න ඕන.

SELinux policy එක කියන්න පුළුවන්:

```text
httpd process
       ↓
can read
       ↓
httpd_sys_content_t
```

ඒ නිසා file එකේ normal Linux permissions allow වුණත්:

```text
Wrong SELinux context
        ↓
SELinux DENY
```

වෙන්න පුළුවන්.

---

# 7. මේක practical example එකකින්

Suppose web server එක:

```text
/var/www/html
```

use කරනවා.

File එක:

```bash
/var/www/html/index.html
```

Normal permissions:

```text
-rw-r--r--
```

හරි.

SELinux context එකත් correct:

```text
httpd_sys_content_t
```

හරි.

එතකොට:

```text
Nginx
  ↓
index.html
  ↓
Allowed
```

---

## දැන් file එක වෙන තැනකට move කළොත්?

```bash
/opt/mywebsite/index.html
```

Normal permissions හරි.

```text
-rw-r--r--
```

ඒත් SELinux context එක web server එකට appropriate නැත්නම්:

```text
Nginx
  ↓
Linux permissions → ALLOW
  ↓
SELinux → DENY
  ↓
Access denied
```

මේක තමයි Linux administrators ලාට මුලින් confusing වෙන්නේ.

> "Permissions හරි. එහෙනම් Nginx ඇයි file එක read කරන්නේ නැත්තේ?"

Answer එක:

**SELinux policy.**

---

# 8. SELinux modes

SELinux වල main modes තුනක්:

```text
Enforcing
Permissive
Disabled
```

### Enforcing

Actual policy enforce කරනවා.

```text
Violation
   ↓
DENY
```

Production වල generally මේ mode එක desired.

---

### Permissive

Policy violations log කරනවා.

නමුත් block කරන්නේ නැහැ.

```text
Violation
   ↓
LOG
   ↓
ALLOW
```

Troubleshooting/testing වල useful.

---

### Disabled

SELinux disabled.

```text
No SELinux enforcement
```

Modern production environment එකක simply disable කිරීම සාමාන්‍යයෙන් first-choice troubleshooting solution එකක් නෙමෙයි.

---

# 9. SELinux commands

Status:

```bash
getenforce
```

Output:

```text
Enforcing
```

Detailed status:

```bash
sestatus
```

Context:

```bash
ls -Z
```

Process context:

```bash
ps -eZ
```

Context change / restore:

```bash
restorecon
```

Policy/context management වල commonly:

```bash
semanage
```

SELinux denials troubleshoot කරන්න logs බලනවා.

Commonly:

```bash
ausearch
```

---

# 10. SELinux Boolean

මේකත් important concept එකක්.

SELinux policies sometimes configurable options තියෙනවා.

ඒවා **Booleans**.

Check:

```bash
getsebool -a
```

Example concept:

```text
httpd_can_network_connect --> off
```

මේක application එකකට certain network behavior allow/deny කරන policy control එකක් වගේ.

Enable කිරීම:

```bash
setsebool -P httpd_can_network_connect on
```

`-P` කියන්නේ persistent configuration එකකට apply කරන්න.

---

# 11. SELinux troubleshooting mindset

Nginx/Apache එක file එක access කරන්නේ නැහැ.

ඔයා immediately:

```bash
chmod 777
```

කරන්න එපා.

First think:

```text
1. Linux permissions?
       ↓
2. SELinux context?
       ↓
3. SELinux policy?
       ↓
4. SELinux denial logs?
```

මේ mindset එක **professional Linux administration** වල වැදගත්.

---

# 12. AppArmor 🛡️

දැන් AppArmor.

AppArmor = Linux **Mandatory Access Control** security system එකක්.

Ubuntu ecosystem එකේ මේක particularly important.

Commonly associated with:

- Ubuntu
    
- Debian
    
- SUSE/openSUSE
    

---

# 13. AppArmor main idea

AppArmor කියන්නේ:

> **Application එකකට allowed operations/resource access rules profile එකකින් define කරන system එකක්.**

මෙතන focus එක:

```text
Application
     ↓
AppArmor Profile
     ↓
Allowed actions
     ↓
Resources
```

SELinux වගේ labels everywhere කියන model එකට වඩා AppArmor generally **path-based profile** model එකක්.

---

# 14. AppArmor Profile

උදාහරණයක් විදිහට Nginx.

Conceptually:

```text
Nginx
  ↓
AppArmor profile
  ↓
Can read:
/var/www/**
  
Can access:
/etc/nginx/**

Cannot access:
/home/private/**
```

මේක application-specific security boundary එකක් වගේ.

---

# 15. AppArmor rules

AppArmor profile එකක conceptually:

```text
/var/www/** r,
/etc/nginx/** r,
/var/log/nginx/** rw,
```

මෙතන:

```text
r  = read
w  = write
```

ඒ කියන්නේ application එකට:

```text
/var/www/
```

under files read කරන්න allow.

නමුත්:

```text
/home/user/private/
```

access කරන්න rule එකක් නැත්නම් deny වෙන්න පුළුවන්.

---

# 16. AppArmor profiles

Profiles බලන්න:

```bash
sudo aa-status
```

Common profile states:

```text
Enforce
Complain
```

### Enforce

Profile rules actually enforce කරනවා.

```text
Violation
   ↓
DENY
```

### Complain

Violation log කරනවා, generally block කරන්නේ නැහැ.

```text
Violation
   ↓
LOG
   ↓
ALLOW
```

SELinux වල **Permissive** mode එකට conceptually similar.

---

# 17. AppArmor commands

Status:

```bash
sudo aa-status
```

Profiles:

```bash
ls /etc/apparmor.d/
```

Profile reload:

```bash
sudo apparmor_parser -r /etc/apparmor.d/profile
```

AppArmor service:

```bash
systemctl status apparmor
```

Logs Ubuntu systems වල troubleshoot කරනකොට:

```bash
journalctl
```

හෝ kernel/audit logs inspect කරනවා.

---

# 18. SELinux vs AppArmor — biggest difference

මේක තමයි exam/interview වලත් වැදගත්.

||SELinux|AppArmor|
|---|---|---|
|MAC|✅|✅|
|Main model|Label/type-based|Path/profile-based|
|Security context|Yes|Not in SELinux-style|
|Profiles|Policy-based|Application profiles|
|Common ecosystem|RHEL/Fedora|Ubuntu/SUSE|
|Learning curve|Higher|Generally easier|
|Fine-grained policy|Very powerful|Powerful|
|Troubleshooting|Context + policy + audit|Profile + path + logs|

### Memory trick:

```text
SELinux
   ↓
LABEL / TYPE

AppArmor
   ↓
PATH / PROFILE
```

**මේ දෙක මතක තිබ්බොත් fundamental difference එක අමතක වෙන්නේ නැහැ.**

---

# 19. Same scenario — Nginx

Suppose:

```text
Nginx
   ↓
/opt/website/index.html
```

### Traditional Linux permissions

```text
File permissions
       ↓
Allow
```

But SELinux:

```text
Nginx
  ↓
SELinux policy
  ↓
File SELinux type
  ↓
Allowed?
```

AppArmor:

```text
Nginx
  ↓
AppArmor profile
  ↓
Path:
/opt/website/index.html
  ↓
Allowed?
```

ඒ නිසා දෙකේ mental model එක:

```text
SELinux:

Process
   +
Security Context
   +
Object Context
   +
Policy
   ↓
Decision


AppArmor:

Process
   +
Profile
   +
Path/rules
   ↓
Decision
```

---

# 20. Why do we need these if Linux permissions already exist?

මේක **ඉතා වැදගත් security concept එකක්.**

Suppose Nginx exploit වුණා.

```text
Internet
   ↓
Nginx
   ↓
Vulnerability
   ↓
Attacker code
```

Nginx process එකට:

```text
www-data
```

වගේ user එකක් තිබුණත්, ඒ user ට access තියෙන resources attack කරන්න පුළුවන්.

MAC layer එකෙන්:

```text
Nginx
  ↓
Allowed resources only
```

කියලා restrict කරන්න පුළුවන්.

ඒ නිසා:

> **Compromised application ≠ unlimited access**

මේක MAC වල biggest security value එකක්.

---

# 21. Defense in Depth

මේක architecture level එකෙන් බලමු.

```text
                Internet
                   ↓
                Firewall
                   ↓
             Authentication
                   ↓
            Linux Permissions
                   ↓
             SELinux/AppArmor
                   ↓
               Application
                   ↓
                Database
```

එක security layer එකක් fail වුණත් තවත් layer එකක් තියෙනවා.

ඒකට කියන්නේ:

> **Defense in Depth**

---

# 22. Ubuntu vs RHEL practical mindset

ඔයා Linux Administration + DevOps + Security path එක යනවා නම් මෙහෙම මතක තියාගන්න:

```text
Ubuntu
   ↓
AppArmor
   ↓
aa-status
   ↓
Profiles
   ↓
Path-based rules
```

```text
RHEL / Rocky / Alma
   ↓
SELinux
   ↓
getenforce / sestatus
   ↓
Contexts
   ↓
Labels / Types / Policies
```

### ⭐ Most important takeaway

**SELinux සහ AppArmor දෙකම Linux kernel security framework එකට සම්බන්ධ Mandatory Access Control systems.**

නමුත්:

```text
SELinux  → "මේ process/type එකට මේ labelled resource එක access කරන්න policy එකෙන් අවසර තියෙනවද?"

AppArmor → "මේ application profile එකට මේ path/resource එක access කරන්න rule එකෙන් අවසර තියෙනවද?"
```

=======
ඔව්. මේ දෙකම Linux security වල **Mandatory Access Control (MAC)** concepts. හැබැයි **SELinux** සහ **AppArmor** වැඩ කරන model එක වෙනස්. Linux administration / security architecture ඉගෙන ගන්නවා නම් මේ difference එක හොඳට තේරුම් ගන්න ඕන.

---

# 1. මුලින්ම — Traditional Linux Permissions vs MAC

Linux වල සාමාන්‍ය permissions:

```text
User
 │
 ├── Owner
 ├── Group
 └── Others
       │
       ├── read
       ├── write
       └── execute
```

උදාහරණයක්:

```bash
ls -l /var/www/html/index.html
```

Output එක conceptually:

```text
-rw-r--r--  www-data www-data  index.html
```

මේකෙන් කියන්නේ:

- owner → read/write
    
- group → read
    
- others → read
    

හැබැයි මෙතන problem එකක් තියෙනවා.

Application එක compromise වුණොත්?

උදාහරණයක්:

```text
Internet
   ↓
Nginx
   ↓
Vulnerability
   ↓
Attacker controls Nginx
```

Nginx user එකට access තියෙන files/resources තවමත් access කරන්න පුළුවන්.

මෙතන තමයි **SELinux / AppArmor** වගේ MAC systems වැදගත් වෙන්නේ.

---

# 2. MAC කියන්නේ මොකක්ද?

MAC = **Mandatory Access Control**

සරලව:

> Linux permissions කියන්නේ "මේ user ට මේ file එක access කරන්න පුළුවන්ද?" කියන එක.

MAC කියන්නේ:

> **"මේ process එකට මේ resource එක access කරන්න policy එකෙන් allow කරලා තියෙනවද?"**

ඒ කියන්නේ user permission pass වුණත් MAC policy එකෙන් deny කරන්න පුළුවන්.

```text
User
  ↓
Linux DAC permissions
  ↓
Allowed?
  ↓
MAC policy
  ↓
Allowed?
  ↓
Resource
```

මෙතන:

**DAC** = Discretionary Access Control  
**MAC** = Mandatory Access Control

SELinux සහ AppArmor දෙකම මේ MAC layer එකට belong වෙනවා.

---

# 3. SELinux 🔐

SELinux = **Security-Enhanced Linux**

Originally developed from work by the NSA and others, and now maintained as an open-source Linux security technology.

RHEL ecosystem එකේ මේක **extremely important**.

Commonly:

- RHEL
    
- Rocky Linux
    
- AlmaLinux
    
- Fedora
    

වගේ systems වල SELinux important.

---

# 4. SELinux එකේ main idea එක

SELinux කියන්නේ:

> **Process එකකට resource එක access කරන්න පුළුවන් ද කියන එක labels + security policy මත තීරණය කරන system එකක්.**

ඒ නිසා SELinux එකේ **labels** concept එක ඉතා වැදගත්.

උදාහරණයක්:

```text
Nginx Process
     │
     ↓
Security Context
     │
     ↓
Policy
     │
     ↓
File Context
     │
     ↓
Allow / Deny
```

---

# 5. SELinux Security Context

SELinux වල file/process එකකට security context එකක් තියෙනවා.

බලන්න:

```bash
ls -Z
```

උදාහරණයක්:

```text
-rw-r--r--. root root system_u:object_r:httpd_sys_content_t:s0 index.html
```

මෙතන:

```text
system_u:object_r:httpd_sys_content_t:s0
```

තමයි SELinux context එක.

මේකේ important parts:

```text
system_u
   ↓
user

object_r
   ↓
role

httpd_sys_content_t
   ↓
type

s0
   ↓
level
```

මුලින්ම deep focus කරන්න ඕනේ:

> **Type**

---

# 6. SELinux Type එක

Suppose Nginx/Apache web server එකට files serve කරන්න ඕන.

SELinux policy එක කියන්න පුළුවන්:

```text
httpd process
       ↓
can read
       ↓
httpd_sys_content_t
```

ඒ නිසා file එකේ normal Linux permissions allow වුණත්:

```text
Wrong SELinux context
        ↓
SELinux DENY
```

වෙන්න පුළුවන්.

---

# 7. මේක practical example එකකින්

Suppose web server එක:

```text
/var/www/html
```

use කරනවා.

File එක:

```bash
/var/www/html/index.html
```

Normal permissions:

```text
-rw-r--r--
```

හරි.

SELinux context එකත් correct:

```text
httpd_sys_content_t
```

හරි.

එතකොට:

```text
Nginx
  ↓
index.html
  ↓
Allowed
```

---

## දැන් file එක වෙන තැනකට move කළොත්?

```bash
/opt/mywebsite/index.html
```

Normal permissions හරි.

```text
-rw-r--r--
```

ඒත් SELinux context එක web server එකට appropriate නැත්නම්:

```text
Nginx
  ↓
Linux permissions → ALLOW
  ↓
SELinux → DENY
  ↓
Access denied
```

මේක තමයි Linux administrators ලාට මුලින් confusing වෙන්නේ.

> "Permissions හරි. එහෙනම් Nginx ඇයි file එක read කරන්නේ නැත්තේ?"

Answer එක:

**SELinux policy.**

---

# 8. SELinux modes

SELinux වල main modes තුනක්:

```text
Enforcing
Permissive
Disabled
```

### Enforcing

Actual policy enforce කරනවා.

```text
Violation
   ↓
DENY
```

Production වල generally මේ mode එක desired.

---

### Permissive

Policy violations log කරනවා.

නමුත් block කරන්නේ නැහැ.

```text
Violation
   ↓
LOG
   ↓
ALLOW
```

Troubleshooting/testing වල useful.

---

### Disabled

SELinux disabled.

```text
No SELinux enforcement
```

Modern production environment එකක simply disable කිරීම සාමාන්‍යයෙන් first-choice troubleshooting solution එකක් නෙමෙයි.

---

# 9. SELinux commands

Status:

```bash
getenforce
```

Output:

```text
Enforcing
```

Detailed status:

```bash
sestatus
```

Context:

```bash
ls -Z
```

Process context:

```bash
ps -eZ
```

Context change / restore:

```bash
restorecon
```

Policy/context management වල commonly:

```bash
semanage
```

SELinux denials troubleshoot කරන්න logs බලනවා.

Commonly:

```bash
ausearch
```

---

# 10. SELinux Boolean

මේකත් important concept එකක්.

SELinux policies sometimes configurable options තියෙනවා.

ඒවා **Booleans**.

Check:

```bash
getsebool -a
```

Example concept:

```text
httpd_can_network_connect --> off
```

මේක application එකකට certain network behavior allow/deny කරන policy control එකක් වගේ.

Enable කිරීම:

```bash
setsebool -P httpd_can_network_connect on
```

`-P` කියන්නේ persistent configuration එකකට apply කරන්න.

---

# 11. SELinux troubleshooting mindset

Nginx/Apache එක file එක access කරන්නේ නැහැ.

ඔයා immediately:

```bash
chmod 777
```

කරන්න එපා.

First think:

```text
1. Linux permissions?
       ↓
2. SELinux context?
       ↓
3. SELinux policy?
       ↓
4. SELinux denial logs?
```

මේ mindset එක **professional Linux administration** වල වැදගත්.

---

# 12. AppArmor 🛡️

දැන් AppArmor.

AppArmor = Linux **Mandatory Access Control** security system එකක්.

Ubuntu ecosystem එකේ මේක particularly important.

Commonly associated with:

- Ubuntu
    
- Debian
    
- SUSE/openSUSE
    

---

# 13. AppArmor main idea

AppArmor කියන්නේ:

> **Application එකකට allowed operations/resource access rules profile එකකින් define කරන system එකක්.**

මෙතන focus එක:

```text
Application
     ↓
AppArmor Profile
     ↓
Allowed actions
     ↓
Resources
```

SELinux වගේ labels everywhere කියන model එකට වඩා AppArmor generally **path-based profile** model එකක්.

---

# 14. AppArmor Profile

උදාහරණයක් විදිහට Nginx.

Conceptually:

```text
Nginx
  ↓
AppArmor profile
  ↓
Can read:
/var/www/**
  
Can access:
/etc/nginx/**

Cannot access:
/home/private/**
```

මේක application-specific security boundary එකක් වගේ.

---

# 15. AppArmor rules

AppArmor profile එකක conceptually:

```text
/var/www/** r,
/etc/nginx/** r,
/var/log/nginx/** rw,
```

මෙතන:

```text
r  = read
w  = write
```

ඒ කියන්නේ application එකට:

```text
/var/www/
```

under files read කරන්න allow.

නමුත්:

```text
/home/user/private/
```

access කරන්න rule එකක් නැත්නම් deny වෙන්න පුළුවන්.

---

# 16. AppArmor profiles

Profiles බලන්න:

```bash
sudo aa-status
```

Common profile states:

```text
Enforce
Complain
```

### Enforce

Profile rules actually enforce කරනවා.

```text
Violation
   ↓
DENY
```

### Complain

Violation log කරනවා, generally block කරන්නේ නැහැ.

```text
Violation
   ↓
LOG
   ↓
ALLOW
```

SELinux වල **Permissive** mode එකට conceptually similar.

---

# 17. AppArmor commands

Status:

```bash
sudo aa-status
```

Profiles:

```bash
ls /etc/apparmor.d/
```

Profile reload:

```bash
sudo apparmor_parser -r /etc/apparmor.d/profile
```

AppArmor service:

```bash
systemctl status apparmor
```

Logs Ubuntu systems වල troubleshoot කරනකොට:

```bash
journalctl
```

හෝ kernel/audit logs inspect කරනවා.

---

# 18. SELinux vs AppArmor — biggest difference

මේක තමයි exam/interview වලත් වැදගත්.

||SELinux|AppArmor|
|---|---|---|
|MAC|✅|✅|
|Main model|Label/type-based|Path/profile-based|
|Security context|Yes|Not in SELinux-style|
|Profiles|Policy-based|Application profiles|
|Common ecosystem|RHEL/Fedora|Ubuntu/SUSE|
|Learning curve|Higher|Generally easier|
|Fine-grained policy|Very powerful|Powerful|
|Troubleshooting|Context + policy + audit|Profile + path + logs|

### Memory trick:

```text
SELinux
   ↓
LABEL / TYPE

AppArmor
   ↓
PATH / PROFILE
```

**මේ දෙක මතක තිබ්බොත් fundamental difference එක අමතක වෙන්නේ නැහැ.**

---

# 19. Same scenario — Nginx

Suppose:

```text
Nginx
   ↓
/opt/website/index.html
```

### Traditional Linux permissions

```text
File permissions
       ↓
Allow
```

But SELinux:

```text
Nginx
  ↓
SELinux policy
  ↓
File SELinux type
  ↓
Allowed?
```

AppArmor:

```text
Nginx
  ↓
AppArmor profile
  ↓
Path:
/opt/website/index.html
  ↓
Allowed?
```

ඒ නිසා දෙකේ mental model එක:

```text
SELinux:

Process
   +
Security Context
   +
Object Context
   +
Policy
   ↓
Decision


AppArmor:

Process
   +
Profile
   +
Path/rules
   ↓
Decision
```

---

# 20. Why do we need these if Linux permissions already exist?

මේක **ඉතා වැදගත් security concept එකක්.**

Suppose Nginx exploit වුණා.

```text
Internet
   ↓
Nginx
   ↓
Vulnerability
   ↓
Attacker code
```

Nginx process එකට:

```text
www-data
```

වගේ user එකක් තිබුණත්, ඒ user ට access තියෙන resources attack කරන්න පුළුවන්.

MAC layer එකෙන්:

```text
Nginx
  ↓
Allowed resources only
```

කියලා restrict කරන්න පුළුවන්.

ඒ නිසා:

> **Compromised application ≠ unlimited access**

මේක MAC වල biggest security value එකක්.

---

# 21. Defense in Depth

මේක architecture level එකෙන් බලමු.

```text
                Internet
                   ↓
                Firewall
                   ↓
             Authentication
                   ↓
            Linux Permissions
                   ↓
             SELinux/AppArmor
                   ↓
               Application
                   ↓
                Database
```

එක security layer එකක් fail වුණත් තවත් layer එකක් තියෙනවා.

ඒකට කියන්නේ:

> **Defense in Depth**

---

# 22. Ubuntu vs RHEL practical mindset

ඔයා Linux Administration + DevOps + Security path එක යනවා නම් මෙහෙම මතක තියාගන්න:

```text
Ubuntu
   ↓
AppArmor
   ↓
aa-status
   ↓
Profiles
   ↓
Path-based rules
```

```text
RHEL / Rocky / Alma
   ↓
SELinux
   ↓
getenforce / sestatus
   ↓
Contexts
   ↓
Labels / Types / Policies
```

### ⭐ Most important takeaway

**SELinux සහ AppArmor දෙකම Linux kernel security framework එකට සම්බන්ධ Mandatory Access Control systems.**

නමුත්:

```text
SELinux  → "මේ process/type එකට මේ labelled resource එක access කරන්න policy එකෙන් අවසර තියෙනවද?"

AppArmor → "මේ application profile එකට මේ path/resource එක access කරන්න rule එකෙන් අවසර තියෙනවද?"
```

>>>>>>> a159bce7962d6af920654f254fefef8e27f23cd4
ඔයා **RHEL + Ubuntu දෙකම infrastructure/security පැත්තෙන් ඉගෙන ගන්නවා නම්**, දෙකම practical කරලා දැනගන්න ඕන. SELinux ටිකක් deeper සහ initially harder; AppArmor concept එක සාමාන්‍යයෙන් ඉක්මනින් grasp කරන්න ලේසියි.