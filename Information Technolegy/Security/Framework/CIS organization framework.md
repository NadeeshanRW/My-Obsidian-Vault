<<<<<<< HEAD

 **CIS** කියන්නේ cybersecurity වල [[NIST Framework|NIST Framework]] එක්ක නිතරම එකට අහන්න ලැබෙන වැදගත් organization/framework එකක්. ඔයා Nipper InfraSight ඉගෙන ගන්න නිසා **CIS Controls** සහ **CIS Benchmarks** දෙකම තේරුම් ගන්න එක විශේෂයෙන් වැදගත්.

## 1. CIS කියන්නේ මොකක්ද?

**CIS = Center for Internet Security**

Official site එක:

[Center for Internet Security (CIS)](https://www.cisecurity.org/?utm_source=chatgpt.com)

CIS එක cybersecurity community එකට:

- Security controls
    
- Secure configuration benchmarks
    
- Hardening guidelines
    
- Security tools/resources
    
- Implementation guidance
    

වගේ දේවල් ලබාදෙන nonprofit organization එකක්.

සරලව:

> **[[NIST Framework]] කියන්නේ cybersecurity risk manage කරන්න framework/standards/guidance දෙන source එකක්. CIS කියන්නේ practical security controls සහ systems harden කරන්න benchmark/guidance දෙන ප්‍රධාන source එකක්.**

---

# 2. CIS කියද්දි දෙයක් දෙකක් මතක තියාගන්න

මේක තමයි මුලින්ම වැදගත්.

### CIS Controls

**"Company එක cybersecurity වලදී මොන security controls implement කරන්න ඕනෙද?"**

කියන පැත්ත.

### CIS Benchmarks

**"Computer/server/network device එක secure විදිහට configure කරන්නේ කොහොමද?"**

කියන පැත්ත.

```text
                 CIS
                  │
        ┌─────────┴─────────┐
        │                   │
   CIS Controls        CIS Benchmarks
        │                   │
        ▼                   ▼
  What should we      How should we
      protect?          configure it?
```

මේ දෙක confuse කරන්න එපා.

---

# 3. CIS Controls කියන්නේ මොකක්ද?

**CIS Controls** කියන්නේ organizations වල cybersecurity risk අඩු කරන්න prioritize කරපු set එකක්.

Current major version එක:

### **CIS Controls v8.1**

Official:

[CIS Controls v8.1](https://www.cisecurity.org/controls/v8-1?utm_source=chatgpt.com)

මේකේ controls 18ක් තියෙනවා.

ඒවා company එකේ cybersecurity program එක structure කරන්න use කරන්න පුළුවන්.

---

# 4. CIS Controls 18

සරලව මේවා බලමු.

### 1. Inventory and Control of Enterprise Assets

Company එකට අයිති devices මොනවද?

```text
Laptop
Server
Router
Firewall
Switch
Cloud resource
```

ඔයා නොදන්න device එකක් network එකේ තිබුණොත් ඒක security risk එකක්.

---

### 2. Inventory and Control of Software Assets

Company computers වල run වෙන software මොනවද?

```text
Windows
Chrome
Office
Python
VPN software
Unknown applications
```

Unknown/unauthorized software identify කරන්න.

---

### 3. Data Protection

Data protect කරන්න.

```text
Encryption
Access control
Data classification
Secure storage
```

---

### 4. Secure Configuration of Enterprise Assets and Software

🔥 **[[Nipper Tool (Nipper InfraSight - Titania)]] එකට ගොඩක් relevant.**

Devices/software secure configuration baseline එකකට configure කරන්න.

උදා:

```text
Router
 ↓
Secure configuration
```

```text
Firewall
 ↓
Secure configuration
```

```text
Server
 ↓
Secure configuration
```

---

### 5. Account Management

User accounts manage කරන්න.

```text
Who has account?
Who needs it?
Who doesn't?
```

Old employee account එක active නම් risk.

---

### 6. Access Control Management

User කෙනෙක්ට access දෙන්න ඕනේ **need-to-know / least privilege** principle එකට.

```text
Employee
    ↓
Only required resources
```

---

### 7. Continuous Vulnerability Management

Systems වල vulnerabilities continuously identify කරලා fix කරන්න.

```text
Discover
 ↓
Scan
 ↓
Prioritize
 ↓
Patch
 ↓
Verify
```

Nessus වගේ vulnerability scanners මෙතන relevant.

---

### 8. Audit Log Management

Logs collect/manage කරන්න.

```text
Firewall
Server
AD
Endpoint
Application
   ↓
Logs
   ↓
SIEM
```

---

### 9. Email and Web Browser Protections

Phishing, malicious websites, email attacks වගේ දේවල් reduce කරන්න.

---

### 10. Malware Defenses

Malware detect/prevent කරන්න.

```text
Antivirus
EDR
Application controls
```

---

### 11. Data Recovery

Backup සහ recovery.

```text
Data
 ↓
Backup
 ↓
Incident
 ↓
Restore
```

---

### 12. Network Infrastructure Management

Network devices manage කරන්න.

```text
Routers
Switches
Firewalls
Wireless
VPN
```

🔥 **Nipper වලටත් directly relevant.**

---

### 13. Network Monitoring and Defense

Network එක monitor කරලා suspicious activity detect කරන්න.

```text
Network
 ↓
Monitoring
 ↓
Detection
 ↓
Alert
```

---

### 14. Security Awareness and Skills Training

Employees security ගැන train කරන්න.

Phishing awareness වගේ දේවල්.

---

### 15. Service Provider Management

Third-party vendors/security risks manage කරන්න.

උදා:

```text
Company
   ↓
Cloud Provider
   ↓
SaaS
   ↓
Managed Service
```

Third-party risk එකත් company risk එකක්.

---

### 16. Application Software Security

Applications secure කරන්න.

```text
Secure SDLC
Code security
Vulnerability testing
Application controls
```

---

### 17. Incident Response Management

Incident එකක් වුණාම:

```text
Detect
 ↓
Analyze
 ↓
Contain
 ↓
Eradicate
 ↓
Recover
```

---

### 18. Penetration Testing

Realistic attack simulations කරලා weaknesses test කරන්න.

```text
Attacker mindset
       ↓
Test defenses
       ↓
Find weaknesses
       ↓
Fix
```

---

# 5. CIS Controls සහ NIST CSF අතර වෙනස

මේක ඔයාට ඉතා වැදගත්.

### NIST CSF

High-level framework.

```text
GOVERN
IDENTIFY
PROTECT
DETECT
RESPOND
RECOVER
```

Company එකේ cybersecurity program එක **overall structure** කරන්න.

### CIS Controls

Prioritized security safeguards.

```text
Asset Inventory
Software Inventory
Configuration
Access Control
Vulnerability Management
Logging
Backups
Network Defense
...
```

Company එකට **practically කරන්න ඕනේ security activities** identify කරන්න.

ඒ නිසා:

```text
NIST CSF
   ↓
"How do we manage cybersecurity risk?"

CIS Controls
   ↓
"What prioritized security safeguards should we implement?"
```

---

# 6. දැන් CIS Benchmarks

මේක තමයි **Nipper InfraSight එක්ක තවත් directly relevant** part එක.

CIS Benchmarks කියන්නේ systems/devices securely configure කරන්න **prescriptive configuration recommendations**.

Official:

[CIS Benchmarks](https://www.cisecurity.org/cis-benchmarks?utm_source=chatgpt.com)

උදාහරණයක්:

Company එකේ:

```text
Windows Server
```

තියෙනවා.

CIS Benchmark එක බලලා:

```text
Password policy
Audit policy
Network settings
Security settings
Services
Registry settings
```

වගේ configuration recommendations check කරන්න පුළුවන්.

---

# 7. Network devices සඳහාත් CIS Benchmarks තියෙනවා

ඔයාගේ Nipper context එකට මේක වැදගත්.

උදා:

```text
Cisco IOS
Cisco ASA
Fortinet
Palo Alto
Juniper
```

වගේ platforms සඳහා relevant CIS Benchmarks තියෙන්න පුළුවන්.

Concept එක:

```text
Device
   ↓
Current Configuration
   ↓
Compare against
   ↓
CIS Benchmark
   ↓
PASS / FAIL
```

---

# 8. Example එකක්

හිතමු Cisco router එකක් තියෙනවා.

Current configuration:

```text
Telnet enabled
SSH weakly configured
Unused service enabled
Logging not configured
Management access broad
```

CIS Benchmark එකේ recommended secure configuration එකට compare කළා.

Result:

```text
Finding #1
Telnet
FAIL

Finding #2
Management access
FAIL

Finding #3
Logging
FAIL
```

ඊට පස්සේ:

```text
Remediate
    ↓
Change configuration
    ↓
Re-test
    ↓
PASS
```

---

# 9. මෙතන Nipper InfraSight එක කොහෙද?

දැන් අපි මේක තුනම connect කරමු.

```text
                    SECURITY PROGRAM
                           │
             ┌─────────────┴─────────────┐
             │                           │
         NIST CSF                   CIS Controls
             │                           │
       Overall framework          Prioritized safeguards
             │                           │
             └─────────────┬─────────────┘
                           │
                           ▼
                    Actual controls
                           │
                           ▼
                  Network devices
                           │
                           ▼
                 CIS Benchmarks
                           │
                           ▼
                  Nipper InfraSight
                           │
                           ▼
                 Configuration Audit
                           │
             ┌─────────────┴─────────────┐
             ▼                           ▼
          Findings                   Evidence
             │                           │
             ▼                           ▼
        Remediation                  Compliance
```

**මේක තමයි ඔයා ඉගෙනගන්න ඕන full picture එක.**

---

# 10. CIS Benchmark එක කියන්නේ law එකක්ද?

නෑ.

CIS Benchmark එක සාමාන්‍යයෙන්:

> **security hardening recommendation / best-practice configuration guidance**

එකක්.

ඒක automatically:

> "මේක follow නොකළොත් company එක illegal."

කියන law එකක් නෙවෙයි.

Company එකේ environment එක අනුව සමහර settings වෙනස් වෙන්න පුළුවන්.

ඒ නිසා benchmark එක apply කරනකොට:

```text
CIS Recommendation
        ↓
Business requirement
        ↓
Technical requirement
        ↓
Risk analysis
        ↓
Approved configuration
```

කියන process එක වැදගත්.

---

# 11. CIS Implementation Groups

CIS Controls වල **Implementation Groups (IGs)** තියෙනවා.

මේක company එකේ maturity/size/risk අනුව controls prioritize කරන්න උදව් කරනවා.

### IG1

Basic cyber hygiene.

Small organizations / basic protection.

### IG2

More mature security program.

More complex environments.

### IG3

Advanced/high-risk environments.

Critical assets / sophisticated threats.

Concept එක:

```text
IG1
 ↓
Basic security hygiene

IG2
 ↓
More advanced controls

IG3
 ↓
Advanced enterprise security
```

ඒ නිසා small company එකක් **CIS Controls 18ම එකවර full implementation කරන්න** ඕනේ කියලා හිතන්න එපා.

---

# 12. CIS එකෙන් company එකක් practically කරන්නේ මොනවද?

උදා company එකක්:

### Step 1

Assets inventory:

```text
100 PCs
20 Servers
10 Switches
5 Firewalls
3 Routers
```

### Step 2

Software inventory.

### Step 3

Secure configuration baseline.

### Step 4

User/access management.

### Step 5

Vulnerability management.

### Step 6

Logging.

### Step 7

Backup.

### Step 8

Network monitoring.

### Step 9

Incident response.

### Step 10

Penetration testing.

මේවා CIS Controls framework එකෙන් systematically organize කරන්න පුළුවන්.

---

# 13. Nipper + CIS එකේ real-world example

හිතමු company එකේ:

**FortiGate Firewall**

තියෙනවා.

Security team එකට requirement එක:

> "Network infrastructure securely configured වෙන්න ඕනේ."

CIS Controls වල **Control 4 — Secure Configuration of Enterprise Assets and Software** සහ **Control 12 — Network Infrastructure Management** වගේ areas relevant වෙනවා.

ඊට පස්සේ firewall configuration එක:

```text
FortiGate
   ↓
Export config
   ↓
Nipper InfraSight
   ↓
Analyze
```

Nipper:

```text
Configuration issue
        ↓
Risk
        ↓
Evidence
        ↓
Remediation
```

ඊට පස්සේ security team එක:

```text
CIS Benchmark / Organization baseline
             ↓
        Configuration
             ↓
           PASS?
```

කියලා verify කරනවා.

---

# 14. NIST + CIS + Nipper එකම project එකක

Company එකක security audit එකක් imagine කරන්න.

### Management level

**NIST CSF**

> අපේ cybersecurity risk management program එක කොහොමද?

### Security operations level

**CIS Controls**

> අපි implement කරන්න ඕනේ prioritized safeguards මොනවද?

### Technical hardening level

**CIS Benchmarks**

> Device/server එක secure configure කරන්න ඕනේ කොහොමද?

### Assessment tool

**Nipper InfraSight**

> මේ network device එකේ actual configuration එක ඒ security requirements වලට match වෙනවද?

ඒක මෙහෙම මතක තියාගන්න:

```text
             NIST
              │
       Security Strategy
              │
              ▼
             CIS
              │
      Security Safeguards
              │
              ▼
       CIS Benchmarks
              │
      Secure Configuration
              │
              ▼
           NIPPER
              │
       Technical Assessment
              │
              ▼
       FINDINGS + EVIDENCE
              │
              ▼
         REMEDIATION
```

**මේ hierarchy එක තේරුම් ගත්තොත් [[NIST Framework]], CIS, Nipper තුන එකිනෙකට compete කරන tools නෙවෙයි කියලා පැහැදිලි වෙනවා. ඒවා security program එකේ වෙනස් layers.**
=======

 **CIS** කියන්නේ cybersecurity වල [[NIST Framework|NIST Framework]] එක්ක නිතරම එකට අහන්න ලැබෙන වැදගත් organization/framework එකක්. ඔයා Nipper InfraSight ඉගෙන ගන්න නිසා **CIS Controls** සහ **CIS Benchmarks** දෙකම තේරුම් ගන්න එක විශේෂයෙන් වැදගත්.

## 1. CIS කියන්නේ මොකක්ද?

**CIS = Center for Internet Security**

Official site එක:

[Center for Internet Security (CIS)](https://www.cisecurity.org/?utm_source=chatgpt.com)

CIS එක cybersecurity community එකට:

- Security controls
    
- Secure configuration benchmarks
    
- Hardening guidelines
    
- Security tools/resources
    
- Implementation guidance
    

වගේ දේවල් ලබාදෙන nonprofit organization එකක්.

සරලව:

> **[[NIST Framework]] කියන්නේ cybersecurity risk manage කරන්න framework/standards/guidance දෙන source එකක්. CIS කියන්නේ practical security controls සහ systems harden කරන්න benchmark/guidance දෙන ප්‍රධාන source එකක්.**

---

# 2. CIS කියද්දි දෙයක් දෙකක් මතක තියාගන්න

මේක තමයි මුලින්ම වැදගත්.

### CIS Controls

**"Company එක cybersecurity වලදී මොන security controls implement කරන්න ඕනෙද?"**

කියන පැත්ත.

### CIS Benchmarks

**"Computer/server/network device එක secure විදිහට configure කරන්නේ කොහොමද?"**

කියන පැත්ත.

```text
                 CIS
                  │
        ┌─────────┴─────────┐
        │                   │
   CIS Controls        CIS Benchmarks
        │                   │
        ▼                   ▼
  What should we      How should we
      protect?          configure it?
```

මේ දෙක confuse කරන්න එපා.

---

# 3. CIS Controls කියන්නේ මොකක්ද?

**CIS Controls** කියන්නේ organizations වල cybersecurity risk අඩු කරන්න prioritize කරපු set එකක්.

Current major version එක:

### **CIS Controls v8.1**

Official:

[CIS Controls v8.1](https://www.cisecurity.org/controls/v8-1?utm_source=chatgpt.com)

මේකේ controls 18ක් තියෙනවා.

ඒවා company එකේ cybersecurity program එක structure කරන්න use කරන්න පුළුවන්.

---

# 4. CIS Controls 18

සරලව මේවා බලමු.

### 1. Inventory and Control of Enterprise Assets

Company එකට අයිති devices මොනවද?

```text
Laptop
Server
Router
Firewall
Switch
Cloud resource
```

ඔයා නොදන්න device එකක් network එකේ තිබුණොත් ඒක security risk එකක්.

---

### 2. Inventory and Control of Software Assets

Company computers වල run වෙන software මොනවද?

```text
Windows
Chrome
Office
Python
VPN software
Unknown applications
```

Unknown/unauthorized software identify කරන්න.

---

### 3. Data Protection

Data protect කරන්න.

```text
Encryption
Access control
Data classification
Secure storage
```

---

### 4. Secure Configuration of Enterprise Assets and Software

🔥 **[[Nipper Tool (Nipper InfraSight - Titania)]] එකට ගොඩක් relevant.**

Devices/software secure configuration baseline එකකට configure කරන්න.

උදා:

```text
Router
 ↓
Secure configuration
```

```text
Firewall
 ↓
Secure configuration
```

```text
Server
 ↓
Secure configuration
```

---

### 5. Account Management

User accounts manage කරන්න.

```text
Who has account?
Who needs it?
Who doesn't?
```

Old employee account එක active නම් risk.

---

### 6. Access Control Management

User කෙනෙක්ට access දෙන්න ඕනේ **need-to-know / least privilege** principle එකට.

```text
Employee
    ↓
Only required resources
```

---

### 7. Continuous Vulnerability Management

Systems වල vulnerabilities continuously identify කරලා fix කරන්න.

```text
Discover
 ↓
Scan
 ↓
Prioritize
 ↓
Patch
 ↓
Verify
```

Nessus වගේ vulnerability scanners මෙතන relevant.

---

### 8. Audit Log Management

Logs collect/manage කරන්න.

```text
Firewall
Server
AD
Endpoint
Application
   ↓
Logs
   ↓
SIEM
```

---

### 9. Email and Web Browser Protections

Phishing, malicious websites, email attacks වගේ දේවල් reduce කරන්න.

---

### 10. Malware Defenses

Malware detect/prevent කරන්න.

```text
Antivirus
EDR
Application controls
```

---

### 11. Data Recovery

Backup සහ recovery.

```text
Data
 ↓
Backup
 ↓
Incident
 ↓
Restore
```

---

### 12. Network Infrastructure Management

Network devices manage කරන්න.

```text
Routers
Switches
Firewalls
Wireless
VPN
```

🔥 **Nipper වලටත් directly relevant.**

---

### 13. Network Monitoring and Defense

Network එක monitor කරලා suspicious activity detect කරන්න.

```text
Network
 ↓
Monitoring
 ↓
Detection
 ↓
Alert
```

---

### 14. Security Awareness and Skills Training

Employees security ගැන train කරන්න.

Phishing awareness වගේ දේවල්.

---

### 15. Service Provider Management

Third-party vendors/security risks manage කරන්න.

උදා:

```text
Company
   ↓
Cloud Provider
   ↓
SaaS
   ↓
Managed Service
```

Third-party risk එකත් company risk එකක්.

---

### 16. Application Software Security

Applications secure කරන්න.

```text
Secure SDLC
Code security
Vulnerability testing
Application controls
```

---

### 17. Incident Response Management

Incident එකක් වුණාම:

```text
Detect
 ↓
Analyze
 ↓
Contain
 ↓
Eradicate
 ↓
Recover
```

---

### 18. Penetration Testing

Realistic attack simulations කරලා weaknesses test කරන්න.

```text
Attacker mindset
       ↓
Test defenses
       ↓
Find weaknesses
       ↓
Fix
```

---

# 5. CIS Controls සහ NIST CSF අතර වෙනස

මේක ඔයාට ඉතා වැදගත්.

### NIST CSF

High-level framework.

```text
GOVERN
IDENTIFY
PROTECT
DETECT
RESPOND
RECOVER
```

Company එකේ cybersecurity program එක **overall structure** කරන්න.

### CIS Controls

Prioritized security safeguards.

```text
Asset Inventory
Software Inventory
Configuration
Access Control
Vulnerability Management
Logging
Backups
Network Defense
...
```

Company එකට **practically කරන්න ඕනේ security activities** identify කරන්න.

ඒ නිසා:

```text
NIST CSF
   ↓
"How do we manage cybersecurity risk?"

CIS Controls
   ↓
"What prioritized security safeguards should we implement?"
```

---

# 6. දැන් CIS Benchmarks

මේක තමයි **Nipper InfraSight එක්ක තවත් directly relevant** part එක.

CIS Benchmarks කියන්නේ systems/devices securely configure කරන්න **prescriptive configuration recommendations**.

Official:

[CIS Benchmarks](https://www.cisecurity.org/cis-benchmarks?utm_source=chatgpt.com)

උදාහරණයක්:

Company එකේ:

```text
Windows Server
```

තියෙනවා.

CIS Benchmark එක බලලා:

```text
Password policy
Audit policy
Network settings
Security settings
Services
Registry settings
```

වගේ configuration recommendations check කරන්න පුළුවන්.

---

# 7. Network devices සඳහාත් CIS Benchmarks තියෙනවා

ඔයාගේ Nipper context එකට මේක වැදගත්.

උදා:

```text
Cisco IOS
Cisco ASA
Fortinet
Palo Alto
Juniper
```

වගේ platforms සඳහා relevant CIS Benchmarks තියෙන්න පුළුවන්.

Concept එක:

```text
Device
   ↓
Current Configuration
   ↓
Compare against
   ↓
CIS Benchmark
   ↓
PASS / FAIL
```

---

# 8. Example එකක්

හිතමු Cisco router එකක් තියෙනවා.

Current configuration:

```text
Telnet enabled
SSH weakly configured
Unused service enabled
Logging not configured
Management access broad
```

CIS Benchmark එකේ recommended secure configuration එකට compare කළා.

Result:

```text
Finding #1
Telnet
FAIL

Finding #2
Management access
FAIL

Finding #3
Logging
FAIL
```

ඊට පස්සේ:

```text
Remediate
    ↓
Change configuration
    ↓
Re-test
    ↓
PASS
```

---

# 9. මෙතන Nipper InfraSight එක කොහෙද?

දැන් අපි මේක තුනම connect කරමු.

```text
                    SECURITY PROGRAM
                           │
             ┌─────────────┴─────────────┐
             │                           │
         NIST CSF                   CIS Controls
             │                           │
       Overall framework          Prioritized safeguards
             │                           │
             └─────────────┬─────────────┘
                           │
                           ▼
                    Actual controls
                           │
                           ▼
                  Network devices
                           │
                           ▼
                 CIS Benchmarks
                           │
                           ▼
                  Nipper InfraSight
                           │
                           ▼
                 Configuration Audit
                           │
             ┌─────────────┴─────────────┐
             ▼                           ▼
          Findings                   Evidence
             │                           │
             ▼                           ▼
        Remediation                  Compliance
```

**මේක තමයි ඔයා ඉගෙනගන්න ඕන full picture එක.**

---

# 10. CIS Benchmark එක කියන්නේ law එකක්ද?

නෑ.

CIS Benchmark එක සාමාන්‍යයෙන්:

> **security hardening recommendation / best-practice configuration guidance**

එකක්.

ඒක automatically:

> "මේක follow නොකළොත් company එක illegal."

කියන law එකක් නෙවෙයි.

Company එකේ environment එක අනුව සමහර settings වෙනස් වෙන්න පුළුවන්.

ඒ නිසා benchmark එක apply කරනකොට:

```text
CIS Recommendation
        ↓
Business requirement
        ↓
Technical requirement
        ↓
Risk analysis
        ↓
Approved configuration
```

කියන process එක වැදගත්.

---

# 11. CIS Implementation Groups

CIS Controls වල **Implementation Groups (IGs)** තියෙනවා.

මේක company එකේ maturity/size/risk අනුව controls prioritize කරන්න උදව් කරනවා.

### IG1

Basic cyber hygiene.

Small organizations / basic protection.

### IG2

More mature security program.

More complex environments.

### IG3

Advanced/high-risk environments.

Critical assets / sophisticated threats.

Concept එක:

```text
IG1
 ↓
Basic security hygiene

IG2
 ↓
More advanced controls

IG3
 ↓
Advanced enterprise security
```

ඒ නිසා small company එකක් **CIS Controls 18ම එකවර full implementation කරන්න** ඕනේ කියලා හිතන්න එපා.

---

# 12. CIS එකෙන් company එකක් practically කරන්නේ මොනවද?

උදා company එකක්:

### Step 1

Assets inventory:

```text
100 PCs
20 Servers
10 Switches
5 Firewalls
3 Routers
```

### Step 2

Software inventory.

### Step 3

Secure configuration baseline.

### Step 4

User/access management.

### Step 5

Vulnerability management.

### Step 6

Logging.

### Step 7

Backup.

### Step 8

Network monitoring.

### Step 9

Incident response.

### Step 10

Penetration testing.

මේවා CIS Controls framework එකෙන් systematically organize කරන්න පුළුවන්.

---

# 13. Nipper + CIS එකේ real-world example

හිතමු company එකේ:

**FortiGate Firewall**

තියෙනවා.

Security team එකට requirement එක:

> "Network infrastructure securely configured වෙන්න ඕනේ."

CIS Controls වල **Control 4 — Secure Configuration of Enterprise Assets and Software** සහ **Control 12 — Network Infrastructure Management** වගේ areas relevant වෙනවා.

ඊට පස්සේ firewall configuration එක:

```text
FortiGate
   ↓
Export config
   ↓
Nipper InfraSight
   ↓
Analyze
```

Nipper:

```text
Configuration issue
        ↓
Risk
        ↓
Evidence
        ↓
Remediation
```

ඊට පස්සේ security team එක:

```text
CIS Benchmark / Organization baseline
             ↓
        Configuration
             ↓
           PASS?
```

කියලා verify කරනවා.

---

# 14. NIST + CIS + Nipper එකම project එකක

Company එකක security audit එකක් imagine කරන්න.

### Management level

**NIST CSF**

> අපේ cybersecurity risk management program එක කොහොමද?

### Security operations level

**CIS Controls**

> අපි implement කරන්න ඕනේ prioritized safeguards මොනවද?

### Technical hardening level

**CIS Benchmarks**

> Device/server එක secure configure කරන්න ඕනේ කොහොමද?

### Assessment tool

**Nipper InfraSight**

> මේ network device එකේ actual configuration එක ඒ security requirements වලට match වෙනවද?

ඒක මෙහෙම මතක තියාගන්න:

```text
             NIST
              │
       Security Strategy
              │
              ▼
             CIS
              │
      Security Safeguards
              │
              ▼
       CIS Benchmarks
              │
      Secure Configuration
              │
              ▼
           NIPPER
              │
       Technical Assessment
              │
              ▼
       FINDINGS + EVIDENCE
              │
              ▼
         REMEDIATION
```

**මේ hierarchy එක තේරුම් ගත්තොත් [[NIST Framework]], CIS, Nipper තුන එකිනෙකට compete කරන tools නෙවෙයි කියලා පැහැදිලි වෙනවා. ඒවා security program එකේ වෙනස් layers.**
>>>>>>> a159bce7962d6af920654f254fefef8e27f23cd4
