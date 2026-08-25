<<<<<<< HEAD


**Nipper InfraSight by Titania** කියන්නේ සාමාන්‍ය vulnerability scanner එකකට වඩා ටිකක් වෙනස්, **network devices වල configuration එක deep ලෙස analyze කරලා security weaknesses, misconfigurations, exploitable access paths, hardening gaps සහ compliance evidence හොයන cybersecurity assessment tool එකක්.**

මම මේක **මොකක්ද → ඇතුළෙන් වැඩ කරන්නේ කොහොමද → මොනවද හොයන්නේ → practically use කරන්නේ කොහොමද → වෙනත් tools වලින් වෙනස → real example** කියලා breakdown කරන්නම්.

![Image](https://images.openai.com/static-rsc-4/pQSAbTvU92ycXZ-Ux9ekmXVwdLcdnFzwNK7Js5cLe6LJLryIiXG3lt2TwBLI_8QaYplkRLW7rixc7-lG8258TsuZP2ZKzqOz2tdICbTHjT5mKewoLLeINra6Cilm4rgD4WaihwIM4I5PlE1b8Dq5lLGnIVCyQjXfiNF-fsDOHVFXnzFHsgoSXJ5-MT_Q64dn?purpose=fullsize)

![Image](https://images.openai.com/static-rsc-4/xojEYOsetyzvzADIghWbaQzk8i9fjhm9ucFtuqjHRY698I_8_L-rkRYO_uDhXhykXOgFsGEkMCnSP7hsDf-X6fCf2SkLeU4iD0FPIMKiWnATrj8VVTcD2OxbEokDsk3yTn3FgJU_aOv7oSRiPamJPLzhe1MitWz7gIoF6qw1R12GeybrCeIasSUskO8glI9j?purpose=fullsize)

![Image](https://images.openai.com/static-rsc-4/gGcOKoM28-wHZqKuNKlJKf_RmgDsdNnhL48PyHKrxFFpnBcWlHmeq4rUV7tOODfp3ufs2QZ_DWs8OVeXSJFI-kNpZeqPGPbSoQY7iazIS3Sv2pxMri6SM92kxfbv4azCC-FYAfIdAA_mD5MgYG-hQNRNYYRdumzD6AwQ7EYw3bygbBQOsns04rf4tjau3dI8?purpose=fullsize)





![Image](https://images.openai.com/static-rsc-4/AbZt2HD2gNqBAC7FszrdpAQyfE1SdA7xUVLshhF0K3pcbWanN6FZmXNFynt0ehBa2MsaQeRYy34VtASwovO49aJ5vHkxHnLSLmm4zzLyfRWfE07zq-oMENdT5SL2zzLc74sTqX9g7UL8AVkeUjMH8UoucMcnzoVyMxLDwyROa-Q7QZfeB2NmA3BLcnrwkURB?purpose=fullsize)

## 1. මුලින්ම — Nipper InfraSight කියන්නේ මොකක්ද?

**Nipper InfraSight** කියන්නේ Titania company එකේ network-security configuration assessment platform එකක්.

එහි primary target එක:

- Routers
    
- Switches
    
- Firewalls
    
- Wireless Access Points
    
- SD-WAN/network devices
    

වගේ devices.

Titania කියන විදිහට Nipper InfraSight එක **180+ network devices** support කරනවා, Cisco, Palo Alto, Fortinet, Juniper, Check Point, Aruba, Sophos, Huawei, F5 වගේ vendors ඇතුළුව. ([Titania](https://www.titania.com/nipper-infrasight?utm_source=chatgpt.com "Nipper InfraSight"))

වැදගත්ම concept එක මේකයි:

> **Device එකට attack එකක් කරනවා වෙනුවට, device එකේ configuration එක අරගෙන "මේ configuration එක ඇත්තටම enforce කරන්නේ මොකක්ද?" කියලා analyze කරනවා.**

---

# 2. සාමාන්‍යයෙන් අපි හිතන vulnerability scanner එකයි මේකයි අතර වෙනස

උදාහරණයක් ගමු.

ඔයාගේ company එකේ firewall එකක් තියෙනවා.

```text
Internet
    |
    v
[ Firewall ]
    |
    +-------- Web Server
    |
    +-------- Database
    |
    +-------- Internal Network
```

සාමාන්‍ය vulnerability scanner එකක් server එකට scan කරලා:

```text
Port 22 open
Port 80 open
Old TLS version
CVE-XXXX
```

වගේ දේවල් හොයනවා.

**Nipper InfraSight එක වෙනස් angle එකකින් බලනවා.**

එය බලන්නේ:

```text
Firewall configuration
        ↓
ACL / Rules
NAT
Interfaces
Routing
VPN
Authentication
Services
Management access
Segmentation
Logging
Security settings
        ↓
"මේ device එක ඇත්තටම enforce කරන්නේ මොකක්ද?"
        ↓
Security weaknesses
```

ඒක නිසා මේක **configuration-centric security assessment** එකක්.

Titania official description එකේත් exported configurations වලින් device behavior එකේ virtual model එකක් build කරලා exploitable conditions/gaps test කරන බව කියනවා. ([Titania](https://www.titania.com/nipper-infrasight?utm_source=chatgpt.com "Nipper InfraSight"))

---

# 3. මෙහි තියෙන biggest advantage එක

### Live device එක touch කරන්න අවශ්‍ය නෑ.

උදාහරණයක්:

Cisco firewall/router එකක configuration එක:

```text
show running-config
```

වගේ command එකකින් export කරගත්තා කියමු.

ඊට පස්සේ:

```text
Cisco_Config.txt
        ↓
Nipper InfraSight
        ↓
Configuration analysis
        ↓
Security findings
        ↓
Risk prioritization
        ↓
Remediation instructions
        ↓
Report
```

ඒ කියන්නේ production firewall එකට scanner එකක් active ලෙස attack/probe කරන්න ඕනෙ නෑ.

Titania specifically කියන්නේ **exported configurations** use කරලා analysis කරන නිසා credentials නැතිව සහ live device එකට changes නොකර assessment කරන්න පුළුවන් කියලා. ([Titania](https://www.titania.com/nipper-infrasight?utm_source=chatgpt.com "Nipper InfraSight"))

---

# 4. එහෙනම් configuration එකෙන් මොනවද හොයන්නේ?

මේක තමයි වැදගත්ම කොටස.

## A. Misconfiguration

උදාහරණයක්:

```text
Firewall Rule #15

Source: ANY
Destination: ANY
Service: ANY
Action: ALLOW
```

මෙවැනි rule එකක් තිබුණොත් huge exposure එකක් වෙන්න පුළුවන්.

Nipperට configuration logic එක analyze කරලා මේ වගේ weak rules identify කරන්න පුළුවන්.

---

## B. Weak access control

උදාහරණයක්:

```text
Management interface
        ↓
SSH
        ↓
Source = ANY
```

එහෙම නම් management access එක unnecessarily broad වෙන්න පුළුවන්.

Nipper එක configuration context එකෙන් මේ වගේ weak access controls හඳුනාගන්න පුළුවන්. ([Titania](https://www.titania.com/nipper-infrasight?utm_source=chatgpt.com "Nipper InfraSight"))

---

# 5. Network segmentation problems

මේක Nipper එකේ interesting part එකක්.

හිතන්න:

```text
Internet
   |
Firewall
   |
   +---- DMZ
   |
   +---- Users
   |
   +---- Finance
   |
   +---- Database
```

Company එකේ intention එක:

```text
Internet → DMZ       YES
Internet → Database  NO

Users → Database     LIMITED
Finance → Database   YES
Guest → Finance      NO
```

නමුත් firewall rules වල mistake එකක් නිසා:

```text
Guest → Finance → Database
```

path එක theoretically open වෙලා තියෙන්න පුළුවන්.

Nipper configuration එක analyze කරලා **device configuration එකෙන් enforce වෙන network behavior** ගැන insight දෙන්න පුළුවන්.

Titania මේකට **behavior-based virtual modeling** කියන concept එක භාවිතා කරනවා. ([Titania](https://www.titania.com/nipper-infrasight?utm_source=chatgpt.com "Nipper InfraSight"))

---

# 6. Firewall rules ගැන විශේෂයෙන් useful

Firewall එකක rules 500ක්, 1000ක්, 5000ක් තියෙන environment එකක් imagine කරන්න.

Human කෙනෙක්ට manually බලලා:

```text
Rule 1
Rule 2
Rule 3
...
Rule 1000
```

analyse කරන එක අමාරුයි.

Nipperට configuration එක parse කරලා security perspective එකෙන් assess කරන්න පුළුවන්.

උදාහරණයක්:

```text
Rule 120
Source: 10.10.0.0/16
Destination: 172.16.10.0/24
Service: ANY
Action: ACCEPT
```

Tool එකේ concern එක:

> "මෙතන unnecessarily broad access එකක් තියෙනවද?"

ඒක security risk එකක් නම් finding එකක් ලෙස report වෙන්න පුළුවන්.

---

# 7. Hardening වලටත් use කරනවා

මේක **network device hardening** සඳහා ගොඩක් useful.

උදාහරණයක් Cisco router එකක්.

Security baseline එක:

```text
✓ SSH only
✓ Telnet disabled
✓ Strong authentication
✓ Unnecessary services disabled
✓ Secure SNMP
✓ Logging enabled
✓ NTP configured
✓ Management access restricted
✓ Password policies
✓ ACL restrictions
```

Device configuration එකේ:

```text
Telnet = ENABLED
SNMP = weak
Logging = disabled
Management = ANY
```

වගේ දේවල් තිබුණොත් Nipper finding generate කරයි.

Titania කියන්නේ CIS Benchmarks ඇතුළු hardening guidance/best-practice checks support කරන බවයි. ([Titania](https://www.titania.com/nipper-infrasight?utm_source=chatgpt.com "Nipper InfraSight"))

---

# 8. Vulnerability Management එකටත් use වෙනවා

මේක important distinction එකක්.

Nipper InfraSight එක **network-device vulnerability management** සඳහා use කරනවා.

ඒක configuration එකේ exact settings වලට findings tie කරනවා.

උදාහරණයක්:

```text
Device:
Cisco Firewall

Finding:
Weak / insecure configuration

Risk:
HIGH

Why:
Specific configuration creates exposure

Evidence:
Configuration lines/settings

Fix:
Specific remediation guidance
```

ඒ නිසා security engineer කෙනෙක්ට:

> "Issue එක තියෙනවා"

කියනවාට වඩා,

> "Issue එක මේ configuration setting එක නිසා ඇතිවෙලා තියෙනවා. මේක change කරන්න."

කියලා action කරන්න පුළුවන්.

Titania තමන්ගේ vulnerability-management capability එක configuration-based vulnerability detection ලෙස describe කරනවා. ([Titania](https://www.titania.com/nipper-infrasight/vulnerability-management?utm_source=chatgpt.com "Nipper InfraSight | Capabilities | Vulnerability Management"))

---

# 9. Compliance වලටත් use කරන්න පුළුවන්

මේක enterprises වලට ඉතා වැදගත්.

උදාහරණයක්:

Company එකට:

- [[NIST Framework]]
    
- PCI DSS
    
- CMMC
    
- DISA STIG
    

වගේ frameworks follow කරන්න ඕනේ.

Nipper InfraSight Compliance tier එකේ configuration findings **control-mapped evidence** විදිහට produce කරන්න පුළුවන්. ([Titania](https://www.titania.com/resource-center/nipper-infrasight-compliance-datasheet?utm_source=chatgpt.com "Nipper InfraSight (Compliance) Datasheet - Titania"))

උදාහරණයක්:

```text
NIST Control
      ↓
Network Device Configuration
      ↓
Pass / Fail
      ↓
Evidence
      ↓
Finding
      ↓
Remediation
```

ඒක auditor කෙනෙක්ට useful.

**හැබැයි Nipper එක "ඔයා compliant" කියලා legally certify කරන්නේ නෑ.** ඒක audit evidence/support ලබාදෙන tool එකක්. Final compliance decision එක auditor/regulator ගන්නවා. ([Titania](https://www.titania.com/solutions/use-cases/audits-assessments?utm_source=chatgpt.com "Solutions | Use-Cases | Audits & Assessments - Titania"))

---

# 10. Practically use කරන workflow එක

මේක තමයි ඔයාට practically මතක තියාගන්න ඕන flow එක.

### Step 1 — Device identify කරනවා

උදා:

```text
Cisco ASA
```

### Step 2 — Configuration export කරනවා

Device එකෙන් configuration එක ලබාගන්නවා.

```text
running-config
```

හෝ vendor/device එකට අදාළ export method එක.

### Step 3 — Nipper InfraSight එකට import කරනවා

```text
Configuration
       ↓
Nipper InfraSight
```

### Step 4 — Device එක parse කරනවා

Tool එක configuration syntax එක තේරුම් ගන්නවා.

### Step 5 — Virtual behavior model එකක් build කරනවා

මේක තමයි Nipper එකේ powerful part එක.

Configuration එකෙන්:

```text
Interfaces
Routes
ACLs
Firewall rules
NAT
Services
Access controls
```

වගේ things combine කරලා device එක behave වෙන ආකාරය model කරනවා.

### Step 6 — Security assessment

ඊට පස්සේ:

```text
Misconfiguration
Hardening gap
Weak access
Vulnerability
Segmentation issue
Policy issue
```

වගේ දේවල් identify කරනවා.

### Step 7 — Risk prioritize කරනවා

හැම finding එකම එකම priority එකක් නෙවෙයි.

```text
Critical
High
Medium
Low
```

වගේ risk perspective එකෙන් prioritize කරන්න පුළුවන්.

### Step 8 — Fix කරනවා

Tool එක device-specific remediation guidance දෙන්න පුළුවන්.

උදා:

```text
Finding
   ↓
Why dangerous
   ↓
Affected configuration
   ↓
Recommended fix
```

Titania specifically device-specific CLI commands/guidance ලබාදෙන බව සඳහන් කරනවා. ([Titania](https://www.titania.com/try/trial?utm_source=chatgpt.com "Request a Trial | Titania"))

### Step 9 — නැවත assessment

Fix කළාට පස්සේ configuration එක නැවත export කරලා:

```text
Before
  ↓
Finding
  ↓
Fix
  ↓
New Config
  ↓
Nipper
  ↓
Finding resolved?
```

කියලා verify කරන්න පුළුවන්.

---

# 11. Example එකක් — company firewall එකක්

හිතමු company එකේ FortiGate firewall එකක් තියෙනවා.

Configuration:

```text
Internet
    |
FortiGate
    |
    +---- Web Server
    |
    +---- Employee LAN
    |
    +---- Database
```

Nipperට config එක දුන්නා.

එයා findings 5ක් හොයාගත්තා කියමු:

```text
1. HIGH
Management access too broad

2. HIGH
Overly permissive firewall rule

3. MEDIUM
Weak service configuration

4. MEDIUM
Hardening baseline mismatch

5. LOW
Logging configuration issue
```

එතකොට security engineerට:

```text
HIGH
 ↓
Fix first

MEDIUM
 ↓
Next

LOW
 ↓
Later
```

කියලා prioritize කරන්න පුළුවන්.

මේක තමයි **risk-based vulnerability management** කියන්නේ.

---

# 12. Nipper vs Nessus වගේ tool එකක්

මේ distinction එක තේරුම් ගත්තොත් Nipper එක තේරෙනවා.

|Tool type|Main focus|
|---|---|
|Nessus-style vulnerability scanner|Hosts/services/vulnerabilities|
|Nmap|Network discovery/ports/services|
|SIEM|Logs/events|
|EDR|Endpoint behavior|
|Firewall|Traffic enforcement|
|**Nipper InfraSight**|**Network-device configuration/security assurance**|

ඒ කියන්නේ Nipper කියන්නේ **Nessus replace කරන්න හදපු එකක් නෙවෙයි.**

ඒවා එකට use කරන්න පුළුවන්.

උදා:

```text
Nmap
  ↓
Network discovery

Nessus
  ↓
Host vulnerability assessment

EDR
  ↓
Endpoint behavior

SIEM
  ↓
Security events

Nipper
  ↓
Router / Switch / Firewall configuration assurance
```

ඒක තමයි complete security stack එකක Nipperගේ position එක.

---

# 13. Nipper එකේ "offline" capability එක ඇයි වැදගත්?

මේක military / government / critical infrastructure environments වල ගොඩක් valuable.

උදාහරණයක්:

```text
Classified Network

     NO INTERNET
          |
          |
    [Network Devices]
          |
       Config
          ↓
   Offline Nipper
          ↓
     Assessment
```

Configuration data එක cloud එකකට upload කරන්න බැරි environment එකක් වෙන්න පුළුවන්.

Nipper InfraSight **Air Gapped** version එක isolated/classified/OT environments වල offline operation සඳහා design කරලා තියෙනවා. ([Titania](https://www.titania.com/resource-center/nipper-infrasight-air-gapped-datasheet?utm_source=chatgpt.com "Nipper InfraSight (Air gapped) Datasheet - Titania"))

---

# 14. Nipper InfraSight tiers

දැනට Titania describe කරන major options:

### **Nipper InfraSight — Essential**

Basic point-in-time:

- Configuration assessment
    
- Hardening
    
- Prioritized findings
    
- Remediation
    
- Reporting
    

([Titania](https://www.titania.com/nipper-infrasight/network-hardening?utm_source=chatgpt.com "Nipper InfraSight | Capabilities | Network Hardening"))

### **Nipper InfraSight — Compliance**

ඊට අමතරව:

- Control mapping
    
- Audit evidence
    
- CMMC
    
- NIST
    
- PCI DSS
    
- DISA STIG-related assessment
    

වගේ compliance-oriented capabilities. ([Titania](https://www.titania.com/resource-center/nipper-infrasight-compliance-datasheet?utm_source=chatgpt.com "Nipper InfraSight (Compliance) Datasheet - Titania"))

### **Nipper InfraSight — Air Gapped**

Offline / isolated environments සඳහා.

([Titania](https://www.titania.com/resource-center/nipper-infrasight-air-gapped-datasheet?utm_source=chatgpt.com "Nipper InfraSight (Air gapped) Datasheet - Titania"))

---

# 15. මේක install කරලා use කරන්න පුළුවන්ද?

ඔව්.

Current documentation අනුව Nipper InfraSight Windows සහ Ubuntu environments සඳහා available. Current v4.0.0 release එක Windows 11 සහ Ubuntu 22.04.5 tested platforms ලෙස සඳහන් කරනවා. ([Titania Docs](https://docs.titania.com/nipper-infrasight/release-notes/v4.0.0?utm_source=chatgpt.com "Nipper InfraSight v4.0.0"))

Titania official documentation එකේ installation, licensing, offline activation, first report creation වගේ user guides තියෙනවා. ([Titania Docs](https://docs.titania.com/nipper-infrasight/user-guides?utm_source=chatgpt.com "User Guides | Nipper InfraSight"))

[Titania Nipper InfraSight official page](https://www.titania.com/nipper-infrasight?utm_source=chatgpt.com)  
[Nipper InfraSight User Guides](https://docs.titania.com/nipper-infrasight/user-guides?utm_source=chatgpt.com)  
[Nipper InfraSight free trial](https://www.titania.com/try/trial?utm_source=chatgpt.com)


### මෙය Install කරන්නේ කොහේද? (ප්‍රධාන නීතිය)

- ❌ **Main Server එකේ හෝ සාමාන්‍ය සේවකයෙකුගේ PC එකක Install කරන්න එපා.**
    
- ✅ **IT හෝ Security අංශයේ වෙනම වෙන් කළ පරිගණකයක (Dedicated Workstation/Laptop) පමණක් Install කරන්න.**
    

### 3. වැඩ කරන ක්‍රමය (Workflow එක) කොහොමද?

මෙය ජාලය පුරා ස්වයංක්‍රීයව දත්ත මංකොල්ලකන (Scanner) tool එකක් නොවේ. ක්‍රමය ඉතා සරලයි:

1. **Router/Firewall එකෙන්** එහි සැකසුම් ගොනුව (Configuration File) ඩවුන්लोड කරගන්න.
    
2. ඒ ගොනුව **Security Workstation එකට** දමාගෙන, **Nipper** මෘදුකාංගයෙන් විවෘත කරන්න.
    
3. එය පරීක්ෂා කර ආරක්ෂක වාර්තාවක් (Report) ලබා දෙයි.
    

### 4. අන්තර්ජාලය අවශ්‍යද? (Offline පාවිච්චි කළ හැකිද?)

- **අන්තර්ජාලය අවශ්‍ය නැත.** සම්පූර්ණයෙන්ම අන්තර්ජාලය නොමැති (Isolated) පරිසරයක වුණත් මේක පාවිච්චි කරන්න පුළුවන්.
    
- බලපත්‍රය (License) සක්‍රීය කරගැනීමට පමණක් අන්තර්ජාලය ඇති වෙනත් පරිගණකයක් හරහා USB එකක් මඟින් එය කරගත හැක.
    

### කෙටි සාරාංශය (Quick Summary)

> **Production Server එකට අත තබන්න එපා.**
> 
> **Company එකේ ආරක්ෂාව සහ Settings පරීක්ෂා කිරීමට, වෙනම වෙන් කළ (Dedicated) PC එකක Nipper දමා, අවශ්‍ය Network Config Files පමණක් ගෙන විමර්ශනය කරන්න.**

# 16. මට නම් Nipper එකේ core concept එක මෙහෙමයි

ඔයා මේ diagram එක මතක තියාගන්න:

```text
             NETWORK DEVICE
                   │
                   │
          Export Configuration
                   │
                   ▼
          ┌─────────────────┐
          │ Nipper InfraSight│
          └────────┬────────┘
                   │
                   ▼
       Parse Configuration
                   │
                   ▼
      Build Behavior Model
                   │
                   ▼
        Security Assessment
                   │
       ┌───────────┼───────────┐
       ▼           ▼           ▼
   Vulnerability Misconfig   Hardening
       │           │           │
       └───────────┼───────────┘
                   ▼
             Risk Priority
                   │
                   ▼
          Remediation Guidance
                   │
                   ▼
             Fix Device
                   │
                   ▼
          Export New Config
                   │
                   ▼
             Re-assess
                   │
                   ▼
          Evidence / Report
```

**ඒක තමයි Nipper InfraSight එකේ heart එක.**

---

## 17. තවත් වැදගත් point එකක්

**Nipper InfraSight = "hack the network" tool එකක් නෙවෙයි.**

ඒක mainly:

> **"Network device එක configure කරලා තියෙන විදිහ අනුව, එය security-wise ඇත්තටම enforce කරන්නේ මොනවාද?"**

කියන ප්‍රශ්නයට answer එකක් දෙන්න හදන tool එකක්.

ඒ නිසා penetration testing එකක් වෙනුවට/ඒකට අමතරව **configuration assurance** කියන layer එක cover කරනවා.

Titania කියන විදිහට Nipper technology එක penetration testers විසින්ත් භාවිතා කරපු background එකක් තියෙනවා, නමුත් current InfraSight product positioning එක point-in-time hardening/compliance/configuration assessment එකට focus වෙනවා. ([Titania](https://www.titania.com/?utm_source=chatgpt.com "Proactive network security solutions | Titania"))

=======


**Nipper InfraSight by Titania** කියන්නේ සාමාන්‍ය vulnerability scanner එකකට වඩා ටිකක් වෙනස්, **network devices වල configuration එක deep ලෙස analyze කරලා security weaknesses, misconfigurations, exploitable access paths, hardening gaps සහ compliance evidence හොයන cybersecurity assessment tool එකක්.**

මම මේක **මොකක්ද → ඇතුළෙන් වැඩ කරන්නේ කොහොමද → මොනවද හොයන්නේ → practically use කරන්නේ කොහොමද → වෙනත් tools වලින් වෙනස → real example** කියලා breakdown කරන්නම්.

![Image](https://images.openai.com/static-rsc-4/pQSAbTvU92ycXZ-Ux9ekmXVwdLcdnFzwNK7Js5cLe6LJLryIiXG3lt2TwBLI_8QaYplkRLW7rixc7-lG8258TsuZP2ZKzqOz2tdICbTHjT5mKewoLLeINra6Cilm4rgD4WaihwIM4I5PlE1b8Dq5lLGnIVCyQjXfiNF-fsDOHVFXnzFHsgoSXJ5-MT_Q64dn?purpose=fullsize)

![Image](https://images.openai.com/static-rsc-4/xojEYOsetyzvzADIghWbaQzk8i9fjhm9ucFtuqjHRY698I_8_L-rkRYO_uDhXhykXOgFsGEkMCnSP7hsDf-X6fCf2SkLeU4iD0FPIMKiWnATrj8VVTcD2OxbEokDsk3yTn3FgJU_aOv7oSRiPamJPLzhe1MitWz7gIoF6qw1R12GeybrCeIasSUskO8glI9j?purpose=fullsize)

![Image](https://images.openai.com/static-rsc-4/gGcOKoM28-wHZqKuNKlJKf_RmgDsdNnhL48PyHKrxFFpnBcWlHmeq4rUV7tOODfp3ufs2QZ_DWs8OVeXSJFI-kNpZeqPGPbSoQY7iazIS3Sv2pxMri6SM92kxfbv4azCC-FYAfIdAA_mD5MgYG-hQNRNYYRdumzD6AwQ7EYw3bygbBQOsns04rf4tjau3dI8?purpose=fullsize)





![Image](https://images.openai.com/static-rsc-4/AbZt2HD2gNqBAC7FszrdpAQyfE1SdA7xUVLshhF0K3pcbWanN6FZmXNFynt0ehBa2MsaQeRYy34VtASwovO49aJ5vHkxHnLSLmm4zzLyfRWfE07zq-oMENdT5SL2zzLc74sTqX9g7UL8AVkeUjMH8UoucMcnzoVyMxLDwyROa-Q7QZfeB2NmA3BLcnrwkURB?purpose=fullsize)

## 1. මුලින්ම — Nipper InfraSight කියන්නේ මොකක්ද?

**Nipper InfraSight** කියන්නේ Titania company එකේ network-security configuration assessment platform එකක්.

එහි primary target එක:

- Routers
    
- Switches
    
- Firewalls
    
- Wireless Access Points
    
- SD-WAN/network devices
    

වගේ devices.

Titania කියන විදිහට Nipper InfraSight එක **180+ network devices** support කරනවා, Cisco, Palo Alto, Fortinet, Juniper, Check Point, Aruba, Sophos, Huawei, F5 වගේ vendors ඇතුළුව. ([Titania](https://www.titania.com/nipper-infrasight?utm_source=chatgpt.com "Nipper InfraSight"))

වැදගත්ම concept එක මේකයි:

> **Device එකට attack එකක් කරනවා වෙනුවට, device එකේ configuration එක අරගෙන "මේ configuration එක ඇත්තටම enforce කරන්නේ මොකක්ද?" කියලා analyze කරනවා.**

---

# 2. සාමාන්‍යයෙන් අපි හිතන vulnerability scanner එකයි මේකයි අතර වෙනස

උදාහරණයක් ගමු.

ඔයාගේ company එකේ firewall එකක් තියෙනවා.

```text
Internet
    |
    v
[ Firewall ]
    |
    +-------- Web Server
    |
    +-------- Database
    |
    +-------- Internal Network
```

සාමාන්‍ය vulnerability scanner එකක් server එකට scan කරලා:

```text
Port 22 open
Port 80 open
Old TLS version
CVE-XXXX
```

වගේ දේවල් හොයනවා.

**Nipper InfraSight එක වෙනස් angle එකකින් බලනවා.**

එය බලන්නේ:

```text
Firewall configuration
        ↓
ACL / Rules
NAT
Interfaces
Routing
VPN
Authentication
Services
Management access
Segmentation
Logging
Security settings
        ↓
"මේ device එක ඇත්තටම enforce කරන්නේ මොකක්ද?"
        ↓
Security weaknesses
```

ඒක නිසා මේක **configuration-centric security assessment** එකක්.

Titania official description එකේත් exported configurations වලින් device behavior එකේ virtual model එකක් build කරලා exploitable conditions/gaps test කරන බව කියනවා. ([Titania](https://www.titania.com/nipper-infrasight?utm_source=chatgpt.com "Nipper InfraSight"))

---

# 3. මෙහි තියෙන biggest advantage එක

### Live device එක touch කරන්න අවශ්‍ය නෑ.

උදාහරණයක්:

Cisco firewall/router එකක configuration එක:

```text
show running-config
```

වගේ command එකකින් export කරගත්තා කියමු.

ඊට පස්සේ:

```text
Cisco_Config.txt
        ↓
Nipper InfraSight
        ↓
Configuration analysis
        ↓
Security findings
        ↓
Risk prioritization
        ↓
Remediation instructions
        ↓
Report
```

ඒ කියන්නේ production firewall එකට scanner එකක් active ලෙස attack/probe කරන්න ඕනෙ නෑ.

Titania specifically කියන්නේ **exported configurations** use කරලා analysis කරන නිසා credentials නැතිව සහ live device එකට changes නොකර assessment කරන්න පුළුවන් කියලා. ([Titania](https://www.titania.com/nipper-infrasight?utm_source=chatgpt.com "Nipper InfraSight"))

---

# 4. එහෙනම් configuration එකෙන් මොනවද හොයන්නේ?

මේක තමයි වැදගත්ම කොටස.

## A. Misconfiguration

උදාහරණයක්:

```text
Firewall Rule #15

Source: ANY
Destination: ANY
Service: ANY
Action: ALLOW
```

මෙවැනි rule එකක් තිබුණොත් huge exposure එකක් වෙන්න පුළුවන්.

Nipperට configuration logic එක analyze කරලා මේ වගේ weak rules identify කරන්න පුළුවන්.

---

## B. Weak access control

උදාහරණයක්:

```text
Management interface
        ↓
SSH
        ↓
Source = ANY
```

එහෙම නම් management access එක unnecessarily broad වෙන්න පුළුවන්.

Nipper එක configuration context එකෙන් මේ වගේ weak access controls හඳුනාගන්න පුළුවන්. ([Titania](https://www.titania.com/nipper-infrasight?utm_source=chatgpt.com "Nipper InfraSight"))

---

# 5. Network segmentation problems

මේක Nipper එකේ interesting part එකක්.

හිතන්න:

```text
Internet
   |
Firewall
   |
   +---- DMZ
   |
   +---- Users
   |
   +---- Finance
   |
   +---- Database
```

Company එකේ intention එක:

```text
Internet → DMZ       YES
Internet → Database  NO

Users → Database     LIMITED
Finance → Database   YES
Guest → Finance      NO
```

නමුත් firewall rules වල mistake එකක් නිසා:

```text
Guest → Finance → Database
```

path එක theoretically open වෙලා තියෙන්න පුළුවන්.

Nipper configuration එක analyze කරලා **device configuration එකෙන් enforce වෙන network behavior** ගැන insight දෙන්න පුළුවන්.

Titania මේකට **behavior-based virtual modeling** කියන concept එක භාවිතා කරනවා. ([Titania](https://www.titania.com/nipper-infrasight?utm_source=chatgpt.com "Nipper InfraSight"))

---

# 6. Firewall rules ගැන විශේෂයෙන් useful

Firewall එකක rules 500ක්, 1000ක්, 5000ක් තියෙන environment එකක් imagine කරන්න.

Human කෙනෙක්ට manually බලලා:

```text
Rule 1
Rule 2
Rule 3
...
Rule 1000
```

analyse කරන එක අමාරුයි.

Nipperට configuration එක parse කරලා security perspective එකෙන් assess කරන්න පුළුවන්.

උදාහරණයක්:

```text
Rule 120
Source: 10.10.0.0/16
Destination: 172.16.10.0/24
Service: ANY
Action: ACCEPT
```

Tool එකේ concern එක:

> "මෙතන unnecessarily broad access එකක් තියෙනවද?"

ඒක security risk එකක් නම් finding එකක් ලෙස report වෙන්න පුළුවන්.

---

# 7. Hardening වලටත් use කරනවා

මේක **network device hardening** සඳහා ගොඩක් useful.

උදාහරණයක් Cisco router එකක්.

Security baseline එක:

```text
✓ SSH only
✓ Telnet disabled
✓ Strong authentication
✓ Unnecessary services disabled
✓ Secure SNMP
✓ Logging enabled
✓ NTP configured
✓ Management access restricted
✓ Password policies
✓ ACL restrictions
```

Device configuration එකේ:

```text
Telnet = ENABLED
SNMP = weak
Logging = disabled
Management = ANY
```

වගේ දේවල් තිබුණොත් Nipper finding generate කරයි.

Titania කියන්නේ CIS Benchmarks ඇතුළු hardening guidance/best-practice checks support කරන බවයි. ([Titania](https://www.titania.com/nipper-infrasight?utm_source=chatgpt.com "Nipper InfraSight"))

---

# 8. Vulnerability Management එකටත් use වෙනවා

මේක important distinction එකක්.

Nipper InfraSight එක **network-device vulnerability management** සඳහා use කරනවා.

ඒක configuration එකේ exact settings වලට findings tie කරනවා.

උදාහරණයක්:

```text
Device:
Cisco Firewall

Finding:
Weak / insecure configuration

Risk:
HIGH

Why:
Specific configuration creates exposure

Evidence:
Configuration lines/settings

Fix:
Specific remediation guidance
```

ඒ නිසා security engineer කෙනෙක්ට:

> "Issue එක තියෙනවා"

කියනවාට වඩා,

> "Issue එක මේ configuration setting එක නිසා ඇතිවෙලා තියෙනවා. මේක change කරන්න."

කියලා action කරන්න පුළුවන්.

Titania තමන්ගේ vulnerability-management capability එක configuration-based vulnerability detection ලෙස describe කරනවා. ([Titania](https://www.titania.com/nipper-infrasight/vulnerability-management?utm_source=chatgpt.com "Nipper InfraSight | Capabilities | Vulnerability Management"))

---

# 9. Compliance වලටත් use කරන්න පුළුවන්

මේක enterprises වලට ඉතා වැදගත්.

උදාහරණයක්:

Company එකට:

- [[NIST Framework]]
    
- PCI DSS
    
- CMMC
    
- DISA STIG
    

වගේ frameworks follow කරන්න ඕනේ.

Nipper InfraSight Compliance tier එකේ configuration findings **control-mapped evidence** විදිහට produce කරන්න පුළුවන්. ([Titania](https://www.titania.com/resource-center/nipper-infrasight-compliance-datasheet?utm_source=chatgpt.com "Nipper InfraSight (Compliance) Datasheet - Titania"))

උදාහරණයක්:

```text
NIST Control
      ↓
Network Device Configuration
      ↓
Pass / Fail
      ↓
Evidence
      ↓
Finding
      ↓
Remediation
```

ඒක auditor කෙනෙක්ට useful.

**හැබැයි Nipper එක "ඔයා compliant" කියලා legally certify කරන්නේ නෑ.** ඒක audit evidence/support ලබාදෙන tool එකක්. Final compliance decision එක auditor/regulator ගන්නවා. ([Titania](https://www.titania.com/solutions/use-cases/audits-assessments?utm_source=chatgpt.com "Solutions | Use-Cases | Audits & Assessments - Titania"))

---

# 10. Practically use කරන workflow එක

මේක තමයි ඔයාට practically මතක තියාගන්න ඕන flow එක.

### Step 1 — Device identify කරනවා

උදා:

```text
Cisco ASA
```

### Step 2 — Configuration export කරනවා

Device එකෙන් configuration එක ලබාගන්නවා.

```text
running-config
```

හෝ vendor/device එකට අදාළ export method එක.

### Step 3 — Nipper InfraSight එකට import කරනවා

```text
Configuration
       ↓
Nipper InfraSight
```

### Step 4 — Device එක parse කරනවා

Tool එක configuration syntax එක තේරුම් ගන්නවා.

### Step 5 — Virtual behavior model එකක් build කරනවා

මේක තමයි Nipper එකේ powerful part එක.

Configuration එකෙන්:

```text
Interfaces
Routes
ACLs
Firewall rules
NAT
Services
Access controls
```

වගේ things combine කරලා device එක behave වෙන ආකාරය model කරනවා.

### Step 6 — Security assessment

ඊට පස්සේ:

```text
Misconfiguration
Hardening gap
Weak access
Vulnerability
Segmentation issue
Policy issue
```

වගේ දේවල් identify කරනවා.

### Step 7 — Risk prioritize කරනවා

හැම finding එකම එකම priority එකක් නෙවෙයි.

```text
Critical
High
Medium
Low
```

වගේ risk perspective එකෙන් prioritize කරන්න පුළුවන්.

### Step 8 — Fix කරනවා

Tool එක device-specific remediation guidance දෙන්න පුළුවන්.

උදා:

```text
Finding
   ↓
Why dangerous
   ↓
Affected configuration
   ↓
Recommended fix
```

Titania specifically device-specific CLI commands/guidance ලබාදෙන බව සඳහන් කරනවා. ([Titania](https://www.titania.com/try/trial?utm_source=chatgpt.com "Request a Trial | Titania"))

### Step 9 — නැවත assessment

Fix කළාට පස්සේ configuration එක නැවත export කරලා:

```text
Before
  ↓
Finding
  ↓
Fix
  ↓
New Config
  ↓
Nipper
  ↓
Finding resolved?
```

කියලා verify කරන්න පුළුවන්.

---

# 11. Example එකක් — company firewall එකක්

හිතමු company එකේ FortiGate firewall එකක් තියෙනවා.

Configuration:

```text
Internet
    |
FortiGate
    |
    +---- Web Server
    |
    +---- Employee LAN
    |
    +---- Database
```

Nipperට config එක දුන්නා.

එයා findings 5ක් හොයාගත්තා කියමු:

```text
1. HIGH
Management access too broad

2. HIGH
Overly permissive firewall rule

3. MEDIUM
Weak service configuration

4. MEDIUM
Hardening baseline mismatch

5. LOW
Logging configuration issue
```

එතකොට security engineerට:

```text
HIGH
 ↓
Fix first

MEDIUM
 ↓
Next

LOW
 ↓
Later
```

කියලා prioritize කරන්න පුළුවන්.

මේක තමයි **risk-based vulnerability management** කියන්නේ.

---

# 12. Nipper vs Nessus වගේ tool එකක්

මේ distinction එක තේරුම් ගත්තොත් Nipper එක තේරෙනවා.

|Tool type|Main focus|
|---|---|
|Nessus-style vulnerability scanner|Hosts/services/vulnerabilities|
|Nmap|Network discovery/ports/services|
|SIEM|Logs/events|
|EDR|Endpoint behavior|
|Firewall|Traffic enforcement|
|**Nipper InfraSight**|**Network-device configuration/security assurance**|

ඒ කියන්නේ Nipper කියන්නේ **Nessus replace කරන්න හදපු එකක් නෙවෙයි.**

ඒවා එකට use කරන්න පුළුවන්.

උදා:

```text
Nmap
  ↓
Network discovery

Nessus
  ↓
Host vulnerability assessment

EDR
  ↓
Endpoint behavior

SIEM
  ↓
Security events

Nipper
  ↓
Router / Switch / Firewall configuration assurance
```

ඒක තමයි complete security stack එකක Nipperගේ position එක.

---

# 13. Nipper එකේ "offline" capability එක ඇයි වැදගත්?

මේක military / government / critical infrastructure environments වල ගොඩක් valuable.

උදාහරණයක්:

```text
Classified Network

     NO INTERNET
          |
          |
    [Network Devices]
          |
       Config
          ↓
   Offline Nipper
          ↓
     Assessment
```

Configuration data එක cloud එකකට upload කරන්න බැරි environment එකක් වෙන්න පුළුවන්.

Nipper InfraSight **Air Gapped** version එක isolated/classified/OT environments වල offline operation සඳහා design කරලා තියෙනවා. ([Titania](https://www.titania.com/resource-center/nipper-infrasight-air-gapped-datasheet?utm_source=chatgpt.com "Nipper InfraSight (Air gapped) Datasheet - Titania"))

---

# 14. Nipper InfraSight tiers

දැනට Titania describe කරන major options:

### **Nipper InfraSight — Essential**

Basic point-in-time:

- Configuration assessment
    
- Hardening
    
- Prioritized findings
    
- Remediation
    
- Reporting
    

([Titania](https://www.titania.com/nipper-infrasight/network-hardening?utm_source=chatgpt.com "Nipper InfraSight | Capabilities | Network Hardening"))

### **Nipper InfraSight — Compliance**

ඊට අමතරව:

- Control mapping
    
- Audit evidence
    
- CMMC
    
- NIST
    
- PCI DSS
    
- DISA STIG-related assessment
    

වගේ compliance-oriented capabilities. ([Titania](https://www.titania.com/resource-center/nipper-infrasight-compliance-datasheet?utm_source=chatgpt.com "Nipper InfraSight (Compliance) Datasheet - Titania"))

### **Nipper InfraSight — Air Gapped**

Offline / isolated environments සඳහා.

([Titania](https://www.titania.com/resource-center/nipper-infrasight-air-gapped-datasheet?utm_source=chatgpt.com "Nipper InfraSight (Air gapped) Datasheet - Titania"))

---

# 15. මේක install කරලා use කරන්න පුළුවන්ද?

ඔව්.

Current documentation අනුව Nipper InfraSight Windows සහ Ubuntu environments සඳහා available. Current v4.0.0 release එක Windows 11 සහ Ubuntu 22.04.5 tested platforms ලෙස සඳහන් කරනවා. ([Titania Docs](https://docs.titania.com/nipper-infrasight/release-notes/v4.0.0?utm_source=chatgpt.com "Nipper InfraSight v4.0.0"))

Titania official documentation එකේ installation, licensing, offline activation, first report creation වගේ user guides තියෙනවා. ([Titania Docs](https://docs.titania.com/nipper-infrasight/user-guides?utm_source=chatgpt.com "User Guides | Nipper InfraSight"))

[Titania Nipper InfraSight official page](https://www.titania.com/nipper-infrasight?utm_source=chatgpt.com)  
[Nipper InfraSight User Guides](https://docs.titania.com/nipper-infrasight/user-guides?utm_source=chatgpt.com)  
[Nipper InfraSight free trial](https://www.titania.com/try/trial?utm_source=chatgpt.com)


### මෙය Install කරන්නේ කොහේද? (ප්‍රධාන නීතිය)

- ❌ **Main Server එකේ හෝ සාමාන්‍ය සේවකයෙකුගේ PC එකක Install කරන්න එපා.**
    
- ✅ **IT හෝ Security අංශයේ වෙනම වෙන් කළ පරිගණකයක (Dedicated Workstation/Laptop) පමණක් Install කරන්න.**
    

### 3. වැඩ කරන ක්‍රමය (Workflow එක) කොහොමද?

මෙය ජාලය පුරා ස්වයංක්‍රීයව දත්ත මංකොල්ලකන (Scanner) tool එකක් නොවේ. ක්‍රමය ඉතා සරලයි:

1. **Router/Firewall එකෙන්** එහි සැකසුම් ගොනුව (Configuration File) ඩවුන්लोड කරගන්න.
    
2. ඒ ගොනුව **Security Workstation එකට** දමාගෙන, **Nipper** මෘදුකාංගයෙන් විවෘත කරන්න.
    
3. එය පරීක්ෂා කර ආරක්ෂක වාර්තාවක් (Report) ලබා දෙයි.
    

### 4. අන්තර්ජාලය අවශ්‍යද? (Offline පාවිච්චි කළ හැකිද?)

- **අන්තර්ජාලය අවශ්‍ය නැත.** සම්පූර්ණයෙන්ම අන්තර්ජාලය නොමැති (Isolated) පරිසරයක වුණත් මේක පාවිච්චි කරන්න පුළුවන්.
    
- බලපත්‍රය (License) සක්‍රීය කරගැනීමට පමණක් අන්තර්ජාලය ඇති වෙනත් පරිගණකයක් හරහා USB එකක් මඟින් එය කරගත හැක.
    

### කෙටි සාරාංශය (Quick Summary)

> **Production Server එකට අත තබන්න එපා.**
> 
> **Company එකේ ආරක්ෂාව සහ Settings පරීක්ෂා කිරීමට, වෙනම වෙන් කළ (Dedicated) PC එකක Nipper දමා, අවශ්‍ය Network Config Files පමණක් ගෙන විමර්ශනය කරන්න.**

# 16. මට නම් Nipper එකේ core concept එක මෙහෙමයි

ඔයා මේ diagram එක මතක තියාගන්න:

```text
             NETWORK DEVICE
                   │
                   │
          Export Configuration
                   │
                   ▼
          ┌─────────────────┐
          │ Nipper InfraSight│
          └────────┬────────┘
                   │
                   ▼
       Parse Configuration
                   │
                   ▼
      Build Behavior Model
                   │
                   ▼
        Security Assessment
                   │
       ┌───────────┼───────────┐
       ▼           ▼           ▼
   Vulnerability Misconfig   Hardening
       │           │           │
       └───────────┼───────────┘
                   ▼
             Risk Priority
                   │
                   ▼
          Remediation Guidance
                   │
                   ▼
             Fix Device
                   │
                   ▼
          Export New Config
                   │
                   ▼
             Re-assess
                   │
                   ▼
          Evidence / Report
```

**ඒක තමයි Nipper InfraSight එකේ heart එක.**

---

## 17. තවත් වැදගත් point එකක්

**Nipper InfraSight = "hack the network" tool එකක් නෙවෙයි.**

ඒක mainly:

> **"Network device එක configure කරලා තියෙන විදිහ අනුව, එය security-wise ඇත්තටම enforce කරන්නේ මොනවාද?"**

කියන ප්‍රශ්නයට answer එකක් දෙන්න හදන tool එකක්.

ඒ නිසා penetration testing එකක් වෙනුවට/ඒකට අමතරව **configuration assurance** කියන layer එක cover කරනවා.

Titania කියන විදිහට Nipper technology එක penetration testers විසින්ත් භාවිතා කරපු background එකක් තියෙනවා, නමුත් current InfraSight product positioning එක point-in-time hardening/compliance/configuration assessment එකට focus වෙනවා. ([Titania](https://www.titania.com/?utm_source=chatgpt.com "Proactive network security solutions | Titania"))

>>>>>>> a159bce7962d6af920654f254fefef8e27f23cd4
