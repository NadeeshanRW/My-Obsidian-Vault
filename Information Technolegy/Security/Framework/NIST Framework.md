<<<<<<< HEAD
**NIST** කියන්නේ cybersecurity field එකේ ඉතා වැදගත් framework/standards source එකක්. විශේෂයෙන් ඔයා දැන් **Nipper InfraSight** ගැන බලන නිසා NIST තේරුම් ගන්න එක ගොඩක් වැදගත්.

## 1. NIST කියන්නේ මොකක්ද?

**NIST = National Institute of Standards and Technology**

ඒක **U.S. Department of Commerce** යටතේ තියෙන U.S. government organization එකක්.

NIST එක technology, cybersecurity, privacy, cryptography, measurements, standards වගේ areas වල **standards, frameworks, guidelines, technical publications** develop කරනවා.

Cybersecurity වලදී NIST කියන්නේ:

> **"Organization එකක් තමන්ගේ cybersecurity එක systematic විදිහට manage කරන්නේ කොහොමද?"**

කියන ප්‍රශ්නයට framework සහ guidance දෙන ප්‍රධාන source එකක්.

---

# 2. NIST කියන්නේ cybersecurity software එකක් නෙවෙයි

මේක මුලින්ම clear කරගන්න.

❌ NIST = Antivirus එකක් නෙවෙයි  
❌ NIST = Firewall එකක් නෙවෙයි  
❌ NIST = Vulnerability scanner එකක් නෙවෙයි  
❌ NIST = Nipper වගේ application එකක් නෙවෙයි

NIST කියන්නේ **frameworks / standards / guidelines**.

උදාහරණයක්:

NIST

  ↓

Security requirements / guidance

  ↓

Company

  ↓

Policies + Controls

  ↓

Tools

  ↓

Monitoring + Assessment

ඒ නිසා **NIST කියන්නේ "මොන security controls තියෙන්න ඕනෙද?" කියන පැත්ත**, Nipper වගේ tools කියන්නේ ඒ controls/configurations **ප්‍රායෝගිකව assess කරන්න උපකාර කරන tools**.

---

# 3. NIST Cybersecurity Framework — CSF

NIST එකේ cybersecurity පැත්තෙන් famousම framework එකක්:

**NIST Cybersecurity Framework (CSF)**.

දැනට current major version එක **CSF 2.0**.

Official source:

[NIST Cybersecurity Framework 2.0](https://www.nist.gov/cyberframework?utm_source=chatgpt.com)

CSF 2.0 එක organization එකේ cybersecurity risk manage කරන්න framework එකක්.

එහි core functions:

GOVERN

IDENTIFY

PROTECT

DETECT

RESPOND

RECOVER

මීට කලින් popular CSF model එකේ:

Identify

Protect

Detect

Respond

Recover

කියන functions 5 තිබුණා.

CSF 2.0 එකේ **GOVERN** function එක එකතු කරලා cybersecurity governance එක තවත් explicit කරලා තියෙනවා.

---

# 4. ඒ functions එකින් එක බලමු

## 🟢 1. GOVERN

මේක CSF 2.0 එකේ අලුත්ම major concept එක.

ප්‍රශ්නය:

> "Company එක cybersecurity risk එක manage කරන්නේ කවුද? කොහොමද?"

උදා:

Who owns security?

      ↓

What are our security policies?

      ↓

What risk are we willing to accept?

      ↓

Who is responsible?

      ↓

How do suppliers affect our risk?

Company එකට:

- Cybersecurity policies
- Roles & responsibilities
- Risk appetite
- Governance
- Supply-chain risk
- Oversight

වගේ දේවල් define කරන්න වෙනවා.

---

# 5. IDENTIFY

දැන් company එකට තමන් සතු දේවල් තේරුම් ගන්න ඕනේ.

උදා:

Servers

Routers

Firewalls

Switches

Laptops

Applications

Databases

Cloud services

Data

Users

ඊට පස්සේ:

> "මේ assets වලින් වැදගත්ම assets මොනවද?"

කියලා identify කරනවා.

උදා:

Database Server

      ↓

Critical

  

Employee Laptop

      ↓

Medium

  

Guest WiFi

      ↓

Lower

ඒ වගේම cybersecurity risks identify කරනවා.

---

# 6. PROTECT

දැන් assets protect කරන්න controls දානවා.

උදා:

MFA

Passwords

Encryption

Access Control

Firewall

Network Segmentation

Security Training

Backups

Hardening

උදාහරණයක්:

Company database එකට හැම employee කෙනෙක්ටම access තියෙන්න බෑ.

Employee

   X

   ↓

Database

  

Authorized DBA

   ↓

Database

   ✓

ඒක **Protect** function එකේ concept එකක්.

---

# 7. DETECT

Attack එකක් හෝ abnormal activity එකක් වෙද්දි **ඒක හඳුනාගන්න** ඕනේ.

උදා:

SIEM

IDS

IPS

EDR

Log monitoring

Network monitoring

Threat detection

උදාහරණයක්:

Normal:

User → 10 requests/min

  

Suddenly:

User → 50,000 requests/min

Detection system එක:

> "Something unusual is happening."

කියලා alert කරන්න පුළුවන්.

---

# 8. RESPOND

Incident එකක් detect වුණාම:

> "දැන් මොකක්ද කරන්නේ?"

කියන එක.

උදා:

Attack detected

      ↓

Investigate

      ↓

Contain

      ↓

Block attacker

      ↓

Notify responsible teams

      ↓

Take corrective action

උදාහරණයක්:

Compromised account එකක් හම්බ වුණා.

Disable account

       ↓

Revoke sessions

       ↓

Block source

       ↓

Investigate logs

---

# 9. RECOVER

Incident එකෙන් පස්සේ company එක normal state එකට ගේන්න ඕනේ.

උදා:

Ransomware

    ↓

Systems down

    ↓

Incident response

    ↓

Clean systems

    ↓

Restore backup

    ↓

Verify

    ↓

Business resumes

ඒක Recover.

---

# 10. දැන් Nipper InfraSight එක NIST එක්ක සම්බන්ධ වෙන්නේ කොහොමද?

මේක තමයි ඔයාගේ previous question එකට directly connect වෙන point එක.

හිතමු NIST guidance එක අනුව organization එකට:

> Network devices securely configured and access properly controlled වෙන්න ඕනේ.

Company එකේ firewall configuration එක:

Firewall

   ↓

Configuration

   ↓

Nipper InfraSight

   ↓

Assessment

Nipper එකෙන් හොයාගන්න පුළුවන්:

Weak configuration

Overly permissive rule

Unnecessary service

Poor management access

Hardening gap

ඊට පස්සේ ඒ finding එක **NIST control/guidance context** එකකට map කරන්න පුළුවන්, depending on the Nipper assessment/profile being used.

ඒකෙන් auditorට:

NIST requirement/guidance

          ↓

Security control

          ↓

Network device configuration

          ↓

Nipper assessment

          ↓

Evidence

          ↓

Finding

          ↓

Remediation

වගේ chain එකක් ලැබෙනවා.

---

# 11. NIST CSF vs NIST SP 800-53

මෙතන ගොඩක් අය confuse වෙනවා.

## NIST CSF

මේක **high-level cybersecurity risk management framework** එකක්.

Govern

Identify

Protect

Detect

Respond

Recover

Organization එකට cybersecurity program එක structure කරන්න useful.

---

## NIST SP 800-53

මේක තවත් detailed.

**Security and Privacy Controls for Information Systems and Organizations**.

ඒකේ controls ගොඩක් detailed ලෙස define කරනවා.

උදා:

Access Control

Audit and Accountability

Configuration Management

Identification and Authentication

Incident Response

System and Communications Protection

System Integrity

...

Official NIST publication:

[NIST SP 800-53 Rev. 5](https://csrc.nist.gov/pubs/sp/800/53/r5/upd1/final?utm_source=chatgpt.com)

---

# 12. NIST SP 800-53 එකේ controls කියන්නේ මොනවද?

උදාහරණයක්:

### Access Control

ප්‍රශ්නය:

> "කවුද system එකට access වෙන්නේ?"

User

 ↓

Authentication

 ↓

Authorization

 ↓

Resource

---

### Configuration Management

මේක **Nipper** වගේ tool එකකට particularly relevant.

ප්‍රශ්නය:

> "System/device එක securely configured ද?"

උදා:

Firewall

Router

Switch

Server

ඒවායේ configuration:

Baseline

   ↓

Actual configuration

   ↓

Compare

   ↓

Deviation

   ↓

Risk

---

# 13. NIST SP 800-171

තවත් famous standard එකක්:

**NIST SP 800-171**

මේක particularly **Controlled Unclassified Information (CUI)** protect කරන non-federal organizations සඳහා security requirements ගැන.

ඒක U.S. defense supply chain වගේ environments වල important.

---

# 14. NIST SP 800-61

Incident response ගැන.

Organization එකේ:

Incident

   ↓

Preparation

   ↓

Detection/Analysis

   ↓

Containment

   ↓

Eradication

   ↓

Recovery

වගේ incident response lifecycle එක understand කරන්න useful.

---

# 15. NIST සහ ISO 27001 එක එකම දෙයක්ද?

නෑ.

### NIST

Frameworks + technical security guidance + controls.

### ISO/IEC 27001

Information Security Management System (**ISMS**) සඳහා international standard එකක්.

උදා:

ISO 27001

   ↓

Information Security Management System

NIST:

NIST CSF

   ↓

Cybersecurity Risk Management

Company එකකට දෙකම use කරන්න පුළුවන්.

---

# 16. NIST කියන්නේ "company එක NIST compliant" කියලා එක වචනයෙන් කියන්න පුළුවන්ද?

මේක tricky.

**NIST CSF එක ISO 27001 වගේ certification standard එකක් ලෙස simply "NIST certified" කියලා treat කරන්න බෑ.**

Company එකකට:

> "We align our cybersecurity program with NIST CSF 2.0."

කියන එක වඩා නිවැරදියි.

Specific NIST publication/control set එකකට compliance requirements තිබුණොත් ඒක වෙනම matter එකක්.

NIST

NIST

=======
**NIST** කියන්නේ cybersecurity field එකේ ඉතා වැදගත් framework/standards source එකක්. විශේෂයෙන් ඔයා දැන් **Nipper InfraSight** ගැන බලන නිසා NIST තේරුම් ගන්න එක ගොඩක් වැදගත්.

## 1. NIST කියන්නේ මොකක්ද?

**NIST = National Institute of Standards and Technology**

ඒක **U.S. Department of Commerce** යටතේ තියෙන U.S. government organization එකක්.

NIST එක technology, cybersecurity, privacy, cryptography, measurements, standards වගේ areas වල **standards, frameworks, guidelines, technical publications** develop කරනවා.

Cybersecurity වලදී NIST කියන්නේ:

> **"Organization එකක් තමන්ගේ cybersecurity එක systematic විදිහට manage කරන්නේ කොහොමද?"**

කියන ප්‍රශ්නයට framework සහ guidance දෙන ප්‍රධාන source එකක්.

---

# 2. NIST කියන්නේ cybersecurity software එකක් නෙවෙයි

මේක මුලින්ම clear කරගන්න.

❌ NIST = Antivirus එකක් නෙවෙයි  
❌ NIST = Firewall එකක් නෙවෙයි  
❌ NIST = Vulnerability scanner එකක් නෙවෙයි  
❌ NIST = Nipper වගේ application එකක් නෙවෙයි

NIST කියන්නේ **frameworks / standards / guidelines**.

උදාහරණයක්:

NIST

  ↓

Security requirements / guidance

  ↓

Company

  ↓

Policies + Controls

  ↓

Tools

  ↓

Monitoring + Assessment

ඒ නිසා **NIST කියන්නේ "මොන security controls තියෙන්න ඕනෙද?" කියන පැත්ත**, Nipper වගේ tools කියන්නේ ඒ controls/configurations **ප්‍රායෝගිකව assess කරන්න උපකාර කරන tools**.

---

# 3. NIST Cybersecurity Framework — CSF

NIST එකේ cybersecurity පැත්තෙන් famousම framework එකක්:

**NIST Cybersecurity Framework (CSF)**.

දැනට current major version එක **CSF 2.0**.

Official source:

[NIST Cybersecurity Framework 2.0](https://www.nist.gov/cyberframework?utm_source=chatgpt.com)

CSF 2.0 එක organization එකේ cybersecurity risk manage කරන්න framework එකක්.

එහි core functions:

GOVERN

IDENTIFY

PROTECT

DETECT

RESPOND

RECOVER

මීට කලින් popular CSF model එකේ:

Identify

Protect

Detect

Respond

Recover

කියන functions 5 තිබුණා.

CSF 2.0 එකේ **GOVERN** function එක එකතු කරලා cybersecurity governance එක තවත් explicit කරලා තියෙනවා.

---

# 4. ඒ functions එකින් එක බලමු

## 🟢 1. GOVERN

මේක CSF 2.0 එකේ අලුත්ම major concept එක.

ප්‍රශ්නය:

> "Company එක cybersecurity risk එක manage කරන්නේ කවුද? කොහොමද?"

උදා:

Who owns security?

      ↓

What are our security policies?

      ↓

What risk are we willing to accept?

      ↓

Who is responsible?

      ↓

How do suppliers affect our risk?

Company එකට:

- Cybersecurity policies
- Roles & responsibilities
- Risk appetite
- Governance
- Supply-chain risk
- Oversight

වගේ දේවල් define කරන්න වෙනවා.

---

# 5. IDENTIFY

දැන් company එකට තමන් සතු දේවල් තේරුම් ගන්න ඕනේ.

උදා:

Servers

Routers

Firewalls

Switches

Laptops

Applications

Databases

Cloud services

Data

Users

ඊට පස්සේ:

> "මේ assets වලින් වැදගත්ම assets මොනවද?"

කියලා identify කරනවා.

උදා:

Database Server

      ↓

Critical

  

Employee Laptop

      ↓

Medium

  

Guest WiFi

      ↓

Lower

ඒ වගේම cybersecurity risks identify කරනවා.

---

# 6. PROTECT

දැන් assets protect කරන්න controls දානවා.

උදා:

MFA

Passwords

Encryption

Access Control

Firewall

Network Segmentation

Security Training

Backups

Hardening

උදාහරණයක්:

Company database එකට හැම employee කෙනෙක්ටම access තියෙන්න බෑ.

Employee

   X

   ↓

Database

  

Authorized DBA

   ↓

Database

   ✓

ඒක **Protect** function එකේ concept එකක්.

---

# 7. DETECT

Attack එකක් හෝ abnormal activity එකක් වෙද්දි **ඒක හඳුනාගන්න** ඕනේ.

උදා:

SIEM

IDS

IPS

EDR

Log monitoring

Network monitoring

Threat detection

උදාහරණයක්:

Normal:

User → 10 requests/min

  

Suddenly:

User → 50,000 requests/min

Detection system එක:

> "Something unusual is happening."

කියලා alert කරන්න පුළුවන්.

---

# 8. RESPOND

Incident එකක් detect වුණාම:

> "දැන් මොකක්ද කරන්නේ?"

කියන එක.

උදා:

Attack detected

      ↓

Investigate

      ↓

Contain

      ↓

Block attacker

      ↓

Notify responsible teams

      ↓

Take corrective action

උදාහරණයක්:

Compromised account එකක් හම්බ වුණා.

Disable account

       ↓

Revoke sessions

       ↓

Block source

       ↓

Investigate logs

---

# 9. RECOVER

Incident එකෙන් පස්සේ company එක normal state එකට ගේන්න ඕනේ.

උදා:

Ransomware

    ↓

Systems down

    ↓

Incident response

    ↓

Clean systems

    ↓

Restore backup

    ↓

Verify

    ↓

Business resumes

ඒක Recover.

---

# 10. දැන් Nipper InfraSight එක NIST එක්ක සම්බන්ධ වෙන්නේ කොහොමද?

මේක තමයි ඔයාගේ previous question එකට directly connect වෙන point එක.

හිතමු NIST guidance එක අනුව organization එකට:

> Network devices securely configured and access properly controlled වෙන්න ඕනේ.

Company එකේ firewall configuration එක:

Firewall

   ↓

Configuration

   ↓

Nipper InfraSight

   ↓

Assessment

Nipper එකෙන් හොයාගන්න පුළුවන්:

Weak configuration

Overly permissive rule

Unnecessary service

Poor management access

Hardening gap

ඊට පස්සේ ඒ finding එක **NIST control/guidance context** එකකට map කරන්න පුළුවන්, depending on the Nipper assessment/profile being used.

ඒකෙන් auditorට:

NIST requirement/guidance

          ↓

Security control

          ↓

Network device configuration

          ↓

Nipper assessment

          ↓

Evidence

          ↓

Finding

          ↓

Remediation

වගේ chain එකක් ලැබෙනවා.

---

# 11. NIST CSF vs NIST SP 800-53

මෙතන ගොඩක් අය confuse වෙනවා.

## NIST CSF

මේක **high-level cybersecurity risk management framework** එකක්.

Govern

Identify

Protect

Detect

Respond

Recover

Organization එකට cybersecurity program එක structure කරන්න useful.

---

## NIST SP 800-53

මේක තවත් detailed.

**Security and Privacy Controls for Information Systems and Organizations**.

ඒකේ controls ගොඩක් detailed ලෙස define කරනවා.

උදා:

Access Control

Audit and Accountability

Configuration Management

Identification and Authentication

Incident Response

System and Communications Protection

System Integrity

...

Official NIST publication:

[NIST SP 800-53 Rev. 5](https://csrc.nist.gov/pubs/sp/800/53/r5/upd1/final?utm_source=chatgpt.com)

---

# 12. NIST SP 800-53 එකේ controls කියන්නේ මොනවද?

උදාහරණයක්:

### Access Control

ප්‍රශ්නය:

> "කවුද system එකට access වෙන්නේ?"

User

 ↓

Authentication

 ↓

Authorization

 ↓

Resource

---

### Configuration Management

මේක **Nipper** වගේ tool එකකට particularly relevant.

ප්‍රශ්නය:

> "System/device එක securely configured ද?"

උදා:

Firewall

Router

Switch

Server

ඒවායේ configuration:

Baseline

   ↓

Actual configuration

   ↓

Compare

   ↓

Deviation

   ↓

Risk

---

# 13. NIST SP 800-171

තවත් famous standard එකක්:

**NIST SP 800-171**

මේක particularly **Controlled Unclassified Information (CUI)** protect කරන non-federal organizations සඳහා security requirements ගැන.

ඒක U.S. defense supply chain වගේ environments වල important.

---

# 14. NIST SP 800-61

Incident response ගැන.

Organization එකේ:

Incident

   ↓

Preparation

   ↓

Detection/Analysis

   ↓

Containment

   ↓

Eradication

   ↓

Recovery

වගේ incident response lifecycle එක understand කරන්න useful.

---

# 15. NIST සහ ISO 27001 එක එකම දෙයක්ද?

නෑ.

### NIST

Frameworks + technical security guidance + controls.

### ISO/IEC 27001

Information Security Management System (**ISMS**) සඳහා international standard එකක්.

උදා:

ISO 27001

   ↓

Information Security Management System

NIST:

NIST CSF

   ↓

Cybersecurity Risk Management

Company එකකට දෙකම use කරන්න පුළුවන්.

---

# 16. NIST කියන්නේ "company එක NIST compliant" කියලා එක වචනයෙන් කියන්න පුළුවන්ද?

මේක tricky.

**NIST CSF එක ISO 27001 වගේ certification standard එකක් ලෙස simply "NIST certified" කියලා treat කරන්න බෑ.**

Company එකකට:

> "We align our cybersecurity program with NIST CSF 2.0."

කියන එක වඩා නිවැරදියි.

Specific NIST publication/control set එකකට compliance requirements තිබුණොත් ඒක වෙනම matter එකක්.

NIST

NIST

>>>>>>> a159bce7962d6af920654f254fefef8e27f23cd4
NIST