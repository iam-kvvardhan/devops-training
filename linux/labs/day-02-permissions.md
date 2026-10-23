# Day 2 — Linux File Permissions

## Topics Covered

- Linux file permissions
- Read, write and execute permissions
- User, group and others
- Numeric permission notation
- `chmod`
- `chown`
- File vs directory permissions
- Basic permission troubleshooting

## Permission Model

Linux permissions are divided into three categories:

- User/Owner
- Group
- Others

The three basic permissions are:

- `r` — Read
- `w` — Write
- `x` — Execute

## Numeric Permissions

| Permission | Value |
|---|---:|
| Read (`r`) | 4 |
| Write (`w`) | 2 |
| Execute (`x`) | 1 |

Examples:

### 755

```text
Owner  → rwx
Group  → r-x
Others → r-x


### 644

Owner  → rw-
Group  → r--
Others → r--


### 600

```text
Owner  → rw-
Group  → ---
Others → ---

## Hands-on Lab

``` text

permissions-lab/
├── private.txt -> 600
├── public.txt  -> 644
└── script.sh   -> 755

## Key Learnings

1. chmod is used to modify file or directory permissions.
2. chown is used to modify ownership.
3. 755 gives the owner read/write/execute permissions and group/others read/execute permissions.
4. 644 gives the owner read/write permissions and group/others read permissions.
5. 600 gives the owner read/write permissions while denying group and others access.
6. File and directory permissions have different meanings, particularly for the x permission on directories.

## Interview Takeaway

When troubleshooting a permission issue, first inspect the file's ownership and permissions using ls -l rather than immediately changing permissions.


