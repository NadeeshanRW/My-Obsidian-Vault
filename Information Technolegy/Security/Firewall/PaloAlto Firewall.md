<<<<<<< HEAD
ඔව්. මේක **Palo Alto Networks Firewall** එකක් zero ඉඳන් advanced level එකට තේරුම් ගන්න විදිහට බලමු.

## 1. Palo Alto Firewall කියන්නේ මොකක්ද?

**Palo Alto Networks NGFW (Next-Generation Firewall)** කියන්නේ network එකට ඇතුල් වෙන සහ පිට වෙන traffic එක **IP/port විතරක් බලලා නොව**, application, user, device, content සහ threat context එකත් බලලා control කරන security platform එකක්.

සරල firewall එක:

```text
Source IP
    +
Destination IP
    +
Port
    ↓
ALLOW / DENY
```

Palo Alto NGFW එක:

```text
User
  +
Device
  +
Source
  +
Destination
  +
Application
  +
Content
  +
Threat
  +
Security Policy
        ↓
ALLOW / BLOCK / INSPECT
```

ඒක තමයි **Next-Generation Firewall** එකක් කියන්නේ.

---

# 2. ඇයි Palo Alto Firewall එකක් ඕනේ?

Company network එකේ devices තියෙනවා:

```text
                    INTERNET
                       │
                       ▼
                 [ FIREWALL ]
                       │
          ┌────────────┼────────────┐
          ▼            ▼            ▼
        USERS        SERVERS       DMZ
          │            │            │
       PCs/Laptops   Database     Web Server
```

Firewall එකේ ප්‍රධාන job එක:

> **කවුද → මොන application එකෙන් → කොහෙට → මොන traffic එකක් → යවන්නේද, ඒක allow කරන්නද block කරන්නද?**

උදාහරණයක්:

Employee කෙනෙක් Internet යනවා.

```text
Employee PC
     ↓
HTTPS
     ↓
Internet
```

සාමාන්‍ය firewall එක:

```text
TCP 443
    ↓
ALLOW
```

Palo Alto එකට:

```text
TCP 443
   ↓
Identify Application
   ↓
GitHub
   ↓
Identify User
   ↓
IT Department
   ↓
Check Security Policy
   ↓
Threat Inspection
   ↓
ALLOW
```

වගේ granular decision එකක් ගන්න පුළුවන්.

---

# 3. Palo Alto Architecture එක

Palo Alto firewall එක තේරුම් ගන්න මේ architecture එක මුලින්ම මතක තියාගන්න.

```text
                         INTERNET
                            │
                            ▼
                     ┌─────────────┐
                     │  PALO ALTO  │
                     │    NGFW     │
                     └──────┬──────┘
                            │
                  ┌─────────┼─────────┐
                  │         │         │
                  ▼         ▼         ▼
                 DMZ      USERS     SERVERS
                  │         │         │
                  ▼         ▼         ▼
               Web Apps   PCs      Databases
```

Firewall එක ඇතුළේ:

```text
Palo Alto NGFW
│
├── PAN-OS
│
├── Interfaces
│
├── Security Zones
│
├── Virtual Routers
│
├── Security Policies
│
├── NAT Policies
│
├── App-ID
│
├── User-ID
│
├── Device-ID
│
├── Content-ID
│
├── Security Profiles
│
├── Decryption
│
├── VPN
│
├── Logging
│
├── High Availability
│
└── Panorama
```

---

# 4. PAN-OS

**PAN-OS** කියන්නේ Palo Alto firewall එකේ operating system / software platform එක.

මේක ඇතුළේ network සහ security functions ගොඩක් integrate වෙනවා.

```text
Palo Alto Hardware
       │
       ▼
     PAN-OS
       │
 ┌─────┼─────────────┐
 ▼     ▼             ▼
Network Security   Visibility
```

PAN-OS තමයි:

- Interfaces
    
- Routing
    
- Security policies
    
- NAT
    
- VPN
    
- Application identification
    
- User identification
    
- Threat prevention
    
- Logging
    

වගේ functions manage කරන්නේ.

---

# 5. Interfaces

Firewall එකට network connections තියෙනවා.

උදා:

```text
ethernet1/1 → Internet
ethernet1/2 → Internal LAN
ethernet1/3 → DMZ
```

Architecture:

```text
Internet
   │
   ▼
ethernet1/1
   │
┌──────────────┐
│ Palo Alto    │
└──────────────┘
   │
ethernet1/2
   │
   ▼
Internal LAN
```

Palo Alto interfaces Layer 3, Layer 2, Virtual Wire, Tunnel, TAP වගේ modes වල configure කරන්න පුළුවන්.

---

# 6. Security Zones

Zone කියන්නේ security boundary එකක්.

උදා:

```text
Untrust
   ↓
Internet

Trust
   ↓
Employees

Server
   ↓
Servers

DMZ
   ↓
Public-facing servers

Management
   ↓
Management systems
```

Example:

```text
                 INTERNET
                    │
                 Untrust
                    │
                    ▼
              PALO ALTO
                    │
          ┌─────────┼─────────┐
          ▼         ▼         ▼
        DMZ       Trust      Server
```

ඊට පස්සේ security policy එකේ කියන්න පුළුවන්:

```text
Trust → Internet
ALLOW

Guest → Server
DENY

DMZ → Internal
LIMITED
```

---

# 7. Virtual Router

Palo Alto firewall එක routing කරන්නත් පුළුවන්.

Virtual Router එකේ:

- Static routes
    
- OSPF
    
- BGP
    
- Route redistribution
    
- Routing tables
    

වගේ functions manage කරන්න පුළුවන්.

උදා:

```text
                    Palo Alto
                        │
                  Virtual Router
                        │
             ┌──────────┼──────────┐
             ▼          ▼          ▼
            LAN        DMZ       Internet
```

---

# 8. Security Policy — Firewall එකේ Heart එක

මේක තමයි Palo Alto ඉගෙනගන්නකොට ගොඩක් වැදගත්.

Security policy එක roughly:

```text
Source Zone
Source Address
Source User
Destination Zone
Destination Address
Application
Service
Action
```

Example:

```text
Rule: Allow-Employees-Web

Source Zone:
Trust

Source:
Employee-Network

Destination Zone:
Untrust

Destination:
Any

Application:
web-browsing
ssl

Service:
application-default

Action:
ALLOW
```

මෙතන important difference එක:

**"Port 443 allow කරන්න"** කියන එකට වඩා

**"Approved applications සඳහා 443 traffic allow කරන්න"**

වගේ granular control එකක් ගන්න පුළුවන්.

---

# 9. App-ID 🔥

Palo Altoගේ famous technology එකක් තමයි **App-ID**.

මේක firewall එකට traffic එකේ application එක identify කරන්න උදව් කරනවා.

උදා:

```text
TCP 443
   ↓
App-ID
   ↓
YouTube
```

හෝ:

```text
TCP 443
   ↓
App-ID
   ↓
GitHub
```

ඒ නිසා policy එක:

```text
ALLOW TCP 443
```

වෙනුවට:

```text
ALLOW GitHub
BLOCK Social Media
ALLOW Microsoft 365
BLOCK Unknown Applications
```

වගේ application-aware policy හදන්න පුළුවන්.

---

# 10. User-ID

IP address එකෙන් user identity එකට map කරන්න පුළුවන්.

උදා:

```text
10.10.10.25
      ↓
Active Directory
      ↓
John
      ↓
IT Department
```

ඊට පස්සේ:

```text
IT Users
    ↓
GitHub
    ↓
ALLOW
```

වගේ policy එකක් හදන්න පුළුවන්.

ඒක IP-based firewall එකකට වඩා powerful.

---

# 11. Device-ID

Traffic එක එවන device එක ගැන context එකත් security policy එකට ගන්න පුළුවන්.

Concept:

```text
User
 +
Device
 +
Application
 +
Location
 +
Network
       ↓
Security Policy
```

මේක modern Zero Trust architectures වලදී useful.

---

# 12. Content / Threat Inspection

Traffic එක allow කළා කියලා ඒ traffic එක safe කියලා අදහස් වෙන්නේ නෑ.

උදා:

```text
User
 ↓
Website
 ↓
Download file
 ↓
Malware
```

Palo Alto security inspection technologies මගින් traffic/content එක inspect කරලා malicious activity identify/block කිරීමට හැකියාව තියෙනවා.

Common security capabilities:

```text
Antivirus
Anti-Spyware
Vulnerability Protection
URL Filtering
File Blocking
WildFire
DNS Security
Data Loss Prevention
```

---

# 13. Security Profiles

Security policy එක:

> "මේ traffic එක allow."

Security profile:

> "Allow කළ traffic එක inspect කරලා dangerous දෙයක් තිබුණොත් stop කරන්න."

Concept:

```text
Traffic
   ↓
Security Policy
   ↓
ALLOW
   ↓
Security Profiles
   ├── Antivirus
   ├── Anti-Spyware
   ├── Vulnerability Protection
   ├── URL Filtering
   └── File Blocking
   ↓
Safe?
   ↓
YES → Continue
NO  → Block
```

---

# 14. NAT

Internal private IP addresses Internet එකට directly use කරන්න බෑ.

ඒ නිසා NAT.

```text
Internal PC
192.168.1.20
      │
      ▼
Palo Alto
      │
      ▼
Public IP
203.x.x.x
      │
      ▼
Internet
```

Palo Alto වල:

- Source NAT
    
- Destination NAT
    
- Static NAT
    
- PAT
    

වගේ methods තියෙනවා.

---

# 15. Example — Public Web Server

හිතමු company එකේ web server එක:

```text
Internal:
10.10.20.50
```

Internet users access කරන්නේ:

```text
Public IP:
203.x.x.50
```

Architecture:

```text
Internet
    │
203.x.x.50
    │
    ▼
Palo Alto
    │
Destination NAT
    │
    ▼
10.10.20.50
    │
Web Server
```

ඒකට security policy එකත් වෙනම තියෙනවා.

---

# 16. SSL/TLS Decryption

මේක advanced feature එකක්.

අද Internet traffic ගොඩක්:

```text
HTTPS
 ↓
Encrypted
```

ඒ නිසා firewall එකට encrypted content එක inspect කරන්න limitations තියෙනවා.

Decryption architecture:

```text
User
  │
HTTPS
  │
  ▼
Palo Alto
  │
Decrypt
  │
Security Inspection
  │
Threat?
  │
  ├── YES → BLOCK
  │
  └── NO
       │
     Encrypt
       │
       ▼
   Internet
```

හැබැයි මේක deploy කරනකොට:

- Privacy
    
- Legal requirements
    
- Certificates
    
- Performance
    
- Sensitive applications
    
- Banking/health-related destinations
    

වගේ දේවල් carefully consider කරන්න ඕනේ.

---

# 17. VPN

Palo Alto VPN gateway එකක් විදිහටත් use කරන්න පුළුවන්.

### Site-to-Site VPN

```text
HEAD OFFICE
     │
Palo Alto
     │
   IPsec
     │
     ▼
Palo Alto
     │
BRANCH OFFICE
```

### Remote access

```text
Employee
    │
 Internet
    │
    ▼
Palo Alto
    │
Company Network
```

Palo Alto ecosystem එකේ remote access සඳහා **GlobalProtect** technology එකත් තියෙනවා.

---

# 18. Logging

Firewall එකේ security decisions වල evidence අවශ්‍යයි.

Common logs:

```text
Traffic Logs
Threat Logs
URL Logs
WildFire Logs
System Logs
Configuration Logs
Authentication Logs
```

Architecture:

```text
Palo Alto
    │
    ▼
Logs
    │
    ├── Local
    │
    └── SIEM
          │
          ▼
     SOC / Analyst
```

ඒක **[[NIST Framework]] → Detect / Respond** concepts වලට directly connect වෙනවා.

---

# 19. High Availability — Enterprise Level

Production environment එකේ එක firewall එකක් විතරක් තිබ්බොත්:

```text
Firewall failure
      ↓
Network outage
```

ඒ නිසා firewalls දෙකක්:

```text
                 INTERNET
                    │
          ┌─────────┴─────────┐
          │                   │
       PA-01                PA-02
       ACTIVE               PASSIVE
          │                   │
          └─────────┬─────────┘
                    │
                   LAN
```

PA-01 fail වුණොත්:

```text
PA-01
 ↓
FAIL
 ↓
PA-02
 ↓
ACTIVE
```

මේක enterprise reliability එකට වැදගත්.

---

# 20. Panorama

Company එකක firewalls 20ක් තියෙනවා කියලා හිතන්න.

```text
              Panorama
                  │
       ┌──────────┼──────────┐
       │          │          │
      HQ       Branch 1    Branch 2
       │          │          │
      PA-01      PA-02      PA-03
```

**Panorama** centralized management platform එකක්.

එකෙන් multiple Palo Alto firewalls manage කරන්න පුළුවන්.

Centralized:

- Policies
    
- Objects
    
- Templates
    
- Device Groups
    
- Administration
    
- Monitoring
    

වගේ things manage කරන්න පුළුවන්.

---

# 21. Advanced Enterprise Architecture

Large organization එකකට architecture එක මෙහෙම වෙන්න පුළුවන්:

```text
                              INTERNET
                                  │
                            ISP / Edge
                                  │
                                  ▼
                     ┌─────────────────────┐
                     │    PALO ALTO HA     │
                     │                     │
                     │ PA-01     PA-02     │
                     │ ACTIVE    PASSIVE   │
                     └──────────┬──────────┘
                                │
                         Core Network
                                │
          ┌─────────────────────┼─────────────────────┐
          │                     │                     │
          ▼                     ▼                     ▼
         DMZ                   USERS                SERVERS
          │                     │                     │
      Web Server              PCs/Laptops         Applications
                                                        │
                                                        ▼
                                                    Database

                                │
                                ▼
                            Panorama
                                │
                    Central Management
                                │
                                ▼
                              SIEM
                                │
                                ▼
                              SOC
```

මේක තමයි Palo Alto එක **එක firewall එකක්** කියන concept එකෙන් එහාට **enterprise security architecture** එකක් විදිහට බලන්න ඕන විදිහ.

---

# 22. Palo Alto vs Traditional Firewall

|Traditional Firewall|Palo Alto NGFW|
|---|---|
|IP-based|Application-aware|
|Port-based|App-ID|
|Basic ACL|Context-aware policies|
|Limited user visibility|User-ID|
|Basic filtering|Advanced threat prevention|
|Basic logging|Deep security logging|
|Basic VPN|Enterprise VPN capabilities|
|Basic firewalling|NGFW security platform|

උදාහරණයක්:

Traditional:

```text
TCP 443 → ALLOW
```

Palo Alto:

```text
User = Finance
        +
Application = Dropbox
        +
Destination = Internet
        +
File Type = Executable
        ↓
BLOCK
```

මේක තමයි NGFW එකේ power එක.

---

# 23. Palo Alto + CIS + NIST + [[Nipper Tool (Nipper InfraSight - Titania)|Nipper]]

ඔයාගේ current learning path එකේ මේ හතර එකට connect වෙනවා.

```text
                [[NIST Framework]]
                       │
              Security Risk Model
                       │
                       ▼
              [[CIS organization framework]]
                       │
              Security Controls
                       │
                       ▼
              CIS Benchmarks
                       │
                       ▼
                Palo Alto Firewall
                       │
                       ▼
        [[Nipper Tool (Nipper InfraSight - Titania)]]
                       │
              Configuration Audit
                       │
                       ▼
             Findings / Evidence
                       │
                       ▼
                  Remediation
```

### Simple mental model:

**NIST** → _"Cybersecurity risk එක manage කරන්නේ කොහොමද?"_

**CIS Controls** → _"මොන security safeguards implement කරන්නද?"_

**CIS Benchmarks** → _"Device එක secure configure කරන්නේ කොහොමද?"_

**Palo Alto** → _"Network traffic එක enforce/protect කරන්නේ කොහොමද?"_

**[[Nipper Tool (Nipper InfraSight - Titania)|Nipper]]** → _"Network device configuration එක secure ද කියලා assess කරන්නේ කොහොමද?"_

---

## 🧠 අන්තිමට මේ sentence එක මතක තියාගන්න

**Palo Alto Firewall එකේ main idea එක:**

> **"Who is accessing What, From Where, To Where, Using Which Application, on Which Device, and Is that traffic safe?"**

=======
ඔව්. මේක **Palo Alto Networks Firewall** එකක් zero ඉඳන් advanced level එකට තේරුම් ගන්න විදිහට බලමු.

## 1. Palo Alto Firewall කියන්නේ මොකක්ද?

**Palo Alto Networks NGFW (Next-Generation Firewall)** කියන්නේ network එකට ඇතුල් වෙන සහ පිට වෙන traffic එක **IP/port විතරක් බලලා නොව**, application, user, device, content සහ threat context එකත් බලලා control කරන security platform එකක්.

සරල firewall එක:

```text
Source IP
    +
Destination IP
    +
Port
    ↓
ALLOW / DENY
```

Palo Alto NGFW එක:

```text
User
  +
Device
  +
Source
  +
Destination
  +
Application
  +
Content
  +
Threat
  +
Security Policy
        ↓
ALLOW / BLOCK / INSPECT
```

ඒක තමයි **Next-Generation Firewall** එකක් කියන්නේ.

---

# 2. ඇයි Palo Alto Firewall එකක් ඕනේ?

Company network එකේ devices තියෙනවා:

```text
                    INTERNET
                       │
                       ▼
                 [ FIREWALL ]
                       │
          ┌────────────┼────────────┐
          ▼            ▼            ▼
        USERS        SERVERS       DMZ
          │            │            │
       PCs/Laptops   Database     Web Server
```

Firewall එකේ ප්‍රධාන job එක:

> **කවුද → මොන application එකෙන් → කොහෙට → මොන traffic එකක් → යවන්නේද, ඒක allow කරන්නද block කරන්නද?**

උදාහරණයක්:

Employee කෙනෙක් Internet යනවා.

```text
Employee PC
     ↓
HTTPS
     ↓
Internet
```

සාමාන්‍ය firewall එක:

```text
TCP 443
    ↓
ALLOW
```

Palo Alto එකට:

```text
TCP 443
   ↓
Identify Application
   ↓
GitHub
   ↓
Identify User
   ↓
IT Department
   ↓
Check Security Policy
   ↓
Threat Inspection
   ↓
ALLOW
```

වගේ granular decision එකක් ගන්න පුළුවන්.

---

# 3. Palo Alto Architecture එක

Palo Alto firewall එක තේරුම් ගන්න මේ architecture එක මුලින්ම මතක තියාගන්න.

```text
                         INTERNET
                            │
                            ▼
                     ┌─────────────┐
                     │  PALO ALTO  │
                     │    NGFW     │
                     └──────┬──────┘
                            │
                  ┌─────────┼─────────┐
                  │         │         │
                  ▼         ▼         ▼
                 DMZ      USERS     SERVERS
                  │         │         │
                  ▼         ▼         ▼
               Web Apps   PCs      Databases
```

Firewall එක ඇතුළේ:

```text
Palo Alto NGFW
│
├── PAN-OS
│
├── Interfaces
│
├── Security Zones
│
├── Virtual Routers
│
├── Security Policies
│
├── NAT Policies
│
├── App-ID
│
├── User-ID
│
├── Device-ID
│
├── Content-ID
│
├── Security Profiles
│
├── Decryption
│
├── VPN
│
├── Logging
│
├── High Availability
│
└── Panorama
```

---

# 4. PAN-OS

**PAN-OS** කියන්නේ Palo Alto firewall එකේ operating system / software platform එක.

මේක ඇතුළේ network සහ security functions ගොඩක් integrate වෙනවා.

```text
Palo Alto Hardware
       │
       ▼
     PAN-OS
       │
 ┌─────┼─────────────┐
 ▼     ▼             ▼
Network Security   Visibility
```

PAN-OS තමයි:

- Interfaces
    
- Routing
    
- Security policies
    
- NAT
    
- VPN
    
- Application identification
    
- User identification
    
- Threat prevention
    
- Logging
    

වගේ functions manage කරන්නේ.

---

# 5. Interfaces

Firewall එකට network connections තියෙනවා.

උදා:

```text
ethernet1/1 → Internet
ethernet1/2 → Internal LAN
ethernet1/3 → DMZ
```

Architecture:

```text
Internet
   │
   ▼
ethernet1/1
   │
┌──────────────┐
│ Palo Alto    │
└──────────────┘
   │
ethernet1/2
   │
   ▼
Internal LAN
```

Palo Alto interfaces Layer 3, Layer 2, Virtual Wire, Tunnel, TAP වගේ modes වල configure කරන්න පුළුවන්.

---

# 6. Security Zones

Zone කියන්නේ security boundary එකක්.

උදා:

```text
Untrust
   ↓
Internet

Trust
   ↓
Employees

Server
   ↓
Servers

DMZ
   ↓
Public-facing servers

Management
   ↓
Management systems
```

Example:

```text
                 INTERNET
                    │
                 Untrust
                    │
                    ▼
              PALO ALTO
                    │
          ┌─────────┼─────────┐
          ▼         ▼         ▼
        DMZ       Trust      Server
```

ඊට පස්සේ security policy එකේ කියන්න පුළුවන්:

```text
Trust → Internet
ALLOW

Guest → Server
DENY

DMZ → Internal
LIMITED
```

---

# 7. Virtual Router

Palo Alto firewall එක routing කරන්නත් පුළුවන්.

Virtual Router එකේ:

- Static routes
    
- OSPF
    
- BGP
    
- Route redistribution
    
- Routing tables
    

වගේ functions manage කරන්න පුළුවන්.

උදා:

```text
                    Palo Alto
                        │
                  Virtual Router
                        │
             ┌──────────┼──────────┐
             ▼          ▼          ▼
            LAN        DMZ       Internet
```

---

# 8. Security Policy — Firewall එකේ Heart එක

මේක තමයි Palo Alto ඉගෙනගන්නකොට ගොඩක් වැදගත්.

Security policy එක roughly:

```text
Source Zone
Source Address
Source User
Destination Zone
Destination Address
Application
Service
Action
```

Example:

```text
Rule: Allow-Employees-Web

Source Zone:
Trust

Source:
Employee-Network

Destination Zone:
Untrust

Destination:
Any

Application:
web-browsing
ssl

Service:
application-default

Action:
ALLOW
```

මෙතන important difference එක:

**"Port 443 allow කරන්න"** කියන එකට වඩා

**"Approved applications සඳහා 443 traffic allow කරන්න"**

වගේ granular control එකක් ගන්න පුළුවන්.

---

# 9. App-ID 🔥

Palo Altoගේ famous technology එකක් තමයි **App-ID**.

මේක firewall එකට traffic එකේ application එක identify කරන්න උදව් කරනවා.

උදා:

```text
TCP 443
   ↓
App-ID
   ↓
YouTube
```

හෝ:

```text
TCP 443
   ↓
App-ID
   ↓
GitHub
```

ඒ නිසා policy එක:

```text
ALLOW TCP 443
```

වෙනුවට:

```text
ALLOW GitHub
BLOCK Social Media
ALLOW Microsoft 365
BLOCK Unknown Applications
```

වගේ application-aware policy හදන්න පුළුවන්.

---

# 10. User-ID

IP address එකෙන් user identity එකට map කරන්න පුළුවන්.

උදා:

```text
10.10.10.25
      ↓
Active Directory
      ↓
John
      ↓
IT Department
```

ඊට පස්සේ:

```text
IT Users
    ↓
GitHub
    ↓
ALLOW
```

වගේ policy එකක් හදන්න පුළුවන්.

ඒක IP-based firewall එකකට වඩා powerful.

---

# 11. Device-ID

Traffic එක එවන device එක ගැන context එකත් security policy එකට ගන්න පුළුවන්.

Concept:

```text
User
 +
Device
 +
Application
 +
Location
 +
Network
       ↓
Security Policy
```

මේක modern Zero Trust architectures වලදී useful.

---

# 12. Content / Threat Inspection

Traffic එක allow කළා කියලා ඒ traffic එක safe කියලා අදහස් වෙන්නේ නෑ.

උදා:

```text
User
 ↓
Website
 ↓
Download file
 ↓
Malware
```

Palo Alto security inspection technologies මගින් traffic/content එක inspect කරලා malicious activity identify/block කිරීමට හැකියාව තියෙනවා.

Common security capabilities:

```text
Antivirus
Anti-Spyware
Vulnerability Protection
URL Filtering
File Blocking
WildFire
DNS Security
Data Loss Prevention
```

---

# 13. Security Profiles

Security policy එක:

> "මේ traffic එක allow."

Security profile:

> "Allow කළ traffic එක inspect කරලා dangerous දෙයක් තිබුණොත් stop කරන්න."

Concept:

```text
Traffic
   ↓
Security Policy
   ↓
ALLOW
   ↓
Security Profiles
   ├── Antivirus
   ├── Anti-Spyware
   ├── Vulnerability Protection
   ├── URL Filtering
   └── File Blocking
   ↓
Safe?
   ↓
YES → Continue
NO  → Block
```

---

# 14. NAT

Internal private IP addresses Internet එකට directly use කරන්න බෑ.

ඒ නිසා NAT.

```text
Internal PC
192.168.1.20
      │
      ▼
Palo Alto
      │
      ▼
Public IP
203.x.x.x
      │
      ▼
Internet
```

Palo Alto වල:

- Source NAT
    
- Destination NAT
    
- Static NAT
    
- PAT
    

වගේ methods තියෙනවා.

---

# 15. Example — Public Web Server

හිතමු company එකේ web server එක:

```text
Internal:
10.10.20.50
```

Internet users access කරන්නේ:

```text
Public IP:
203.x.x.50
```

Architecture:

```text
Internet
    │
203.x.x.50
    │
    ▼
Palo Alto
    │
Destination NAT
    │
    ▼
10.10.20.50
    │
Web Server
```

ඒකට security policy එකත් වෙනම තියෙනවා.

---

# 16. SSL/TLS Decryption

මේක advanced feature එකක්.

අද Internet traffic ගොඩක්:

```text
HTTPS
 ↓
Encrypted
```

ඒ නිසා firewall එකට encrypted content එක inspect කරන්න limitations තියෙනවා.

Decryption architecture:

```text
User
  │
HTTPS
  │
  ▼
Palo Alto
  │
Decrypt
  │
Security Inspection
  │
Threat?
  │
  ├── YES → BLOCK
  │
  └── NO
       │
     Encrypt
       │
       ▼
   Internet
```

හැබැයි මේක deploy කරනකොට:

- Privacy
    
- Legal requirements
    
- Certificates
    
- Performance
    
- Sensitive applications
    
- Banking/health-related destinations
    

වගේ දේවල් carefully consider කරන්න ඕනේ.

---

# 17. VPN

Palo Alto VPN gateway එකක් විදිහටත් use කරන්න පුළුවන්.

### Site-to-Site VPN

```text
HEAD OFFICE
     │
Palo Alto
     │
   IPsec
     │
     ▼
Palo Alto
     │
BRANCH OFFICE
```

### Remote access

```text
Employee
    │
 Internet
    │
    ▼
Palo Alto
    │
Company Network
```

Palo Alto ecosystem එකේ remote access සඳහා **GlobalProtect** technology එකත් තියෙනවා.

---

# 18. Logging

Firewall එකේ security decisions වල evidence අවශ්‍යයි.

Common logs:

```text
Traffic Logs
Threat Logs
URL Logs
WildFire Logs
System Logs
Configuration Logs
Authentication Logs
```

Architecture:

```text
Palo Alto
    │
    ▼
Logs
    │
    ├── Local
    │
    └── SIEM
          │
          ▼
     SOC / Analyst
```

ඒක **[[NIST Framework]] → Detect / Respond** concepts වලට directly connect වෙනවා.

---

# 19. High Availability — Enterprise Level

Production environment එකේ එක firewall එකක් විතරක් තිබ්බොත්:

```text
Firewall failure
      ↓
Network outage
```

ඒ නිසා firewalls දෙකක්:

```text
                 INTERNET
                    │
          ┌─────────┴─────────┐
          │                   │
       PA-01                PA-02
       ACTIVE               PASSIVE
          │                   │
          └─────────┬─────────┘
                    │
                   LAN
```

PA-01 fail වුණොත්:

```text
PA-01
 ↓
FAIL
 ↓
PA-02
 ↓
ACTIVE
```

මේක enterprise reliability එකට වැදගත්.

---

# 20. Panorama

Company එකක firewalls 20ක් තියෙනවා කියලා හිතන්න.

```text
              Panorama
                  │
       ┌──────────┼──────────┐
       │          │          │
      HQ       Branch 1    Branch 2
       │          │          │
      PA-01      PA-02      PA-03
```

**Panorama** centralized management platform එකක්.

එකෙන් multiple Palo Alto firewalls manage කරන්න පුළුවන්.

Centralized:

- Policies
    
- Objects
    
- Templates
    
- Device Groups
    
- Administration
    
- Monitoring
    

වගේ things manage කරන්න පුළුවන්.

---

# 21. Advanced Enterprise Architecture

Large organization එකකට architecture එක මෙහෙම වෙන්න පුළුවන්:

```text
                              INTERNET
                                  │
                            ISP / Edge
                                  │
                                  ▼
                     ┌─────────────────────┐
                     │    PALO ALTO HA     │
                     │                     │
                     │ PA-01     PA-02     │
                     │ ACTIVE    PASSIVE   │
                     └──────────┬──────────┘
                                │
                         Core Network
                                │
          ┌─────────────────────┼─────────────────────┐
          │                     │                     │
          ▼                     ▼                     ▼
         DMZ                   USERS                SERVERS
          │                     │                     │
      Web Server              PCs/Laptops         Applications
                                                        │
                                                        ▼
                                                    Database

                                │
                                ▼
                            Panorama
                                │
                    Central Management
                                │
                                ▼
                              SIEM
                                │
                                ▼
                              SOC
```

මේක තමයි Palo Alto එක **එක firewall එකක්** කියන concept එකෙන් එහාට **enterprise security architecture** එකක් විදිහට බලන්න ඕන විදිහ.

---

# 22. Palo Alto vs Traditional Firewall

|Traditional Firewall|Palo Alto NGFW|
|---|---|
|IP-based|Application-aware|
|Port-based|App-ID|
|Basic ACL|Context-aware policies|
|Limited user visibility|User-ID|
|Basic filtering|Advanced threat prevention|
|Basic logging|Deep security logging|
|Basic VPN|Enterprise VPN capabilities|
|Basic firewalling|NGFW security platform|

උදාහරණයක්:

Traditional:

```text
TCP 443 → ALLOW
```

Palo Alto:

```text
User = Finance
        +
Application = Dropbox
        +
Destination = Internet
        +
File Type = Executable
        ↓
BLOCK
```

මේක තමයි NGFW එකේ power එක.

---

# 23. Palo Alto + CIS + NIST + [[Nipper Tool (Nipper InfraSight - Titania)|Nipper]]

ඔයාගේ current learning path එකේ මේ හතර එකට connect වෙනවා.

```text
                [[NIST Framework]]
                       │
              Security Risk Model
                       │
                       ▼
              [[CIS organization framework]]
                       │
              Security Controls
                       │
                       ▼
              CIS Benchmarks
                       │
                       ▼
                Palo Alto Firewall
                       │
                       ▼
        [[Nipper Tool (Nipper InfraSight - Titania)]]
                       │
              Configuration Audit
                       │
                       ▼
             Findings / Evidence
                       │
                       ▼
                  Remediation
```

### Simple mental model:

**NIST** → _"Cybersecurity risk එක manage කරන්නේ කොහොමද?"_

**CIS Controls** → _"මොන security safeguards implement කරන්නද?"_

**CIS Benchmarks** → _"Device එක secure configure කරන්නේ කොහොමද?"_

**Palo Alto** → _"Network traffic එක enforce/protect කරන්නේ කොහොමද?"_

**[[Nipper Tool (Nipper InfraSight - Titania)|Nipper]]** → _"Network device configuration එක secure ද කියලා assess කරන්නේ කොහොමද?"_

---

## 🧠 අන්තිමට මේ sentence එක මතක තියාගන්න

**Palo Alto Firewall එකේ main idea එක:**

> **"Who is accessing What, From Where, To Where, Using Which Application, on Which Device, and Is that traffic safe?"**

>>>>>>> a159bce7962d6af920654f254fefef8e27f23cd4
ඒ ප්‍රශ්න ටිකට answer එකක් අරගෙන **policy + inspection + threat prevention + logging** හරහා traffic එක control කරන එක තමයි Palo Alto NGFW එකේ core idea එක.