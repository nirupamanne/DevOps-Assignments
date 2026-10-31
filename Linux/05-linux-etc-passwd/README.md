# Assignment – /etc/passwd Deep Dive

## Assignment Questions

1. How many user accounts exist on the system?
2. Extract only the usernames.
3. Show only users with UID greater than 999.
4. Find the entry for `ec2-user`. What is their home directory?
5. List all unique login shells assigned to users on the system.

---

## Solutions

### Step 1: How many user accounts exist on the system?

**Command:**

```bash
cat /etc/passwd | wc -l
```

**Explanation:**

- `cat /etc/passwd` – Displays all user account entries.
- `|` – Sends the output to the next command.
- `wc -l` – Counts the total number of lines.

Each line in `/etc/passwd` represents one user account entry.

**Expected Output:**

```text
Number of user accounts
```

**Alternative command:**

```bash
wc -l /etc/passwd
```
![User Account Exist](<user account exist.png>)

### Step 2: Extract only the usernames

**Command:**

```bash
cut -d: -f1 /etc/passwd
```

**Explanation:**

- `cut` – Extracts specific fields from a file.
- `-d:` – Uses `:` as the delimiter.
- `-f1` – Extracts the first field.
- `/etc/passwd` – User account information file.

The first field in `/etc/passwd` contains the username.
![Extract User Account Names](<extract-user account names.png>)
**Expected Output:**

```text
root
bin
daemon
adm
ec2-user
```

*The actual usernames depend on your EC2 instance.*

### Step 3: Show only users with UID greater than 999

**Command:**

```bash
awk -F: '$3 > 999 {print $1}' /etc/passwd
```
![Show UID](<show UID.png>)
**Explanation:**

- `awk` – Processes text and extracts specific fields.
- `-F:` – Uses `:` as the field separator.
- `$3` – Represents the third field (UID).
- `> 999` – Checks whether the UID is greater than 999.
- `{print $1}` – Displays the first field (username).

**Expected Output:**

```text
ec2-user
```

*Your actual output may contain additional users depending on your system.*

### Step 4: Find the entry for ec2-user. What is their home directory?

**Command:**

```bash
grep "^ec2-user:" /etc/passwd
```

**Explanation:**

- `grep` – Searches for matching text.
- `^` – Matches the beginning of a line.
- `ec2-user` – Searches for the specific username.
- `:` – Ensures the username field matches exactly.
![Find EC2 User](<find ec2 user.png>)
**Expected Output Format:**

```text
ec2-user:x:1000:1000:EC2 Default User:/home/ec2-user:/bin/bash
```

*The exact UID, GID, description, and shell may vary.*

**To extract only the home directory:**

```bash
grep "^ec2-user:" /etc/passwd | cut -d: -f6
```

**Expected Output:**

```text
/home/ec2-user
```

**Explanation:**

- `cut -d:` – Uses `:` as the delimiter.
- `-f6` – Extracts the sixth field.
- The sixth field represents the user's home directory.

### Step 5: List all unique login shells assigned to users on the system

**Command:**

```bash
cut -d: -f7 /etc/passwd | sort -u
```
![List Unique Shell](<List unique shell.png>)
**Explanation:**

- `cut -d:` – Uses `:` as the field separator.
- `-f7` – Extracts the seventh field (login shell).
- `sort` – Sorts the output.
- `-u` – Removes duplicate entries and displays unique values.

**Expected Output:**

```text
/bin/bash
/bin/nologin
/bin/sync
/sbin/nologin
```

*The actual shells depend on the users configured on your EC2 instance.*

---

## Quick Reference – Commands Used

| Command | Purpose |
|---|---|
| `cat /etc/passwd \| wc -l` | Counts user account entries |
| `cut -d: -f1 /etc/passwd` | Extracts usernames |
| `awk -F: '$3 > 999 {print $1}' /etc/passwd` | Displays users with UID greater than 999 |
| `grep "^ec2-user:" /etc/passwd` | Finds the ec2-user entry |
| `cut -d: -f6` | Extracts the home directory |
| `cut -d: -f7 /etc/passwd` | Extracts login shells |
| `sort -u` | Displays unique sorted values |

## Assignment Completed
