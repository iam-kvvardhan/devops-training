# Day 3 — Linux Users, Groups & Permissions

## What I Learned

### 1. Users

A **user** represents an individual account in Linux.

```bash
whoami
id
```

- `whoami` → shows the current user
- `id` → shows UID, GID and group memberships
- `UID` → User ID
- `GID` → Group ID

Example:

```text
vinay → UID 1000
root  → UID 0
```

---

### 2. Groups

A **group** is a collection of users used to manage permissions efficiently.

There are two important types:

- **Primary group** → default/main group of a user
- **Secondary group** → additional groups the user belongs to

Example:

```text
developer
├── Primary group → developer
└── Secondary group → devops
```

Real-life example:

> Instead of giving 50 developers access individually, add them to a `devops` group and give the group access to the required directory.

Useful commands:

```bash
groups developer
sudo groupadd devops
sudo useradd -m developer
sudo usermod -aG devops developer
```

---

### 3. Ownership

Every file/directory has an **owner** and a **group owner**.

```bash
ls -l
```

Example:

```text
root devops app.log
```

Means:

```text
Owner → root
Group → devops
```

Change ownership:

```bash
sudo chown root:devops /opt/devops-app
```

---

### 4. Group-Based Permissions

Created:

```text
/opt/devops-app
```

Set ownership:

```text
root:devops
```

Set permissions:

```bash
sudo chmod 770 /opt/devops-app
```

`770` means:

```text
Owner  → rwx
Group  → rwx
Others → ---
```

Since `developer` belongs to `devops`, the developer can access the directory.

```text
developer
    ↓
member of devops
    ↓
directory group = devops
    ↓
group permission = rwx
    ↓
access ✅
```

---

### 5. Permission Troubleshooting

When `vinay` received:

```text
Permission denied
```

for:

```bash
ls -l /opt/devops-app
```

I checked:

```bash
ls -ld /opt/devops-app
ls -ld /opt
```

Then checked group membership.

The problem was:

```text
vinay → not a member of devops
developer → member of devops
```

Therefore, `developer` had access through the group permissions while `vinay` did not.

**Important:** Don't blindly use `chmod 777`. First identify the required access and give the minimum necessary permissions.

---

### 6. sudo vs su

**sudo**

Run a specific command with elevated privileges:

```bash
sudo command
```

Run a command as another user:

```bash
sudo -u developer whoami
```

**su**

Switch the current shell to another user:

```bash
su - developer
```

Return to the previous shell:

```bash
exit
```

### Easy way to remember

```text
sudo → Run a command
su   → Switch user
```

---

## Commands to Remember

| Command | Purpose |
|---|---|
| `whoami` | Current user |
| `id` | UID, GID & groups |
| `groups` | Group membership |
| `useradd` | Create user |
| `groupadd` | Create group |
| `usermod -aG` | Add user to secondary group |
| `ls -l` | Check permissions/ownership |
| `chmod` | Change permissions |
| `chown` | Change ownership |
| `sudo` | Run with elevated privileges |
| `su` | Switch user |

## Key Takeaway

```text
User
 ↓
Group membership
 ↓
File/Directory ownership
 ↓
Permissions
 ↓
Access
```

**Day 3 — Completed ✅**
