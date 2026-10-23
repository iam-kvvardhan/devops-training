# Day 2 — Linux File Permissions

## Topics Covered

- Linux file permissions
- Read, write and execute permissions
- User, group and others
- Numeric permission notation
- chmod
- chown
- File vs directory permissions
- Basic permission troubleshooting

## Permission Model

Linux permissions are divided into three categories:

- User/Owner
- Group
- Others

The three basic permissions are:

- r — Read
- w — Write
- x — Execute

## Numeric Permissions

| Permission | Value |
|---|---:|
| Read (r) | 4 |
| Write (w) | 2 |
| Execute (x) | 1 |

### 755

**Owner:** rwx

**Group:** r-x

**Others:** r-x

### 644

**Owner:** rw-

**Group:** r--

**Others:** r--

### 600

**Owner:** rw-

**Group:** ---

**Others:** ---

## Hands-on Lab

Created the following files:

- private.txt
- public.txt
- script.sh

Configured permissions as:

- private.txt → 600
- public.txt → 644
- script.sh → 755

Verified the permissions using ls -l.

## Script Execution

The script was executed using ./script.sh.

Output:

DevOps Day 2

## Ownership Verification

Used whoami to verify the current Linux user.

Current user: vinay

## Key Learnings

- chmod is used to modify file or directory permissions.
- chown is used to modify ownership.
- 755 gives the owner read/write/execute permissions and group/others read/execute permissions.
- 644 gives the owner read/write permissions and group/others read permissions.
- 600 gives the owner read/write permissions while denying group and others access.
- File and directory permissions have different meanings, particularly for the x permission on directories.

## Interview Takeaway

When troubleshooting a permission issue, first inspect the file's ownership and permissions using ls -l rather than immediately changing permissions.

## Status

Completed
