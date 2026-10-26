# Day 3 — Linux Users, Groups, Ownership & sudo

## Topics Covered

- Linux users
- UID and GID
- Primary groups
- Secondary (supplementary) groups
- User and group management
- File and directory ownership
- Group-based access control
- `sudo`
- `su`
- Permission troubleshooting

---

## 1. Linux Users

A Linux user represents an individual account that can interact with the system.

Every user has a unique numeric **UID (User ID)**.

For example:

```text
vinay → UID 1000
root  → UID 0
```

The `root` user has UID `0` and has administrative privileges.

### Real-life example

In a company server, different people or services may have different accounts:

```text
developer
devopsadmin
jenkins
www-data
```

Each account can have different permissions and access.

---

## 2. UID and GID

### UID

UID stands for **User ID**.

It is the numeric identifier used by Linux to identify a user.

### GID

GID stands for **Group ID**.

It is the numeric identifier used by Linux to identify a group.

The `id` command can display this information:

```text
uid=1000(vinay) gid=1000(vinay) groups=...
```

---

## 3. Primary Group

Every Linux user has one **primary group**.

The primary group is the user's default group.

For example:

```text
User: developer
Primary group: developer
```

When a user creates a new file, the file is commonly associated with the user's primary group.

### Real-life example

If a developer creates:

```text
application.log
```

the file might look like:

```text
developer developer application.log
```

The first `developer` is the owner and the second `developer` is the group owner.

---

## 4. Secondary Groups

A user can also belong to additional groups called **secondary/supplementary groups**.

For example:

```text
developer
├── Primary group: developer
└── Secondary group: devops
```

Secondary groups are commonly used to provide additional access to resources.

### Real-life example

Imagine 50 developers need access to application deployment files.

Instead of changing permissions for each developer individually, an administrator can create a group:

```text
devops
```

and add the required developers to it.

The directory can then be configured with:

```text
Owner: root
Group: devops
```

This makes access management much easier.

---

## 5. Viewing User and Group Information

### Check current user

```bash
whoami
```

### View detailed user and group information

```bash
id
```

### View groups

```bash
groups
```

### View groups for a specific user

```bash
groups developer
```

---

## 6. User and Group Management

Created a Linux group:

```bash
sudo groupadd devops
```

Created a user:

```bash
sudo useradd -m developer
```

Created a password:

```bash
sudo passwd developer
```

Added the developer to the `devops` secondary group:

```bash
sudo usermod -aG devops developer
```

### Important

The `-aG` option means:

- `-a` → append
- `-G` → supplementary/secondary groups

Using `-a` prevents accidentally replacing the user's existing supplementary group memberships.

Verified the membership using:

```bash
groups developer
```

Output showed:

```text
developer : developer devops
```

Therefore:

```text
Primary group   → developer
Secondary group → devops
```

---

## 7. File and Directory Ownership

Linux files and directories have an owner and a group owner.

Example:

```text
root devops
```

means:

```text
Owner → root
Group → devops
```

Ownership can be changed using `chown`.

Example:

```bash
sudo chown root:devops /opt/devops-app
```

This sets:

```text
Owner → root
Group → devops
```

---

## 8. Group-Based Directory Access

Created an application directory:

```bash
sudo mkdir -p /opt/devops-app
```

Created an application log:

```bash
sudo touch /opt/devops-app/app.log
```

Changed the directory ownership:

```bash
sudo chown root:devops /opt/devops-app
```

Configured directory permissions:

```bash
sudo chmod 770 /opt/devops-app
```

The resulting permissions were:

```text
drwxrwx---
```

Meaning:

```text
Owner  → rwx
Group  → rwx
Others → ---
```

Since `developer` belongs to the `devops` group, the developer receives the group permissions.

---

## 9. Permission Troubleshooting

When `vinay` tried:

```bash
ls -l /opt/devops-app
```

the system returned:

```text
Permission denied
```

Instead of immediately changing the permissions, I investigated the problem.

Checked the directory:

```bash
ls -ld /opt/devops-app
```

Output:

```text
drwxrwx--- root devops /opt/devops-app
```

Checked the parent directory:

```bash
ls -ld /opt
```

Output showed that `/opt` itself was accessible.

Then checked group membership.

`vinay` was not a member of `devops`, while:

```text
developer : developer devops
```

showed that `developer` was a member of the `devops` group.

Therefore:

```text
vinay
  ↓
Not a member of devops
  ↓
Others permissions = ---
  ↓
Permission denied
```

Whereas:

```text
developer
  ↓
Member of devops
  ↓
Group permissions = rwx
  ↓
Access granted
```

---

## 10. Testing Access as Developer

Tested access using:

```bash
sudo -u developer ls -l /opt/devops-app
```

The developer was able to access the directory.

Created a file:

```bash
sudo -u developer touch /opt/devops-app/test.txt
```

Verified the result:

```bash
sudo -u developer ls -l /opt/devops-app
```

The file was successfully created.

This demonstrated how **group membership + directory permissions + ownership** work together.

---

## 11. sudo

`sudo` allows an authorized user to execute a command with elevated privileges.

Example:

```bash
sudo apt update
```

The user remains logged in as their normal account, but the particular command runs with elevated privileges.

### Real-life example

A DevOps engineer may normally work as:

```text
vinay
```

but need administrative privileges to:

- Install packages
- Modify system configuration
- Restart services
- Change ownership
- Change permissions

Instead of permanently logging in as root, they can use `sudo` for administrative commands.

---

## 12. sudo -u

`sudo` can also execute a specific command as another user.

Example:

```bash
sudo -u developer whoami
```

Output:

```text
developer
```

The command runs as `developer`, but the current shell remains the original user.

---

## 13. su

`su` means **switch user**.

Example:

```bash
su - developer
```

This switches the current shell to the `developer` user.

After switching:

```bash
whoami
```

returns:

```text
developer
```

To return to the previous shell:

```bash
exit
```

### Difference

```text
sudo -u developer command
        ↓
Run one command as developer


su - developer
        ↓
Switch the current shell to developer
```

---

## 14. Important Commands Practiced

| Command | Purpose |
|---|---|
| `whoami` | Show current user |
| `id` | Show UID, GID and group memberships |
| `groups` | Show group memberships |
| `useradd` | Create a user |
| `passwd` | Set/change a user's password |
| `groupadd` | Create a group |
| `usermod -aG` | Add a user to a supplementary group |
| `chown` | Change ownership |
| `chmod` | Change permissions |
| `sudo` | Execute a command with elevated privileges |
| `sudo -u` | Execute a command as another user |
| `su` | Switch user |

---

## Key Learnings

1. A Linux user represents an individual account.
2. A group is a collection of users used to manage access efficiently.
3. Every user has one primary group.
4. A user can belong to multiple secondary/supplementary groups.
5. UID identifies a user numerically.
6. GID identifies a group numerically.
7. File access depends on ownership, group membership and permissions.
8. `chown` changes ownership.
9. `chmod` changes permissions.
10. `sudo` provides temporary elevated privileges for authorized commands.
11. `sudo -u` can execute a specific command as another user.
12. `su` switches the current shell to another user.
13. Group-based permissions are useful for managing access for multiple users.
14. Permission problems should be investigated before changing permissions.

---

## Interview Takeaway

When troubleshooting a Linux permission issue, I should not immediately use commands such as `chmod 777`.

A better approach is:

```text
1. Identify the current user
        ↓
2. Check file/directory permissions
        ↓
3. Check ownership
        ↓
4. Check group membership
        ↓
5. Check parent directory permissions
        ↓
6. Identify the actual cause
        ↓
7. Apply the minimum required permission
```

## Status

Day 3 — Completed ✅
