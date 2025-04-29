# Ansible Automation Setup with 3 EC2 Instances (1 Master, 2 Slaves)

## 🚀 Objective
Set up Ansible on a **Master server** and configure it to manage **2 Slave servers** using SSH and inventory setup. You’ll also run playbooks from this repository:  
🔗 [ansible-playbooks by perumandlahemakumari](https://github.com/perumandlahemakumari/ansible-playbooks.git)

---

## 🖥️ Setup Instructions

### Step 1: Launch EC2 Instances
- Create **3 EC2 instances** (Amazon Linux 2):
  - `1 Master`
  - `2 Slaves`

### Step 2: Install Python & Ansible (on Master only)

```bash
sudo yum install python3-pip -y
sudo amazon-linux-extras install ansible2 -y
```

### Step 3: Generate SSH Key (on Master)

```bash
ssh-keygen
```

> Press Enter to accept default location and no passphrase.

---

## 🔐 SSH Configuration for Root Login

### Step 4: Set Root Password (on all servers)

```bash
sudo passwd root
```

### Step 5: Update SSH Configuration (on all servers)

```bash
sudo vim /etc/ssh/sshd_config
```

- Line 38: Uncomment (`#PermitRootLogin yes`)
- Line 63: Change `PasswordAuthentication no` → `yes`

### Step 6: Restart SSH Service (on all servers)

```bash
sudo systemctl restart sshd
```

---

## 🔗 Setup Passwordless SSH

### Step 7: Copy SSH Key from Master to Slaves

```bash
ssh-copy-id root@<slave1-private-ip>
ssh-copy-id root@<slave2-private-ip>
```

> Enter the root password when prompted.

---

## ⚙️ Configure Ansible on Master

### Step 8: Update Ansible Config

```bash
sudo vim /etc/ansible/ansible.cfg
```

- Un-comment the **`inventory`** line to enable custom inventory.

### Step 9: Define Inventory Hosts

```bash
sudo vim /etc/ansible/hosts
```

Example:

```ini
[dev]
slave1-private-ip

[test]
slave2-private-ip
```

---

## ✅ Test the Connection

```bash
ansible all -m ping
```

Expected output:

```bash
slave1 | SUCCESS => ...
slave2 | SUCCESS => ...
```

---

## 📦 Running Playbooks

### Step 10: Clone the GitHub Repository

```bash
git clone https://github.com/perumandlahemakumari/ansible-playbooks.git
cd ansible-playbooks
```

### Step 11: Execute a Playbook

```bash
ansible-playbook -i /etc/ansible/hosts webserver-setup.yml
```

> Replace `webserver-setup.yml` with the playbook you want to run from the repo.

---

## 📌 Notes
- Ensure your EC2 security group allows inbound SSH (port 22).
- Use private IPs in the inventory if all EC2 instances are in the same VPC.
- Make sure you use `root` as the user in your Ansible commands or set `ansible_user=root` in your inventory.

---

## 📘 References
- [Ansible Docs](https://docs.ansible.com/)
- [Amazon EC2 User Guide](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/)
