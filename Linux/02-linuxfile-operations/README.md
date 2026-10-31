# Assignment — Linux File Operations

1.	Create the following directory structure using a single command:   
devops/
└── projects/
    └── batch-90s/
3.	Inside batch-90s/, create a file called notes.txt. Write three lines into it, then append two more. Read it to verify.
4.	Copy notes.txt to projects/ and rename the copy to backup.txt. Verify both exist.
5.	Move backup.txt to /tmp/. What happens to files there when the server restarts?
6.	Delete the devops/ directory and everything inside it.
7.	List the contents of /var/log/ sorted by time. What do you notice? Why would a DevOps engineer check this directory?

**Step 1:** Create the directory structure using a single command
```bash
mkdir -p devops/projects/batch-90s
```
**Explanation:**
- `mkdir` – Creates a directory.
- `-p` – Creates parent directories automatically.
- `devops/projects/batch-90s` – Creates the complete directory structure.

Execute:

```bash
ls -R devops
```

Expected output:

```text
devops:
projects

devops/projects:
batch-90s
```

## Step 2: Create notes.txt and write five lines

First, navigate to the `batch-90s` directory.

```bash
cd devops/projects/batch-90s
```

Create a file and write three lines:

```bash
cat > notes.txt <<EOF
Linux is an operating system.
Linux is widely used in DevOps.
Linux supports automation.
EOF
```

**Explanation:**

- `cat` – Displays or reads file contents.
- `>` – Creates a file or overwrites existing content.
- `EOF` – Marks the end of the input.

Now append two more lines:

```bash
cat >> notes.txt <<EOF
Linux supports shell scripting.
Linux is used to manage servers.
EOF
```

**Explanation:**

- `>>` – Appends content without deleting existing lines.

Read the file:

```bash
cat notes.txt
```

Expected output:

```text
Linux is an operating system.
Linux is widely used in DevOps.
Linux supports automation.
Linux supports shell scripting.
Linux is used to manage servers.
```

**Step 2 completed.**

## Step 3: Copy notes.txt and rename it to backup.txt

Currently, you are inside:

```text
/home/ec2-user/devops/projects/batch-90s
```

Execute:

```bash
cp notes.txt ../backup.txt
```

**Explanation:**

- `cp` – Copies a file.
- `notes.txt` – Original file.
- `../` – Refers to the parent directory (`projects`).
- `backup.txt` – New filename.

Verify the original file:

```bash
ls -l
```

Expected output:

```text
notes.txt
```

Now go to the parent directory:

```bash
cd ..
```

Check:

```bash
ls -l
```

Expected output:

```text
batch-90s
backup.txt
```

Verify the copied file contents:

```bash
cat backup.txt
```

**Step 3 completed.**

## Step 4: Move backup.txt to /tmp/

You are currently inside:

```text
/home/ec2-user/devops/projects
```

Execute:

```bash
mv backup.txt /tmp/
```

**Explanation:**

- `mv` – Moves or renames files.
- `backup.txt` – Source file.
- `/tmp/` – Destination directory.

Verify:

```bash
ls -l /tmp/backup.txt
```

Also verify it is no longer in projects:

```bash
ls -l
```

Expected output:

```text
batch-90s
```

**Interview Question: What happens to files in /tmp when the server restarts?**

**Answer:**

`/tmp` is a temporary directory. Files stored there may be deleted when the server restarts, depending on the Linux system configuration. We should not store important permanent data in `/tmp`.

**Step 4 completed.**

## Step 5: Delete the devops directory and everything inside it

First, navigate to your home directory:

```bash
cd ~
```

Check your location:

```bash
pwd
```

Expected output:

```text
/home/ec2-user
```

Now execute:

```bash
rm -r devops/
```

**Explanation:**

- `rm` – Removes files and directories.
- `-r` – Recursively deletes directories and their contents.
- `devops/` – Directory to delete.

Verify:

```bash
ls -l
```

The `devops` directory should no longer exist.

**Step 5 completed.**

## Step 6: List /var/log contents sorted by time

Execute:

```bash
ls -lt /var/log/
```

**Explanation:**

- `ls` – Lists files and directories.
- `-l` – Displays detailed information.
- `-t` – Sorts by modification time, newest first.
- `/var/log/` – Directory containing system and application logs.

To see the oldest files first:

```bash
ls -ltr /var/log/
```

To see hidden files too:

```bash
ls -latr /var/log/
```

**Why does a DevOps engineer check /var/log?**

A DevOps engineer checks `/var/log` to monitor server activities, troubleshoot application issues, identify errors, and investigate system failures.

**Step 6 completed.**

---

### Linux Commands – Quick Reference

| Command | Purpose |
|---|---|
| `mkdir -p` | Creates nested directories |
| `cd` | Changes directory |
| `pwd` | Displays current directory |
| `cat >` | Creates or overwrites a file |
| `cat >>` | Appends content to a file |
| `cat` | Reads file contents |
| `cp` | Copies files |
| `mv` | Moves or renames files |
| `rm -r` | Deletes directories and contents |
| `ls -R` | Lists directory contents recursively |
| `ls -lt` | Lists files sorted by modification time |
| `ls -ltr` | Lists oldest files first |
| `ls -latr` | Lists all files, including hidden files, oldest first |
