# Assignment – Linux File Permissions and Ownership

## Assignment Questions

1. Create `secret.txt` with some content. Who can read it by default?
2. Restrict it so only the owner can read and write. Switch to another user and try to read it — what happens?
3. Create `deploy.sh` with one line of content. Make it executable by the owner only. What numeric value represents owner read+write+execute?
4. Create `shared/`. Owner has full access, group can read and execute, others have none — set this in numeric form.
5. Change the owner of `secret.txt` to `alice` and the group to `devops-team`.
6. Create `project/` with 3 files inside. Change ownership of everything in it to `alice:devops-team` in one command.
7. Check the permissions on `/etc/shadow`. Why is it set that way?

---

## Solutions

### Step 1: Create secret.txt with some content

**1. Create a working directory**

```bash
mkdir -p /tmp/permissions-lab
cd /tmp/permissions-lab
```

**2. Create the file**

```bash
echo "This is my secret file." > secret.txt
```

**3. Check the file permissions**

```bash
ls -l secret.txt
```

**Expected Output Example:**

```text
-rw-r--r-- 1 root root 24 Nov 1 10:00 secret.txt
```

**Explanation:**

- `-rw-r--r--` – File permission.
- `rw-` – Owner can read and write.
- `r--` – Group can read.
- `r--` – Others can read.

**Answer:**

By default, a regular file is commonly created with `644` permissions when the system's umask is `022`.

This means the owner can read and write, while the group and others can only read. Actual permissions depend on the system's umask.
![Create Secret](<create secret-1.png>)

### Step 2: Restrict the file so only the owner can read and write

**1. Change the permissions**

```bash
chmod 600 secret.txt
```

**2. Verify the permissions**

```bash
ls -l secret.txt
```

**Expected Output:**

```text
-rw------- 1 root root 24 Nov 1 10:00 secret.txt
```

**Explanation:**

- `chmod` – Changes file permissions.
- `600` – Owner has read and write permissions.
- Group has no permissions.
- Others have no permissions.

**3. Switch to another user**

```bash
su - alice
```

**4. Try to read the file**

```bash
cat /tmp/permissions-lab/secret.txt
```

**Expected Output:**

```text
cat: /tmp/permissions-lab/secret.txt: Permission denied
```

**5. Return to the root user**

```bash
exit
```

**Answer:**

Only the file owner can read and write the file. Another user, such as alice, cannot read it because group and others have no permissions.
![Restrict Permissions](<restrict permissions-2.png>)

### Step 3: Create deploy.sh and make it executable by the owner only

**1. Create the file**

```bash
echo '#!/bin/bash' > deploy.sh
```

**2. Add one line of content**

```bash
echo 'echo "Deployment started"' >> deploy.sh
```

**3. Set permissions**

```bash
chmod 700 deploy.sh
```

**4. Verify permissions**

```bash
ls -l deploy.sh
```

**Expected Output:**

```text
-rwx------ 1 root root 42 Nov 1 10:00 deploy.sh
```

**Explanation:**

- `chmod` – Changes file permissions.
- `7` – Read, write, and execute.
- `0` – No permissions for group.
- `0` – No permissions for others.

**Numeric Permission Values:**

| Permission | Numeric Value |
|---|---:|
| Read (r) | 4 |
| Write (w) | 2 |
| Execute (x) | 1 |
| Read + Write | 6 |
| Read + Execute | 5 |
| Read + Write + Execute | 7 |

**Answer:**

The numeric value for owner read + write + execute is `7`.

The complete permission is `700`.

**5. Execute the script**

```bash
./deploy.sh
```

**Expected Output:**

```text
Deployment started
```
![Create Deploy](<create deploy-3.png>)

### Step 4: Create shared/ with specific permissions

**1. Create the directory**

```bash
mkdir shared
```

**2. Set permissions**

```bash
chmod 750 shared
```

**3. Verify permissions**

```bash
ls -ld shared
```

**Expected Output:**

```text
drwxr-x--- 2 root root 6 Nov 1 10:00 shared
```

**Explanation:**

- `7` – Owner has read, write, and execute permissions.
- `5` – Group has read and execute permissions.
- `0` – Others have no permissions.

**Answer:**

The numeric permission is `750`.

| User Category | Permission | Numeric Value |
|---|---|---:|
| Owner | Read + Write + Execute | 7 |
| Group | Read + Execute | 5 |
| Others | No permissions | 0 |

![Directory Shared](<directory shared -4.png>)

### Step 5: Change the owner of secret.txt to alice and group to devops-team

**Command:**

```bash
chown alice:devops-team secret.txt
```

**Explanation:**

- `chown` – Changes file ownership.
- `alice` – New file owner.
- `devops-team` – New group owner.
- `secret.txt` – File name.

**Verify ownership:**

```bash
ls -l secret.txt
```

**Expected Output Example:**

```text
-rw------- 1 alice devops-team 24 Nov 1 10:00 secret.txt
```

**Answer:**

The file owner is now `alice`, and the group owner is `devops-team`.

![DevOps Team Alice Ownership](<devops team alice ownership-5.png>)

### Step 6: Create project/ with 3 files and change ownership in one command

**1. Create the project directory**

```bash
mkdir project
```

**2. Create three files**

```bash
touch project/file1.txt
touch project/file2.txt
touch project/file3.txt
```

**3. Verify the files**

```bash
ls -l project/
```

**Expected Output:**

```text
file1.txt
file2.txt
file3.txt
```

**4. Change ownership of everything inside project/**

```bash
chown -R alice:devops-team project/
```
```bash
chown -R alice:devops-team .
```

**Explanation:**

- `chown` – Changes ownership.
- `-R` – Recursively changes ownership of the directory and everything inside it.
- `alice` – New owner.
- `devops-team` – New group.
- `project/` – Target directory.
- `.` – Means the current directory.

**5. Verify ownership**

```bash
ls -l project/
```

**Expected Output Example:**

```text
-rw-r--r-- 1 alice devops-team 0 Nov 1 10:00 file1.txt
-rw-r--r-- 1 alice devops-team 0 Nov 1 10:00 file2.txt
-rw-r--r-- 1 alice devops-team 0 Nov 1 10:00 file3.txt
```

**Answer:**

The `chown -R` command changes the ownership of the directory and all files inside it in one command.

![Restrict Permissions](<restrict permissions-2.png>)

### Step 7: Check the permissions on /etc/shadow

**1. Check the permissions**

```bash
ls -l /etc/shadow
```

**Expected Output Example:**

```text
---------- 1 root root 900 Nov 1 10:00 /etc/shadow
```

The exact permissions and group may vary depending on the Linux distribution.

**2. Check the file details**

```bash
stat /etc/shadow
```

**Explanation:**

- `/etc/shadow` – Stores password hashes and password-related account information.
- `ls -l` – Displays file permissions and ownership.
- `stat` – Displays detailed file information.
![Shadow Permissions](<shadow permissions-7.png>)

**Why is it protected?**

The `/etc/shadow` file contains sensitive password information.

If regular users could read this file, attackers could obtain password hashes and attempt offline password-cracking attacks.

Therefore, access is restricted so that only root or specifically authorized system processes can read it.

---

## Quick Reference – Commands Used

| Command | Purpose |
|---|---|
| `touch filename` | Creates an empty file |
| `echo "text" > filename` | Creates a file with content |
| `ls -l` | Displays file permissions |
| `chmod 600 secret.txt` | Gives owner read and write permissions |
| `chmod 700 deploy.sh` | Gives owner full permissions |
| `chmod 750 shared` | Gives owner full access and group read and execute |
| `chown alice:devops-team secret.txt` | Changes file owner and group |
| `chown -R alice:devops-team project/` | Changes ownership recursively |
| `ls -ld directory` | Displays directory permissions |
| `stat /etc/shadow` | Displays detailed file information |

## Assignment Completed
