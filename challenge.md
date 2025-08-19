# 🚀 Day 14 – Linux User, Group & Permission Management

---

## 🔑 Introduction

Welcome to Day 14 of the Daily DevOps + SRE Challenge Series – Season 2! 🔐
Today, you’ll master user, group, and permission management in Linux.
You’ll also learn to organize access control, enforce security with permissions, and troubleshoot ownership/permission issues like an SRE under pressure.

User, group, and permission management is the backbone of Linux administration, CI/CD pipelines, remote debugging, and automated operations. Without them, systems quickly become insecure and unmanageable.

Linux is a **multi-user operating system**. Unlike personal laptops, servers are shared by multiple users, applications, and services. To keep systems **secure, reliable, and scalable**, we must carefully manage:

* **Who** can log in (Users)
* **What team or role** they belong to (Groups)
* **What files, processes, and directories** they can access (Permissions)

Without proper user and permission management, systems can face **security breaches, downtime, or compliance failures**.

---

## ❓ Why Is This Needed?

* **Security** → Prevents unauthorized access and privilege misuse
* **Reliability** → Keeps apps running with correct ownership and permissions
* **Collaboration** → Teams can share resources safely using groups
* **Compliance** → Enforces password policies, access expiry, and auditing
* **Troubleshooting** → Permissions are a common cause of outages

📌 Example: A website crashes because `/var/www/html` files were owned by the wrong user. Knowing user/group/permission management helps you fix it instantly.

---

## 📘 Foundations

### 🔹 What is a User?

A **user** represents an identity in Linux. Each user has:

* A **username** (e.g., `riya`)
* A **UID** (User ID)
* A **home directory** (`/home/riya`)
* A **shell** (`/bin/bash`)

📌 Purpose → Controls **who** is accessing the system.

---

### 🔹 What is a Group?

A **group** is a collection of users. Instead of giving permissions to each user, you assign them to a group.

📌 Example → A `devops-team` group can access `/projects/infrastructure`.

📌 Purpose → Simplifies **role-based access control**.

---

### 🔹 What are Permissions?

Linux files/directories have **3 types of permissions** for **3 categories** of users:

* **Types of Permissions:**

  * `r` → Read (view contents)
  * `w` → Write (modify or delete)
  * `x` → Execute (run file or enter directory)

* **Categories of Users:**

  * **Owner** (user who created file)
  * **Group** (users in the same group)
  * **Others** (everyone else)

📌 Purpose → Defines **what actions are allowed** on files and directories.

---

### 🔹 Special Permission Bits

* **SetUID (4xxx)** → Program runs with file owner’s privileges (useful for root-owned binaries).
* **SetGID (2xxx)** → New files inherit group ownership of the directory.
* **Sticky Bit (1xxx)** → Users can delete only their own files in a shared directory.

---

## 📖 Important Commands & Their Purpose

| Command   | Purpose                             | Example                    |
|-----------|-------------------------------------|----------------------------|
| `useradd` | Create a new user                   | `useradd -m riya`          |
| `passwd`  | Set/modify user password            | `passwd riya`              |
| `usermod` | Modify user (add group, lock, etc.) | `usermod -aG interns riya` |
| `groupadd`| Create a new group                  | `groupadd devops-team`     |
| `groups`  | Show user’s groups                  | `groups riya`              |
| `id`      | Show UID, GID, groups               | `id riya`                  |
| `chown`   | Change file owner/group             | `chown user:group file`    |
| `chmod`   | Change permissions                  | `chmod 755 file.sh`        |
| `ls -l`   | View permissions                    | `ls -l /etc/passwd`        |
| `chage`   | Manage password expiry              | `chage -M 90 riya`         |
---

## 🛠️ Scenarios with Hands-On Steps

---

### **Scenario 1: Adding a New Intern (Riya)**

**Goal:** Give Riya her own account and private home directory.

```bash
# Create user with home directory
useradd -m riya

# Set password
passwd riya

# Create group interns
groupadd interns

# Add riya to interns group
usermod -aG interns riya

# Make home private
chmod 700 /home/riya
```

✅ Riya has a secure account with group access.

---

### **Scenario 2: Shared Project Directory**

**Goal:** Amit, Neha, Vikram collaborate on `/projects/infrastructure`.

```bash
# Create group
groupadd devops-team

# Add users to group
usermod -aG devops-team amit
usermod -aG devops-team neha
usermod -aG devops-team vikram

# Assign directory to group
chown -R :devops-team /projects/infrastructure

# Apply setgid so new files inherit group
chmod 2770 /projects/infrastructure
```

✅ New files belong to `devops-team` automatically.

---

### **Scenario 3: Security Audit & Hardening**

**Goal:** Lock unused accounts, disable root login, enforce password policy.

```bash
# Find system accounts with login shells
awk -F: '$3<1000 && $7!="/usr/sbin/nologin"{print $1,$3,$7}' /etc/passwd

# Lock unused account
usermod -L guest

# Disable root login in SSH
sed -i 's/^PermitRootLogin.*/PermitRootLogin no/' /etc/ssh/sshd_config
systemctl restart sshd

# Password policy for riya
chage -M 90 -m 7 -W 14 riya
```

✅ System is harder to exploit.

---

### **Scenario 4: Fixing Web Server Permissions**

**Goal:** Restore `/var/www/html` after wrong ownership.

```bash
# Correct ownership
chown -R www-data:www-data /var/www/html

# Create webdev group
groupadd webdev

# Add devs
usermod -aG webdev suresh
usermod -aG webdev kiran

# Set group inheritance
chmod -R 2775 /var/www/html
```

✅ Website works, devs collaborate safely.

---

### **Scenario 5: Temporary Contractor Account**

**Goal:** Contractor Anjali needs access for 7 days only.

```bash
# Create user with expiry
useradd -e $(date -d "7 days" +"%Y-%m-%d") anjali

# Restrict workspace
mkdir -p /opt/contractor-workspace
chown anjali:anjali /opt/contractor-workspace
chmod 700 /opt/contractor-workspace
```

✅ Account auto-expires, workspace isolated.

---

### **Scenario 6: Privileged Backup Script**

**Goal:** Allow trusted users to run backup as root.

```bash
# Create group
groupadd backup-operators

# Add users
usermod -aG backup-operators ramesh
usermod -aG backup-operators priya

# Apply setuid
chown root:backup-operators /usr/local/bin/backup-system.sh
chmod 4750 /usr/local/bin/backup-system.sh
```

✅ Only trusted users can run backup as root.

---

## Join Our Community
To make the most of this journey, connect with fellow learners:
- **Discord** – Ask questions and collaborate: https://discord.gg/mNDm39qB8t
- **Google Group** – Get updates and discussions: https://groups.google.com/forum/#!forum/daily-devops-sre-challenge-series/join
- **YouTube** – Watch solution videos and tutorials: https://www.youtube.com/@Sagar.Utekar

---

## Stay Motivated!
Every challenge you complete brings you closer to mastering Git, DevOps, and SRE practices. Keep pushing your limits and collaborating with the community.

Happy Learning!

**Best regards,**  
Sagar Utekar
