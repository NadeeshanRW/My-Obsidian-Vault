
# Linux වල `cron` සහ `crontab` — සම්පූර්ණ පැහැදිලි කිරීම

Linux System Administration වල **Cron** කියන්නේ ඉතාම වැදගත් concept එකක්. Server එකක අපිට යම් task එකක් **automatically, specified time එකකදී** run කරවන්න Cron භාවිතා කරනවා.

උදාහරණ:

- හැම දවසකම database backup එකක් ගන්න
    
- හැම පැයකම log cleanup එකක් කරන්න
    
- සෑම විනාඩි 5කටම script එකක් run කරන්න
    
- හැම ඉරිදාවකම report එකක් generate කරන්න
    
- Temporary files delete කරන්න
    
- Application එකක් health-check කරන්න
    
- Disk usage check කරන්න
    
- Backup server එකකට files copy කරන්න
    

---

# 1. Cron කියන්නේ මොකක්ද?

**Cron** කියන්නේ Linux/Unix systems වල තියෙන **time-based job scheduler** එකක්.

සරලව:

> **Cron = නියමිත වේලාවකට commands/scripts automatically run කරන service එක.**

උදාහරණයක්:

ඔයාට හැම දවසකම:

```text
2:00 AM
   ↓
database backup
   ↓
backup.sh
```

run කරන්න ඕන නම් Cron භාවිතා කරන්න පුළුවන්.

Manual එකෙන්:

```bash
./backup.sh
```

run කරන්න ඕන නැහැ.

Cron automatically run කරනවා.

---

# 2. Cron කියන්නේ Service එකක්ද?

ඔව්.

Linux distribution එක අනුව Cron service එක:

```text
cron
```

හෝ:

```text
crond
```

වෙන්න පුළුවන්.

Debian/Ubuntu වගේ systems වල:

```bash
systemctl status cron
```

RHEL / Rocky / AlmaLinux වගේ systems වල:

```bash
systemctl status crond
```

---

# 3. Cron සහ Crontab අතර වෙනස

මේ දෙක confuse කරගන්න එපා.

### Cron

**Scheduler/service එක.**

එය scheduled jobs check කරලා appropriate වෙලාවේ execute කරනවා.

### Crontab

**Cron jobs configure කරන table/file එක.**

```text
Cron
 ↓
Scheduled jobs
 ↓
Crontab
```

සරලව:

> **Cron = වැඩ කරන scheduler**

> **Crontab = කරන්න ඕන වැඩ සහ වෙලාව තියෙන schedule එක**

---

# 4. Real-world Example

හිතන්න server එකක:

```text
/opt/scripts/backup.sh
```

තියෙනවා.

ඔයාට ඕන:

```text
හැම දවසකම 1:00 AM
        ↓
backup.sh run කරන්න
```

Crontab එකට:

```bash
0 1 * * * /opt/scripts/backup.sh
```

දැම්මොත් Cron ඒ script එක හැමදාම 1:00 AM run කරනවා.

---

# 5. Crontab බලන්නේ කොහොමද?

Current userගේ cron jobs බලන්න:

```bash
crontab -l
```

`-l` = list

Example:

```text
0 1 * * * /opt/scripts/backup.sh
*/5 * * * * /opt/scripts/check.sh
```

---

# 6. Crontab Edit කිරීම

Current userගේ crontab edit කරන්න:

```bash
crontab -e
```

`-e` = edit

First time `crontab -e` run කළොත් editor එක select කරන්න කියලා prompt එකක් එන්න පුළුවන්.

උදාහරණයක්:

```text
Select an editor:
1. nano
2. vim
```

Beginner කෙනෙක් නම් `nano` ලේසියි.

---

# 7. Cron Job එකක Basic Structure එක

මේක **අනිවාර්යයෙන්ම memorize කරන්න**:

```text
* * * * * command
│ │ │ │ │
│ │ │ │ └── Day of Week
│ │ │ └──── Month
│ │ └────── Day of Month
│ └──────── Hour
└────────── Minute
```

ඒ කියන්නේ:

```text
MINUTE
HOUR
DAY OF MONTH
MONTH
DAY OF WEEK
COMMAND
```

Full structure:

```text
┌──────────── minute (0 - 59)
│ ┌────────── hour (0 - 23)
│ │ ┌──────── day of month (1 - 31)
│ │ │ ┌────── month (1 - 12)
│ │ │ │ ┌──── day of week (0 - 7)
│ │ │ │ │
* * * * * command
```

---

# 8. `*` කියන්නේ මොකක්ද?

`*` කියන්නේ:

> **Any / Every**

උදාහරණයක්:

```bash
* * * * * command
```

කියන්නේ:

> හැම minute එකකම command එක run කරන්න.

---

# 9. සෑම විනාඩියකටම Run කිරීම

```bash
* * * * * /opt/scripts/test.sh
```

මේක:

```text
12:00
12:01
12:02
12:03
12:04
...
```

හැම minute එකකම run වෙනවා.

---

# 10. සෑම විනාඩි 5කටම

```bash
*/5 * * * * /opt/scripts/test.sh
```

මෙහි:

```text
*/5
```

කියන්නේ:

> Every 5 minutes

ඒ නිසා:

```text
12:00
12:05
12:10
12:15
12:20
...
```

---

# 11. සෑම විනාඩි 10කටම

```bash
*/10 * * * * /opt/scripts/test.sh
```

Run:

```text
12:00
12:10
12:20
12:30
...
```

---

# 12. සෑම පැයකටම

```bash
0 * * * * /opt/scripts/test.sh
```

මෙහි:

```text
0
```

minute field එකේ තියෙන නිසා:

```text
1:00
2:00
3:00
4:00
...
```

හැම hour එකකම 00 minute එකේ run වෙනවා.

---

# 13. සෑම පැය 2කටම

```bash
0 */2 * * * /opt/scripts/test.sh
```

Run:

```text
00:00
02:00
04:00
06:00
08:00
...
```

---

# 14. සෑම දවසකම 12 AM

```bash
0 0 * * * /opt/scripts/backup.sh
```

මෙහි:

```text
minute = 0
hour   = 0
```

ඒ නිසා:

> හැමදාම midnight එකේ run වෙනවා.

---

# 15. සෑම දවසකම 2 AM

```bash
0 2 * * * /opt/scripts/backup.sh
```

Meaning:

```text
Every day
at 02:00 AM
```

---

# 16. සෑම දවසකම 6:30 PM

```bash
30 18 * * * /opt/scripts/report.sh
```

Meaning:

```text
Every day
18:30
```

---

# 17. Specific Day එකක Run කිරීම

හිතන්න:

> හැම Monday එකකම 9:00 AM run කරන්න.

```bash
0 9 * * 1 /opt/scripts/report.sh
```

මෙහි:

```text
1 = Monday
```

සාමාන්‍යයෙන්:

```text
0 = Sunday
1 = Monday
2 = Tuesday
3 = Wednesday
4 = Thursday
5 = Friday
6 = Saturday
7 = Sunday
```

---

# 18. Monday සිට Friday දක්වා

```bash
0 9 * * 1-5 /opt/scripts/report.sh
```

Meaning:

```text
Monday
Tuesday
Wednesday
Thursday
Friday
```

හැමදාම 9:00 AM.

---

# 19. Weekend එකේ

Saturday සහ Sunday:

```bash
0 10 * * 6,0 /opt/scripts/weekend.sh
```

Meaning:

```text
Saturday 10:00
Sunday   10:00
```

---

# 20. Month එකේ Specific Date එකක

හැම මාසෙම 1 වෙනිදා:

```bash
0 0 1 * * /opt/scripts/monthly.sh
```

Meaning:

> Every month on the 1st at midnight.

---

# 21. හැම මාසෙම 15 වෙනිදා 8 PM

```bash
0 20 15 * * /opt/scripts/report.sh
```

Meaning:

```text
Every month
15th
20:00
```

---

# 22. Specific Months

January, June, December වල run කරන්න:

```bash
0 2 * 1,6,12 /opt/scripts/task.sh
```

මෙහි:

```text
1  = January
6  = June
12 = December
```

---

# 23. Range එකක් භාවිතා කිරීම

Monday සිට Friday:

```bash
0 9 * * 1-5 /opt/scripts/task.sh
```

Hours 9 සිට 17 දක්වා:

```bash
0 9-17 * * * /opt/scripts/task.sh
```

මේක:

```text
09:00
10:00
11:00
...
17:00
```

run වෙනවා.

---

# 24. Multiple Values

Comma භාවිතා කරලා multiple values දෙන්න පුළුවන්.

උදාහරණය:

```bash
0 9,13,18 * * * /opt/scripts/task.sh
```

Meaning:

```text
09:00
13:00
18:00
```

හැමදාම run වෙනවා.

---

# 25. Cron Special Characters

Cron වල ප්‍රධාන symbols:

|Symbol|Meaning|
|---|---|
|`*`|Every|
|`,`|Multiple values|
|`-`|Range|
|`/`|Interval/step|

---

# 26. Example එකක් එකින් එක බලමු

```bash
*/10 9-17 * * 1-5 /opt/scripts/check.sh
```

මේක breakdown කරමු:

```text
*/10
```

→ Every 10 minutes

```text
9-17
```

→ 9 AM සිට 5 PM දක්වා

```text
*
```

→ Every day of month

```text
*
```

→ Every month

```text
1-5
```

→ Monday-Friday

ඒ කියන්නේ:

> **Monday සිට Friday දක්වා, උදේ 9 සිට සවස 5 දක්වා, සෑම විනාඩි 10කටම `check.sh` run කරන්න.**

---

# 27. Cron වල Environment එක

මෙය System Engineer කෙනෙක්ට **ඉතාම වැදගත්**.

Terminal එකෙන්:

```bash
python3 script.py
```

වැඩ කරනවා.

නමුත් Cron එකෙන්:

```bash
* * * * * python3 /opt/script.py
```

වැඩ නොකරන්න පුළුවන්.

ඇයි?

Cron එකේ environment එක interactive shell එක වගේ නෙවෙයි.

ඒ නිසා commands සඳහා **absolute paths** භාවිතා කිරීම හොඳ practice එකක්.

උදාහරණයක්:

```bash
/usr/bin/python3 /opt/scripts/script.py
```

වඩා හොඳයි:

```bash
python3 /opt/scripts/script.py
```

වඩා.

---

# 28. Script එකේ Absolute Paths භාවිතා කරන්න

Bad:

```bash
cd app
python3 backup.py
```

Better:

```bash
cd /opt/myapp && /usr/bin/python3 /opt/myapp/backup.py
```

Cron වලදී:

> **Always prefer absolute paths.**

---

# 29. Cron වල Shell

සාමාන්‍යයෙන් Cron job එක shell එකක් හරහා execute කරනවා.

Crontab එකේ:

```bash
SHELL=/bin/bash
```

වගේ define කරන්න පුළුවන්.

උදාහරණයක්:

```bash
SHELL=/bin/bash

0 2 * * * /opt/scripts/backup.sh
```

---

# 30. Environment Variables

Crontab එකේ:

```bash
PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
```

වගේ variables define කරන්න පුළුවන්.

උදාහරණ:

```bash
PATH=/usr/local/bin:/usr/bin:/bin

0 2 * * * /opt/scripts/backup.sh
```

---

# 31. Cron Output

Cron job එකක් run වුණාම command එක output/error generate කළොත් ඒ output email කිරීමේ behavior එකක් තියෙනවා.

Production environment එකක ඒක explicitly redirect කරන එක හොඳයි.

උදාහරණය:

```bash
0 2 * * * /opt/scripts/backup.sh >> /var/log/backup.log 2>&1
```

මෙහි:

```text
>> /var/log/backup.log
```

stdout log file එකට append කරනවා.

```text
2>&1
```

stderr එකත් stdout එකට redirect කරනවා.

ඒ නිසා:

```text
Normal output
+
Error output
        ↓
backup.log
```

---

# 32. Output එක `/dev/null` ට යැවීම

Output එකක් අවශ්‍ය නැත්නම්:

```bash
0 2 * * * /opt/scripts/backup.sh > /dev/null 2>&1
```

මේකෙන්:

```text
stdout → /dev/null
stderr → /dev/null
```

යනවා.

⚠️ හැබැයි troubleshooting කරන්න ඕන production jobs වල errors completely discard කරන එක හොඳ practice එකක් නෙවෙයි.

---

# 33. Cron Job එකට Comments

Crontab එකේ comments `#` වලින් පටන් ගන්නවා.

```bash
# Daily database backup
0 2 * * * /opt/scripts/db_backup.sh

# Every 10 minutes health check
*/10 * * * * /opt/scripts/health_check.sh
```

මේක production server එකක ඉතාම useful.

---

# 34. Crontab වල User Management

Current user's crontab:

```bash
crontab -e
```

Root userගේ crontab:

```bash
sudo crontab -e
```

Root jobs:

```bash
sudo crontab -l
```

**Root crontab සහ normal user's crontab එක එකම දෙයක් නෙවෙයි.**

උදාහරණ:

```text
User: alice
crontab -e
      ↓
Alice's jobs


Root:
sudo crontab -e
      ↓
Root's jobs
```

---

# 35. `/etc/crontab`

Linux system එකක:

```text
/etc/crontab
```

කියන system-wide crontab එකක් තියෙන්න පුළුවන්.

මේකේ normal user crontab එකට වඩා **extra user field එකක්** තියෙනවා.

User crontab:

```text
* * * * * command
```

System `/etc/crontab`:

```text
* * * * * user command
```

උදාහරණ:

```text
0 2 * * * root /opt/scripts/backup.sh
```

මෙහි:

```text
0  → minute
2  → hour
*  → day
*  → month
*  → weekday
root → user
```

---

# 36. User Crontab vs `/etc/crontab`

|User Crontab|`/etc/crontab`|
|---|---|
|`crontab -e`|`/etc/crontab`|
|User field නැහැ|User field තියෙනවා|
|Current user ලෙස run වෙනවා|Specified user ලෙස run කරන්න පුළුවන්|

උදාහරණ:

User crontab:

```bash
0 2 * * * /opt/scripts/backup.sh
```

System crontab:

```text
0 2 * * * root /opt/scripts/backup.sh
```

---

# 37. `/etc/cron.daily`, `cron.hourly` වගේ directories

බොහෝ Linux systems වල මෙවැනි directories තියෙනවා:

```text
/etc/cron.hourly
/etc/cron.daily
/etc/cron.weekly
/etc/cron.monthly
```

මේවාට scripts දාන්න පුළුවන්.

උදාහරණ:

```text
/etc/cron.daily/backup
```

Daily scheduled execution mechanism එකක් ලෙස system එක configure කරලා තිබුණොත් ඒ scripts daily run වෙනවා.

---

# 38. Cron Service Check කිරීම

### Ubuntu / Debian

```bash
systemctl status cron
```

Start:

```bash
sudo systemctl start cron
```

Enable:

```bash
sudo systemctl enable cron
```

Restart:

```bash
sudo systemctl restart cron
```

---

### Rocky / AlmaLinux / RHEL

```bash
systemctl status crond
```

Start:

```bash
sudo systemctl start crond
```

Enable:

```bash
sudo systemctl enable crond
```

Restart:

```bash
sudo systemctl restart crond
```

---

# 39. Cron Job එක Run වුණාද බලන්නේ කොහොමද?

Distribution එක අනුව logs වෙනස් වෙන්න පුළුවන්.

Ubuntu/Debian වල:

```bash
grep CRON /var/log/syslog
```

RHEL/Rocky/AlmaLinux වල:

```bash
grep CRON /var/log/cron
```

Systemd journal එකෙන්:

```bash
journalctl -u cron
```

හෝ:

```bash
journalctl -u crond
```

---

# 40. Cron Job Troubleshooting

Cron job එක වැඩ නොකරනවා නම් මේ order එකට check කරන්න.

### Step 1 — Crontab එක තියෙනවද?

```bash
crontab -l
```

### Step 2 — Cron service running ද?

Ubuntu:

```bash
systemctl status cron
```

Rocky:

```bash
systemctl status crond
```

### Step 3 — Script එක manually run වෙනවද?

```bash
/opt/scripts/backup.sh
```

### Step 4 — Script executable ද?

```bash
ls -l /opt/scripts/backup.sh
```

Need නම්:

```bash
chmod +x /opt/scripts/backup.sh
```

### Step 5 — Absolute paths තියෙනවද?

```bash
which python3
which docker
which mysqldump
```

ඊට පස්සේ Cron එකේ:

```bash
/usr/bin/python3
/usr/bin/docker
/usr/bin/mysqldump
```

වගේ full paths භාවිතා කරන්න.

### Step 6 — Logs බලන්න

```bash
journalctl -u cron
```

හෝ:

```bash
journalctl -u crond
```

---

# 41. Cron Job එකක් Test කරන හොඳම විදිහ

උදාහරණයක්:

```bash
* * * * * echo "Cron works $(date)" >> /tmp/cron-test.log 2>&1
```

මේක සෑම minute එකකටම run වෙනවා.

Minute 1-2කට පස්සේ:

```bash
cat /tmp/cron-test.log
```

බලන්න.

Output:

```text
Cron works Mon Aug 24 14:50:01 +0530 2026
Cron works Mon Aug 24 14:51:01 +0530 2026
Cron works Mon Aug 24 14:52:01 +0530 2026
```

වගේ පේන්න පුළුවන්.

මේකෙන් Cron service සහ crontab execution test කරන්න පුළුවන්.

---

# 42. Real System Engineer Example — Database Backup

Script:

```text
/opt/scripts/db_backup.sh
```

හැමදාම 2:00 AM run කරන්න:

```bash
0 2 * * * /opt/scripts/db_backup.sh >> /var/log/db_backup.log 2>&1
```

Flow එක:

```text
Cron
 ↓
02:00 AM
 ↓
db_backup.sh
 ↓
Database backup
 ↓
/var/log/db_backup.log
```

---

# 43. Real Example — Docker Cleanup

හැම Sunday එකකම 3:00 AM Docker cleanup:

```bash
0 3 * * 0 /usr/bin/docker system prune -f >> /var/log/docker-cleanup.log 2>&1
```

⚠️ Production server එකක `docker system prune` blindly schedule කරන්න එපා. Images/containers/volumes retention requirements හොඳට check කරලා පමණක් automate කරන්න.

---

# 44. Real Example — Disk Monitoring

සෑම පැයකම:

```bash
0 * * * * /opt/scripts/disk_check.sh >> /var/log/disk_check.log 2>&1
```

Flow:

```text
Every hour
    ↓
disk_check.sh
    ↓
df -h
    ↓
Check disk usage
    ↓
Log / Alert
```

---

# 45. Cron සහ Jenkins අතර වෙනස

ඔයා System Engineer / DevOps පැත්තෙන් වැඩ කරන නිසා මේකත් වැදගත්.

### Cron

Simple scheduled tasks:

```text
Backup
Cleanup
Monitoring
Reports
Scripts
```

### Jenkins

Advanced CI/CD:

```text
Git checkout
Build
Test
Docker build
Push image
Deploy
Notifications
```

ඒ නිසා:

```text
Cron
 ↓
Simple scheduled automation
```

අතර:

```text
Jenkins
 ↓
CI/CD automation
```

---

# 46. Cron සහ Ansible

Ansible එකත් automation සඳහා.

උදාහරණයක්:

```text
Cron
 ↓
Every night
 ↓
Ansible Playbook
 ↓
Multiple servers
 ↓
Backup/configuration task
```

ඒ නිසා Cron එකෙන් Ansible playbook එකක් schedule කරන්නත් පුළුවන්.

---

# 47. Cron vs systemd Timer

Modern Linux systems වල **systemd timers** කියන alternative එකක් තියෙනවා.

```text
Cron
     vs
systemd timer
```

Cron:

- Simple
    
- Easy
    
- Old and widely supported
    
- Scheduling සඳහා excellent
    

systemd timer:

- systemd integration
    
- Better service dependency handling
    
- Journal logging
    
- More control
    
- Modern Linux environments වල useful
    

සාමාන්‍ය simple scheduled scripts සඳහා Cron තවමත් ඉතාම common.

---

# 48. වැදගත් Cron Examples

මේ examples ටික memorize කරගන්න:

### Every minute

```bash
* * * * * /script.sh
```

### Every 5 minutes

```bash
*/5 * * * * /script.sh
```

### Every hour

```bash
0 * * * * /script.sh
```

### Every day at midnight

```bash
0 0 * * * /script.sh
```

### Every day at 2 AM

```bash
0 2 * * * /script.sh
```

### Every day at 6:30 PM

```bash
30 18 * * * /script.sh
```

### Every Monday at 9 AM

```bash
0 9 * * 1 /script.sh
```

### Monday-Friday at 9 AM

```bash
0 9 * * 1-5 /script.sh
```

### Every Sunday at 3 AM

```bash
0 3 * * 0 /script.sh
```

### Every 15 minutes

```bash
*/15 * * * * /script.sh
```

### First day of every month

```bash
0 0 1 * * /script.sh
```

---

# 49. Cron එකේ සම්පූර්ණ Mental Model එක

මේක තේරුම් ගත්තොත් Cron සම්පූර්ණයෙන්ම තේරෙනවා:

```text
                Linux Server
                     │
                     ↓
              Cron / Crond
                     │
                     ↓
                Crontab
                     │
        ┌────────────┼────────────┐
        ↓            ↓            ↓
      Time          User        Command
        │                         │
        ↓                         ↓
    02:00 AM              backup.sh
        │                         │
        └────────────┬────────────┘
                     ↓
              Execute Task
                     ↓
                  Result
                     ↓
             Log / Output
```

---

# 50. අවසාන වශයෙන් මතක තියාගන්න

### Cron

> **Time-based job scheduler/service එක.**

### Crontab

> **Cron jobs සහ ඒවා run වෙන්න ඕන schedule එක configure කරන table එක.**

### ප්‍රධාන commands

```bash
crontab -l
```

→ Current cron jobs බලන්න.

```bash
crontab -e
```

→ Cron jobs edit කරන්න.

```bash
crontab -r
```

→ Current user's **සියලු crontab jobs remove** කරන්න. ⚠️ ඉතාම පරිස්සමින් භාවිතා කරන්න.

```bash
systemctl status cron
```

හෝ:

```bash
systemctl status crond
```

→ Cron service එක check කරන්න.

---

## ⭐ එකම formula එකක් මතක තියාගන්න

```text
┌──────── Minute (0-59)
│ ┌────── Hour (0-23)
│ │ ┌──── Day of Month (1-31)
│ │ │ ┌── Month (1-12)
│ │ │ │ ┌ Day of Week (0-7)
│ │ │ │ │
* * * * * COMMAND
```

උදාහරණය:

```bash
0 2 * * * /opt/scripts/backup.sh
```

කියන්නේ:

> **හැම දවසකම උදේ 2:00ට `backup.sh` automatically run කරන්න.**

=======
# Linux වල `cron` සහ `crontab` — සම්පූර්ණ පැහැදිලි කිරීම

Linux System Administration වල **Cron** කියන්නේ ඉතාම වැදගත් concept එකක්. Server එකක අපිට යම් task එකක් **automatically, specified time එකකදී** run කරවන්න Cron භාවිතා කරනවා.

උදාහරණ:

- හැම දවසකම database backup එකක් ගන්න
    
- හැම පැයකම log cleanup එකක් කරන්න
    
- සෑම විනාඩි 5කටම script එකක් run කරන්න
    
- හැම ඉරිදාවකම report එකක් generate කරන්න
    
- Temporary files delete කරන්න
    
- Application එකක් health-check කරන්න
    
- Disk usage check කරන්න
    
- Backup server එකකට files copy කරන්න
    

---

# 1. Cron කියන්නේ මොකක්ද?

**Cron** කියන්නේ Linux/Unix systems වල තියෙන **time-based job scheduler** එකක්.

සරලව:

> **Cron = නියමිත වේලාවකට commands/scripts automatically run කරන service එක.**

උදාහරණයක්:

ඔයාට හැම දවසකම:

```text
2:00 AM
   ↓
database backup
   ↓
backup.sh
```

run කරන්න ඕන නම් Cron භාවිතා කරන්න පුළුවන්.

Manual එකෙන්:

```bash
./backup.sh
```

run කරන්න ඕන නැහැ.

Cron automatically run කරනවා.

---

# 2. Cron කියන්නේ Service එකක්ද?

ඔව්.

Linux distribution එක අනුව Cron service එක:

```text
cron
```

හෝ:

```text
crond
```

වෙන්න පුළුවන්.

Debian/Ubuntu වගේ systems වල:

```bash
systemctl status cron
```

RHEL / Rocky / AlmaLinux වගේ systems වල:

```bash
systemctl status crond
```

---

# 3. Cron සහ Crontab අතර වෙනස

මේ දෙක confuse කරගන්න එපා.

### Cron

**Scheduler/service එක.**

එය scheduled jobs check කරලා appropriate වෙලාවේ execute කරනවා.

### Crontab

**Cron jobs configure කරන table/file එක.**

```text
Cron
 ↓
Scheduled jobs
 ↓
Crontab
```

සරලව:

> **Cron = වැඩ කරන scheduler**

> **Crontab = කරන්න ඕන වැඩ සහ වෙලාව තියෙන schedule එක**

---

# 4. Real-world Example

හිතන්න server එකක:

```text
/opt/scripts/backup.sh
```

තියෙනවා.

ඔයාට ඕන:

```text
හැම දවසකම 1:00 AM
        ↓
backup.sh run කරන්න
```

Crontab එකට:

```bash
0 1 * * * /opt/scripts/backup.sh
```

දැම්මොත් Cron ඒ script එක හැමදාම 1:00 AM run කරනවා.

---

# 5. Crontab බලන්නේ කොහොමද?

Current userගේ cron jobs බලන්න:

```bash
crontab -l
```

`-l` = list

Example:

```text
0 1 * * * /opt/scripts/backup.sh
*/5 * * * * /opt/scripts/check.sh
```

---

# 6. Crontab Edit කිරීම

Current userගේ crontab edit කරන්න:

```bash
crontab -e
```

`-e` = edit

First time `crontab -e` run කළොත් editor එක select කරන්න කියලා prompt එකක් එන්න පුළුවන්.

උදාහරණයක්:

```text
Select an editor:
1. nano
2. vim
```

Beginner කෙනෙක් නම් `nano` ලේසියි.

---

# 7. Cron Job එකක Basic Structure එක

මේක **අනිවාර්යයෙන්ම memorize කරන්න**:

```text
* * * * * command
│ │ │ │ │
│ │ │ │ └── Day of Week
│ │ │ └──── Month
│ │ └────── Day of Month
│ └──────── Hour
└────────── Minute
```

ඒ කියන්නේ:

```text
MINUTE
HOUR
DAY OF MONTH
MONTH
DAY OF WEEK
COMMAND
```

Full structure:

```text
┌──────────── minute (0 - 59)
│ ┌────────── hour (0 - 23)
│ │ ┌──────── day of month (1 - 31)
│ │ │ ┌────── month (1 - 12)
│ │ │ │ ┌──── day of week (0 - 7)
│ │ │ │ │
* * * * * command
```

---

# 8. `*` කියන්නේ මොකක්ද?

`*` කියන්නේ:

> **Any / Every**

උදාහරණයක්:

```bash
* * * * * command
```

කියන්නේ:

> හැම minute එකකම command එක run කරන්න.

---

# 9. සෑම විනාඩියකටම Run කිරීම

```bash
* * * * * /opt/scripts/test.sh
```

මේක:

```text
12:00
12:01
12:02
12:03
12:04
...
```

හැම minute එකකම run වෙනවා.

---

# 10. සෑම විනාඩි 5කටම

```bash
*/5 * * * * /opt/scripts/test.sh
```

මෙහි:

```text
*/5
```

කියන්නේ:

> Every 5 minutes

ඒ නිසා:

```text
12:00
12:05
12:10
12:15
12:20
...
```

---

# 11. සෑම විනාඩි 10කටම

```bash
*/10 * * * * /opt/scripts/test.sh
```

Run:

```text
12:00
12:10
12:20
12:30
...
```

---

# 12. සෑම පැයකටම

```bash
0 * * * * /opt/scripts/test.sh
```

මෙහි:

```text
0
```

minute field එකේ තියෙන නිසා:

```text
1:00
2:00
3:00
4:00
...
```

හැම hour එකකම 00 minute එකේ run වෙනවා.

---

# 13. සෑම පැය 2කටම

```bash
0 */2 * * * /opt/scripts/test.sh
```

Run:

```text
00:00
02:00
04:00
06:00
08:00
...
```

---

# 14. සෑම දවසකම 12 AM

```bash
0 0 * * * /opt/scripts/backup.sh
```

මෙහි:

```text
minute = 0
hour   = 0
```

ඒ නිසා:

> හැමදාම midnight එකේ run වෙනවා.

---

# 15. සෑම දවසකම 2 AM

```bash
0 2 * * * /opt/scripts/backup.sh
```

Meaning:

```text
Every day
at 02:00 AM
```

---

# 16. සෑම දවසකම 6:30 PM

```bash
30 18 * * * /opt/scripts/report.sh
```

Meaning:

```text
Every day
18:30
```

---

# 17. Specific Day එකක Run කිරීම

හිතන්න:

> හැම Monday එකකම 9:00 AM run කරන්න.

```bash
0 9 * * 1 /opt/scripts/report.sh
```

මෙහි:

```text
1 = Monday
```

සාමාන්‍යයෙන්:

```text
0 = Sunday
1 = Monday
2 = Tuesday
3 = Wednesday
4 = Thursday
5 = Friday
6 = Saturday
7 = Sunday
```

---

# 18. Monday සිට Friday දක්වා

```bash
0 9 * * 1-5 /opt/scripts/report.sh
```

Meaning:

```text
Monday
Tuesday
Wednesday
Thursday
Friday
```

හැමදාම 9:00 AM.

---

# 19. Weekend එකේ

Saturday සහ Sunday:

```bash
0 10 * * 6,0 /opt/scripts/weekend.sh
```

Meaning:

```text
Saturday 10:00
Sunday   10:00
```

---

# 20. Month එකේ Specific Date එකක

හැම මාසෙම 1 වෙනිදා:

```bash
0 0 1 * * /opt/scripts/monthly.sh
```

Meaning:

> Every month on the 1st at midnight.

---

# 21. හැම මාසෙම 15 වෙනිදා 8 PM

```bash
0 20 15 * * /opt/scripts/report.sh
```

Meaning:

```text
Every month
15th
20:00
```

---

# 22. Specific Months

January, June, December වල run කරන්න:

```bash
0 2 * 1,6,12 /opt/scripts/task.sh
```

මෙහි:

```text
1  = January
6  = June
12 = December
```

---

# 23. Range එකක් භාවිතා කිරීම

Monday සිට Friday:

```bash
0 9 * * 1-5 /opt/scripts/task.sh
```

Hours 9 සිට 17 දක්වා:

```bash
0 9-17 * * * /opt/scripts/task.sh
```

මේක:

```text
09:00
10:00
11:00
...
17:00
```

run වෙනවා.

---

# 24. Multiple Values

Comma භාවිතා කරලා multiple values දෙන්න පුළුවන්.

උදාහරණය:

```bash
0 9,13,18 * * * /opt/scripts/task.sh
```

Meaning:

```text
09:00
13:00
18:00
```

හැමදාම run වෙනවා.

---

# 25. Cron Special Characters

Cron වල ප්‍රධාන symbols:

|Symbol|Meaning|
|---|---|
|`*`|Every|
|`,`|Multiple values|
|`-`|Range|
|`/`|Interval/step|

---

# 26. Example එකක් එකින් එක බලමු

```bash
*/10 9-17 * * 1-5 /opt/scripts/check.sh
```

මේක breakdown කරමු:

```text
*/10
```

→ Every 10 minutes

```text
9-17
```

→ 9 AM සිට 5 PM දක්වා

```text
*
```

→ Every day of month

```text
*
```

→ Every month

```text
1-5
```

→ Monday-Friday

ඒ කියන්නේ:

> **Monday සිට Friday දක්වා, උදේ 9 සිට සවස 5 දක්වා, සෑම විනාඩි 10කටම `check.sh` run කරන්න.**

---

# 27. Cron වල Environment එක

මෙය System Engineer කෙනෙක්ට **ඉතාම වැදගත්**.

Terminal එකෙන්:

```bash
python3 script.py
```

වැඩ කරනවා.

නමුත් Cron එකෙන්:

```bash
* * * * * python3 /opt/script.py
```

වැඩ නොකරන්න පුළුවන්.

ඇයි?

Cron එකේ environment එක interactive shell එක වගේ නෙවෙයි.

ඒ නිසා commands සඳහා **absolute paths** භාවිතා කිරීම හොඳ practice එකක්.

උදාහරණයක්:

```bash
/usr/bin/python3 /opt/scripts/script.py
```

වඩා හොඳයි:

```bash
python3 /opt/scripts/script.py
```

වඩා.

---

# 28. Script එකේ Absolute Paths භාවිතා කරන්න

Bad:

```bash
cd app
python3 backup.py
```

Better:

```bash
cd /opt/myapp && /usr/bin/python3 /opt/myapp/backup.py
```

Cron වලදී:

> **Always prefer absolute paths.**

---

# 29. Cron වල Shell

සාමාන්‍යයෙන් Cron job එක shell එකක් හරහා execute කරනවා.

Crontab එකේ:

```bash
SHELL=/bin/bash
```

වගේ define කරන්න පුළුවන්.

උදාහරණයක්:

```bash
SHELL=/bin/bash

0 2 * * * /opt/scripts/backup.sh
```

---

# 30. Environment Variables

Crontab එකේ:

```bash
PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
```

වගේ variables define කරන්න පුළුවන්.

උදාහරණ:

```bash
PATH=/usr/local/bin:/usr/bin:/bin

0 2 * * * /opt/scripts/backup.sh
```

---

# 31. Cron Output

Cron job එකක් run වුණාම command එක output/error generate කළොත් ඒ output email කිරීමේ behavior එකක් තියෙනවා.

Production environment එකක ඒක explicitly redirect කරන එක හොඳයි.

උදාහරණය:

```bash
0 2 * * * /opt/scripts/backup.sh >> /var/log/backup.log 2>&1
```

මෙහි:

```text
>> /var/log/backup.log
```

stdout log file එකට append කරනවා.

```text
2>&1
```

stderr එකත් stdout එකට redirect කරනවා.

ඒ නිසා:

```text
Normal output
+
Error output
        ↓
backup.log
```

---

# 32. Output එක `/dev/null` ට යැවීම

Output එකක් අවශ්‍ය නැත්නම්:

```bash
0 2 * * * /opt/scripts/backup.sh > /dev/null 2>&1
```

මේකෙන්:

```text
stdout → /dev/null
stderr → /dev/null
```

යනවා.

⚠️ හැබැයි troubleshooting කරන්න ඕන production jobs වල errors completely discard කරන එක හොඳ practice එකක් නෙවෙයි.

---

# 33. Cron Job එකට Comments

Crontab එකේ comments `#` වලින් පටන් ගන්නවා.

```bash
# Daily database backup
0 2 * * * /opt/scripts/db_backup.sh

# Every 10 minutes health check
*/10 * * * * /opt/scripts/health_check.sh
```

මේක production server එකක ඉතාම useful.

---

# 34. Crontab වල User Management

Current user's crontab:

```bash
crontab -e
```

Root userගේ crontab:

```bash
sudo crontab -e
```

Root jobs:

```bash
sudo crontab -l
```

**Root crontab සහ normal user's crontab එක එකම දෙයක් නෙවෙයි.**

උදාහරණ:

```text
User: alice
crontab -e
      ↓
Alice's jobs


Root:
sudo crontab -e
      ↓
Root's jobs
```

---

# 35. `/etc/crontab`

Linux system එකක:

```text
/etc/crontab
```

කියන system-wide crontab එකක් තියෙන්න පුළුවන්.

මේකේ normal user crontab එකට වඩා **extra user field එකක්** තියෙනවා.

User crontab:

```text
* * * * * command
```

System `/etc/crontab`:

```text
* * * * * user command
```

උදාහරණ:

```text
0 2 * * * root /opt/scripts/backup.sh
```

මෙහි:

```text
0  → minute
2  → hour
*  → day
*  → month
*  → weekday
root → user
```

---

# 36. User Crontab vs `/etc/crontab`

|User Crontab|`/etc/crontab`|
|---|---|
|`crontab -e`|`/etc/crontab`|
|User field නැහැ|User field තියෙනවා|
|Current user ලෙස run වෙනවා|Specified user ලෙස run කරන්න පුළුවන්|

උදාහරණ:

User crontab:

```bash
0 2 * * * /opt/scripts/backup.sh
```

System crontab:

```text
0 2 * * * root /opt/scripts/backup.sh
```

---

# 37. `/etc/cron.daily`, `cron.hourly` වගේ directories

බොහෝ Linux systems වල මෙවැනි directories තියෙනවා:

```text
/etc/cron.hourly
/etc/cron.daily
/etc/cron.weekly
/etc/cron.monthly
```

මේවාට scripts දාන්න පුළුවන්.

උදාහරණ:

```text
/etc/cron.daily/backup
```

Daily scheduled execution mechanism එකක් ලෙස system එක configure කරලා තිබුණොත් ඒ scripts daily run වෙනවා.

---

# 38. Cron Service Check කිරීම

### Ubuntu / Debian

```bash
systemctl status cron
```

Start:

```bash
sudo systemctl start cron
```

Enable:

```bash
sudo systemctl enable cron
```

Restart:

```bash
sudo systemctl restart cron
```

---

### Rocky / AlmaLinux / RHEL

```bash
systemctl status crond
```

Start:

```bash
sudo systemctl start crond
```

Enable:

```bash
sudo systemctl enable crond
```

Restart:

```bash
sudo systemctl restart crond
```

---

# 39. Cron Job එක Run වුණාද බලන්නේ කොහොමද?

Distribution එක අනුව logs වෙනස් වෙන්න පුළුවන්.

Ubuntu/Debian වල:

```bash
grep CRON /var/log/syslog
```

RHEL/Rocky/AlmaLinux වල:

```bash
grep CRON /var/log/cron
```

Systemd journal එකෙන්:

```bash
journalctl -u cron
```

හෝ:

```bash
journalctl -u crond
```

---

# 40. Cron Job Troubleshooting

Cron job එක වැඩ නොකරනවා නම් මේ order එකට check කරන්න.

### Step 1 — Crontab එක තියෙනවද?

```bash
crontab -l
```

### Step 2 — Cron service running ද?

Ubuntu:

```bash
systemctl status cron
```

Rocky:

```bash
systemctl status crond
```

### Step 3 — Script එක manually run වෙනවද?

```bash
/opt/scripts/backup.sh
```

### Step 4 — Script executable ද?

```bash
ls -l /opt/scripts/backup.sh
```

Need නම්:

```bash
chmod +x /opt/scripts/backup.sh
```

### Step 5 — Absolute paths තියෙනවද?

```bash
which python3
which docker
which mysqldump
```

ඊට පස්සේ Cron එකේ:

```bash
/usr/bin/python3
/usr/bin/docker
/usr/bin/mysqldump
```

වගේ full paths භාවිතා කරන්න.

### Step 6 — Logs බලන්න

```bash
journalctl -u cron
```

හෝ:

```bash
journalctl -u crond
```

---

# 41. Cron Job එකක් Test කරන හොඳම විදිහ

උදාහරණයක්:

```bash
* * * * * echo "Cron works $(date)" >> /tmp/cron-test.log 2>&1
```

මේක සෑම minute එකකටම run වෙනවා.

Minute 1-2කට පස්සේ:

```bash
cat /tmp/cron-test.log
```

බලන්න.

Output:

```text
Cron works Mon Aug 24 14:50:01 +0530 2026
Cron works Mon Aug 24 14:51:01 +0530 2026
Cron works Mon Aug 24 14:52:01 +0530 2026
```

වගේ පේන්න පුළුවන්.

මේකෙන් Cron service සහ crontab execution test කරන්න පුළුවන්.

---

# 42. Real System Engineer Example — Database Backup

Script:

```text
/opt/scripts/db_backup.sh
```

හැමදාම 2:00 AM run කරන්න:

```bash
0 2 * * * /opt/scripts/db_backup.sh >> /var/log/db_backup.log 2>&1
```

Flow එක:

```text
Cron
 ↓
02:00 AM
 ↓
db_backup.sh
 ↓
Database backup
 ↓
/var/log/db_backup.log
```

---

# 43. Real Example — Docker Cleanup

හැම Sunday එකකම 3:00 AM Docker cleanup:

```bash
0 3 * * 0 /usr/bin/docker system prune -f >> /var/log/docker-cleanup.log 2>&1
```

⚠️ Production server එකක `docker system prune` blindly schedule කරන්න එපා. Images/containers/volumes retention requirements හොඳට check කරලා පමණක් automate කරන්න.

---

# 44. Real Example — Disk Monitoring

සෑම පැයකම:

```bash
0 * * * * /opt/scripts/disk_check.sh >> /var/log/disk_check.log 2>&1
```

Flow:

```text
Every hour
    ↓
disk_check.sh
    ↓
df -h
    ↓
Check disk usage
    ↓
Log / Alert
```

---

# 45. Cron සහ Jenkins අතර වෙනස

ඔයා System Engineer / DevOps පැත්තෙන් වැඩ කරන නිසා මේකත් වැදගත්.

### Cron

Simple scheduled tasks:

```text
Backup
Cleanup
Monitoring
Reports
Scripts
```

### Jenkins

Advanced CI/CD:

```text
Git checkout
Build
Test
Docker build
Push image
Deploy
Notifications
```

ඒ නිසා:

```text
Cron
 ↓
Simple scheduled automation
```

අතර:

```text
Jenkins
 ↓
CI/CD automation
```

---

# 46. Cron සහ Ansible

Ansible එකත් automation සඳහා.

උදාහරණයක්:

```text
Cron
 ↓
Every night
 ↓
Ansible Playbook
 ↓
Multiple servers
 ↓
Backup/configuration task
```

ඒ නිසා Cron එකෙන් Ansible playbook එකක් schedule කරන්නත් පුළුවන්.

---

# 47. Cron vs systemd Timer

Modern Linux systems වල **systemd timers** කියන alternative එකක් තියෙනවා.

```text
Cron
     vs
systemd timer
```

Cron:

- Simple
    
- Easy
    
- Old and widely supported
    
- Scheduling සඳහා excellent
    

systemd timer:

- systemd integration
    
- Better service dependency handling
    
- Journal logging
    
- More control
    
- Modern Linux environments වල useful
    

සාමාන්‍ය simple scheduled scripts සඳහා Cron තවමත් ඉතාම common.

---

# 48. වැදගත් Cron Examples

මේ examples ටික memorize කරගන්න:

### Every minute

```bash
* * * * * /script.sh
```

### Every 5 minutes

```bash
*/5 * * * * /script.sh
```

### Every hour

```bash
0 * * * * /script.sh
```

### Every day at midnight

```bash
0 0 * * * /script.sh
```

### Every day at 2 AM

```bash
0 2 * * * /script.sh
```

### Every day at 6:30 PM

```bash
30 18 * * * /script.sh
```

### Every Monday at 9 AM

```bash
0 9 * * 1 /script.sh
```

### Monday-Friday at 9 AM

```bash
0 9 * * 1-5 /script.sh
```

### Every Sunday at 3 AM

```bash
0 3 * * 0 /script.sh
```

### Every 15 minutes

```bash
*/15 * * * * /script.sh
```

### First day of every month

```bash
0 0 1 * * /script.sh
```

---

# 49. Cron එකේ සම්පූර්ණ Mental Model එක

මේක තේරුම් ගත්තොත් Cron සම්පූර්ණයෙන්ම තේරෙනවා:

```text
                Linux Server
                     │
                     ↓
              Cron / Crond
                     │
                     ↓
                Crontab
                     │
        ┌────────────┼────────────┐
        ↓            ↓            ↓
      Time          User        Command
        │                         │
        ↓                         ↓
    02:00 AM              backup.sh
        │                         │
        └────────────┬────────────┘
                     ↓
              Execute Task
                     ↓
                  Result
                     ↓
             Log / Output
```

---

# 50. අවසාන වශයෙන් මතක තියාගන්න

### Cron

> **Time-based job scheduler/service එක.**

### Crontab

> **Cron jobs සහ ඒවා run වෙන්න ඕන schedule එක configure කරන table එක.**

### ප්‍රධාන commands

```bash
crontab -l
```

→ Current cron jobs බලන්න.

```bash
crontab -e
```

→ Cron jobs edit කරන්න.

```bash
crontab -r
```

→ Current user's **සියලු crontab jobs remove** කරන්න. ⚠️ ඉතාම පරිස්සමින් භාවිතා කරන්න.

```bash
systemctl status cron
```

හෝ:

```bash
systemctl status crond
```

→ Cron service එක check කරන්න.

---

## ⭐ එකම formula එකක් මතක තියාගන්න

```text
┌──────── Minute (0-59)
│ ┌────── Hour (0-23)
│ │ ┌──── Day of Month (1-31)
│ │ │ ┌── Month (1-12)
│ │ │ │ ┌ Day of Week (0-7)
│ │ │ │ │
* * * * * COMMAND
```

උදාහරණය:

```bash
0 2 * * * /opt/scripts/backup.sh
```

කියන්නේ:

> **හැම දවසකම උදේ 2:00ට `backup.sh` automatically run කරන්න.**

>>>>>>> a159bce7962d6af920654f254fefef8e27f23cd4
Linux System Engineer කෙනෙක් විදිහට Cron ඉගෙනගත්තට පස්සේ ඊළඟට **`cron + bash scripting + logging + backup automation`** එකට practice කරන එක තමයි හොඳම practical step එක.