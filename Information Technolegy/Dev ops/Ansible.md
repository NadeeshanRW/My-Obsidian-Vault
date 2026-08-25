
## Ansible යනු කුමක්ද?

- Ansible කියන්නේ **Michael DeHaan** විසින් develop කරපු open source software එකක්. දැන් එහි ownership තියෙන්නේ **RedHat** යටතේ.
- Ansible **Python** language එකෙන් ලියලා තියෙනවා.
- Ansible කියන්නේ automation tool එකක්, එය configuration එක **code** විදිහට define කරන්න ක්‍රමයක් සපයනවා.

### Ansible වලින් කරන්න පුළුවන් දේවල්

1. Automate Configuration Management
2. App Deployments

---

## 3. Ansible Architecture

Ansible Architecture එකේ ප්‍රධාන කොටස් 4ක් තියෙනවා:

|Component|විස්තරය|
|---|---|
|**Control Node**|Ansible software එක install කරලා තියෙන machine එක|
|**Managed Nodes**|Control Node එකෙන් manage කරන machines|
|**Host Inventory File**|Managed nodes ගැන information තියෙන file එක|
|**Playbooks**|Set of tasks තියෙන YML/YAML file එකක්|

---

## 4. Ansible Installation Guide

### Prerequisites

- Linux based OS (Ubuntu / CentOS / RedHat / Amazon Linux)
- Python 3 installed
- SSH access to managed nodes
- sudo/root privileges

### Control Node එකේ Ansible Install කිරීම

**Ubuntu / Debian:**

bash

```bash
sudo apt update
sudo apt install software-properties-common -y
sudo add-apt-repository --yes --update ppa:ansible/ansible
sudo apt install ansible -y
```

**CentOS / RedHat / Amazon Linux:**

bash

```bash
sudo yum update -y
sudo amazon-linux-extras install ansible2 -y
# හෝ
sudo yum install ansible -y
```

**Installation එක Verify කිරීම:**

bash

```bash
ansible --version
```

### Managed Nodes Setup කිරීම

1. Control node එකෙන් Managed node එකට SSH access එකක් තියෙන්න ඕන (Key-based authentication recommend කරනවා).
2. SSH key generate කරලා managed nodes වලට copy කරන්න:

bash

```bash
ssh-keygen -t rsa
ssh-copy-id user@managed-node-ip
```

3. Python managed node එකේත් install වෙලා තියෙන්න ඕන.

### Host Inventory File එක Configure කිරීම

Default location: `/etc/ansible/hosts`

ini

```ini
[webservers]
webserver1 ansible_host=172.31.0.95
webserver2 ansible_host=172.31.0.96

[dbservers]
172.31.5.185
172.31.5.186
```

### Connection එක Test කිරීම

bash

```bash
ansible all -m ping
```

Ansible Setup ගැන වැඩිදුර details: [https://github.com/ashokitschool/DevOps-Documents/blob/main/11-Ansible-Setup.md](https://github.com/ashokitschool/DevOps-Documents/blob/main/11-Ansible-Setup.md)

---

## 5. Ansible Ad-Hoc Commands

Ad-hoc commands run කරන්න syntax එක:

bash

```bash
ansible [all/group-name/private-ip] -m <module> -a <args>
```

### Common Modules

|Module|Purpose|
|---|---|
|`ping`|Connectivity check කරන්න|
|`shell`|Shell commands run කරන්න|
|`yum` / `apt`|Package install කරන්න|
|`service`|Services manage කරන්න|
|`copy`|Files copy කරන්න|

### උදාහරණ Commands

bash

```bash
# All hosts ping කරන්න
ansible all -m ping

# Group එකකට ping කරන්න
ansible webservers -m ping
ansible dbservers -m ping

# Date command එක shell module එකෙන් run කරන්න
ansible all -m shell -a date

# Package එකක් install කරන්න (yum)
ansible all -m yum -a "name=git"
ansible webservers -m yum -a "name=httpd"
```

---

## 6. Ansible Playbooks

- Playbook කියන්නේ **YAML** file එකක්.
- Playbook එකක එක හෝ ඊට වැඩි **tasks** තියෙනවා.
- Playbook use කරලා අපිට define කරන්න පුළුවන් මොන tasks මොකද perform වෙන්නේ, කොහෙද perform වෙන්නේ කියලා.
- Playbook එක Ansible Control Node එකට input එකක් විදිහට දීලා managed nodes වල tasks execute කරනවා.

> Playbook ලියන්න කලින් YAML ගැන දැනගන්න ඕන.

---

## 7. YAML (YML) Basics

- YML/YAML කියන්නේ **"Yet Another Markup Language"**
- Data, human & machine readable format එකකින් store කරන්න use කරනවා
- File extensions: `.yml` හෝ `.yaml`
- Official Website: [https://yaml.org/](https://yaml.org/)
- Syntax validate කරන්න: [https://www.yamllint.com/](https://www.yamllint.com/)

> **වැදගත්:** YAML වල indent spacing එක ගොඩක් වැදගත්. Indentation වැරදුනොත් file එක වැඩ කරන්නේ නෑ.

### Sample YML File - 1

yaml

```yaml
---
id: 101
name: Ashok
gender: Male
hobbies:
 - music
 - chess
 - cricket
...
```

### Sample YML File - 2 (Nested Data)

yaml

```yaml
---
person:
 id: 101
 name: Ashok
 address:
  city: hyd
  state: TG
  country: India
 hobbies:
  - cricket
  - chess
  - music
...
```

### Sample YML File - 3 (Employee Data)

yaml

```yaml
---
emp:
 id: 101
 name: Ashok
 company:
  name: Microsoft
 job:
  exp: 11 Years
  type: permanent
...
```

---

## 8. Playbook Structure

Playbook එකක Sections 3ක් තියෙනවා:

1. **Host Section** – Tasks execute වෙන target machines represent කරනවා
2. **Variable Section** – Playbook execution එකට අවශ්‍ය variables declare කරනවා
3. **Task Section** – Ansible එකෙන් perform කරන්න ඕන operations define කරනවා

> එක playbook එකක multiple tasks specify කරන්න පුළුවන්.

### Playbook Execute කරන Syntax

bash

```bash
ansible-playbook <playbook-yml-file>
```

### Useful Playbook Commands

bash

```bash
# Syntax check කරන්න
ansible-playbook <playbook-yml-file> --syntax-check

# Playbook එකෙන් affect වෙන hosts list කරන්න (run කරන්නේ නෑ)
ansible-playbook <playbook-yml-file> --list-hosts

# Playbook එක run කරන්න
ansible-playbook <playbook-yml-file>

# එක task එකක් එක වතාවක් confirm කරලා run කරන්න (Yes/No/Continue)
ansible-playbook <playbook-yml-file> --step

# Verbose mode එකේ run කරන්න
ansible-playbook <playbook-yml-file> -vvv
```

### Playbook Examples

**1) Managed nodes ping කිරීම**

yaml

```yaml
---
- hosts: all
  tasks:
  - name: ping all managed nodes
    ping:
...
```

**2) File එකක් create කිරීම**

yaml

```yaml
---
- hosts: all
  tasks:
  - name: create a file
    file:
     path: /home/ansible/ashokit.txt
     state: touch
...
```

**3) File එකකට Content Copy කිරීම**

yaml

```yaml
---
- hosts: all
  tasks:
  - name: copy content to file
    copy: content="welcome to ashokit\n" dest="/home/ansible/ashokit.txt"
...
```

**4) Apache (httpd) Install කරලා Start කිරීම**

yaml

```yaml
---
- hosts: webservers
  become: true   # sudo privileges අවශ්‍ය නම් use කරන්න
  tasks:
  - name: install httpd package
    yum:
     name: httpd
     state: latest
  - name: copy index.html file
    copy:
     src: index.html
     dest: /var/www/html/index.html
  - name: start httpd service
    service:
     name: httpd
     state: started
...
```

---

## 9. Handlers & Tags

- Playbook එකක default විදිහට tasks execute වෙන්නේ **sequential order** එකට.
- **Handlers** use කරලා, වෙන task එකක status එක අනුව tasks execute කරන්න පුළුවන් (2nd task එකේ status change වුනොත් විතරක් 3rd task run කරන්න, උදාහරණයක් විදිහට).
- Handler execute කරන්න notify කරන්න **`notify`** keyword එක use කරනවා.
- **Tags** use කරලා task එකක් tag name එකකට map කරන්න පුළුවන්. Tag name එකෙන් specific tasks run කරන්න/skip කරන්න පුළුවන්.

bash

```bash
# Playbook එකේ තියෙන tags ලා display කරන්න
ansible-playbook handlers_tags.yml --list-tags

# 'install' tag එකේ task එක run කරන්න
ansible-playbook handlers_tags.yml --tags "install"

# 'install' සහ 'copy' tags ලා run කරන්න
ansible-playbook handlers_tags.yml --tags "install,copy"

# 'install' සහ 'copy' tags ලා skip කරලා ඉතුරු tasks run කරන්න
ansible-playbook handlers_tags.yml --skip-tags "install,copy"
```

---

## 10. Variables

Variables use කරලා data key-value format එකකින් store කරනවා:

```
id=100
name=ashok
age=20
gender=male
```

Ansible වල variables use කරන ක්‍රම 4ක් තියෙනවා:

1. Runtime Variables
2. Playbook Variables
3. Group Variables
4. Host Variables

### Runtime Variables

Playbook run කරද්දී value එක pass කරන්න:

yaml

```yaml
---
- hosts: webservers
  become: true
  tasks:
  - name: install package
    yum:
     name: "{{package_name}}"
     state: latest
...
```

bash

```bash
ansible-playbook <yml> --extra-vars package_name=httpd
```

### Playbook Variables

Playbook එක ඇතුළෙම variable value එක declare කරන්න:

yaml

```yaml
---
- hosts: webservers
  become: true
  vars:
   package_name: httpd
  tasks:
  - name: install package
    yum:
     name: "{{package_name}}"
     state: latest
...
```

### Group Variables

Inventory file එකේ තියෙන group එකකට (managed nodes group) variable value එකක් specify කරන්න group vars use කරනවා.

**Host Inventory File:**

ini

```ini
[webservers]
webserver1 ansible_host=172.31.0.95
webserver2 ansible_host=172.31.0.96

[dbservers]
172.31.5.185
172.31.5.186
```

**Group vars files (Host Inventory file location එකේම create කරන්න ඕන):**

- Host Inventory location: `/etc/ansible/hosts`
- Webservers variable file: `/etc/ansible/group_vars/webservers.yml`
- Dbservers variable file: `/etc/ansible/group_vars/dbservers.yml`

### Host Variables

Host level (machine level) variable value specify කරන්න host vars use කරනවා.

**Location:** `/etc/ansible/host_vars`

`webserver1.yml`

yaml

```yaml
package_name: java
```

`webserver2.yml`

yaml

```yaml
package_name: python
```

> **Note 1:** Host variables, group variables වලට වඩා precedence ගන්නවා. **Note 2:** Playbook එකේ define කරපු variables, host_vars සහ group_vars දෙකම override කරනවා.

---

## 11. Ansible Vault

Playbook secure කරන්න use කරන feature එකක්. Ansible Vault මගින් playbooks encrypt/decrypt කරන්න පුළුවන්.

- **Encryption:** Readable data → Un-readable data
- **Decryption:** Un-readable data → Readable data

bash

```bash
# Playbook එක encrypt කරන්න
ansible-vault encrypt <yml-file-name>

# Encrypted playbook එක බලන්න
cat <yml-file-name>

# Original content එක බලන්න
ansible-vault view <yml-file-name>

# Encrypted playbook එක edit කරන්න
ansible-vault edit <yml-file-name>

# Encrypted playbook එක run කරන්න
ansible-playbook <yml-file-name> --ask-vault-pass

# Playbook එක decrypt කරන්න
ansible-vault decrypt <yml-file-name>
```

---

## 12. Ansible Roles

Playbook එකකට functionalities වැඩිපුර add කරද්දී එය lengthy වෙනවා, manage කරන්නත් maintain කරන්නත් අමාරුයි. **Roles** concept එකෙන් large playbooks, කුඩා chunks වලට break down කරන්න පුළුවන් (modular, re-usable format එකකින්).

### Role එකක් Create කිරීම

bash

```bash
ansible-galaxy init <role-name>
```

### Ansible Role එකක් සමඟ වැඩ කරන අයුරු

**Step 1:** Control node එකට connect වෙලා ansible user එකට switch වෙන්න

bash

```bash
sudo su ansible
cd ~
```

**Step 2:** Role එකක් create කිරීම

bash

```bash
mkdir roles
cd roles
ansible-galaxy init apache
sudo yum install tree
tree apache
```

**Step 3:** `tasks/main.yml` එකේ tasks create කිරීම

yaml

```yaml
---
# tasks file for apache
- name: install httpd
  yum:
    name: httpd
    state: latest
- name: copy index.html
  copy:
    src: index.html
    dest: /var/www/html/
  notify:
    - restart apache
...
```

**Step 4:** `files` directory එකට අවශ්‍ය files copy කිරීම (index.html file එක files directory එකේ තියන්න ඕන)

**Step 5:** `handlers/main.yml` එකේ handlers configure කිරීම

yaml

```yaml
---
# handlers file for apache
- name: restart apache
  service:
    name: httpd
    state: restarted
...
```

**Step 6:** Role එක invoke කරන Main Playbook එකක් create කිරීම

bash

```bash
cd ~
vi invoke-roles.yml
```

yaml

```yaml
---
- hosts: all
  become: true
  roles:
    - apache
...
```

---

## 13. Gather Facts

Ansible වල gathering facts කියන්නේ, tasks execute කරන්න කලින් target hosts ගැන information (OS, memory, CPU architecture වගේ) automatically collect කරන process එකයි. මේක **`setup`** module එකෙන් කරනවා.

yaml

```yaml
---
- hosts: all
  gather_facts: yes
  tasks:
  - name: ping all managed nodes
    ping:
...
```

---

## 14. Debug Keyword

Playbook execute වෙනකොට message එකක් print කරන්න `debug` keyword එක use කරනවා.

yaml

```yaml
---
- hosts: localhost
  gather_facts: yes
  tasks:
  - name: print the name of OS family
    debug:
      msg: "The OS is {{ansible_os_family}}"

  - name: print memory info
    debug:
      msg: "Total memory is {{ ansible_facts['memtotal_mb'] }} MB."
...
```

---

## 15. Register Keyword

`register` keyword එකෙන් task එකක output එකක් capture කරලා variable එකක් විදිහට පස්සේ use කරන්න store කරගන්න පුළුවන්.

yaml

```yaml
---
- hosts: localhost
  tasks:
  - name: Run a command to get current date
    command: date
    register: date_output

  - name: print date command output
    debug:
     msg: "The current Date is => {{ date_output.stdout }}"

  - name: check command is success or not
    debug:
     msg: "command execution successful"
    when: date_output.rc == 0
...
```

### Different OS Family වල Java Install කිරීමට Playbook එකක්

yaml

```yaml
---
- hosts: all
  gather_facts: yes
  tasks:
  - name: install java in Red Hat family
    yum:
     name: java
     state: latest
    when: ansible_os_family == 'RedHat'

  - name: install java in Debian family
    apt:
     name: java
     state: latest
    when: ansible_os_family == 'Debian'
...
```

---

## 16. Error Handling

Task එකක් execute වෙනකොට error එකක් ආවොත් playbook execution එක abnormal විදිහට (middle එකේදී) terminate වෙනවා. Graceful termination එකක් achieve කරගන්න error එක handle කරන්න ඕන (`ignore_errors`).

yaml

```yaml
---
- hosts: all
  tasks:
  - name: This is first task
    command: dates
    register: dates_output
    ignore_errors: yes

  - name: This is second task
    debug:
     msg: "Second task executed..."
    when: dates_output.rc == 0

  - name: this is third task
    debug:
     var: dates_output
...
```

### Maven Check කරලා Install කරන Playbook එකක්

yaml

```yaml
---
- hosts: localhost
  become: true
  tasks:
  - name: check maven version
    command: mvn --version
    register: output
    ignore_errors: yes

  - name: print output
    debug:
     var: output

  - name: install maven
    yum:
     name: maven
     state: latest
    when: output.failed
...
```