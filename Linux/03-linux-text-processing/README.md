# Linux Assignment – Text Processing

### Create a file with this content:

```text
web-server-01 192.168.1.10 running
web-server-02 192.168.1.11 stopped
db-server-01  192.168.1.20 running
db-server-02  192.168.1.21 running
cache-server  192.168.1.30 stopped
```

### Write a command for each:

1. Show only lines where the server is `running`.
2. Show only lines where the server is NOT `running`.
3. Count how many servers are `running`.
4. Show the first 3 lines. Then show the last 2.
5. Extract only the IP addresses.
6. Extract only the server names.
7. Show only the names of `stopped` servers — in one command.

---

## Solutions
# Assignment – Linux Text Processing

## Step 1: Create a file with the given content

First, create a directory for the assignment:

```bash
mkdir -p devops/text-processing
```
Navigate into the directory:
```bash
cd devops/text-processing
```
Create a file called `servers.txt` with the given content:
```bash
touch servers.txt
```
```bash
cat > servers.txt <<EOF
web-server-01 192.168.1.10 running
web-server-02 192.168.1.11 stopped
db-server-01  192.168.1.20 running
db-server-02  192.168.1.21 running
cache-server  192.168.1.30 stopped
EOF
```

**Explanation:**

- `cat` – Creates or displays file content.
- `>` – Creates a file or overwrites existing content.
- `EOF` – Marks the end of the input.

Verify the file:

```bash
cat servers.txt
```
![Create Server File](<create a server file.png>)
**Expected Output:**

```text
web-server-01 192.168.1.10 running
web-server-02 192.168.1.11 stopped
db-server-01  192.168.1.20 running
db-server-02  192.168.1.21 running
cache-server  192.168.1.30 stopped
```
## Step 2: Show only lines where the server is running

**Command:**

```bash
grep "running" servers.txt
```
**Explanation:**

- `grep` – Searches for specific text in a file.
- `"running"` – Searches for lines containing running.
- `servers.txt` – File to search. 
![Line Servers](<line servers.png>)

**Expected Output:**

```text
web-server-01 192.168.1.10 running
db-server-01  192.168.1.20 running
db-server-02  192.168.1.21 running
```

## Step 3: Show only lines where the server is NOT running

**Command:**

```bash
grep -v "running" servers.txt
```

**Explanation:**

- `grep` – Searches for matching text.
- `-v` – Displays lines that do not match.
- `"running"` – Excludes lines containing running.
![Line Server Not Running](<Line server not running.png>)

**Expected Output:**

```text
web-server-02 192.168.1.11 stopped
cache-server  192.168.1.30 stopped
```

## Step 4: Count how many servers are running

**Command:**

```bash
grep -c "running" servers.txt
```
![Line Server Not Running](<Line server not running.png>)
**Explanation:**

- `grep` – Searches for specific text.
- `-c` – Counts matching lines.
- `"running"` – Searches for running status.

**Expected Output:**

```text
3
```

## Step 5: Show the first 3 lines and last 2 lines

**Show the first 3 lines:**

```bash
head -n 3 servers.txt
```

**Explanation:**

- `head` – Displays the beginning of a file.
- `-n 3` – Displays the first three lines.

**Expected Output:**

```text
web-server-01 192.168.1.10 running
web-server-02 192.168.1.11 stopped
db-server-01  192.168.1.20 running
```

**Show the last 2 lines:**

```bash
tail -n 2 servers.txt
```

**Explanation:**

- `tail` – Displays the end of a file.
- `-n 2` – Displays the last two lines.

**Expected Output:**

```text
db-server-02  192.168.1.21 running
cache-server  192.168.1.30 stopped
```

## Step 6: Extract only the IP addresses

**Command:**

```bash
awk '{print $2}' servers.txt
```

**Explanation:**

- `awk` – Processes and extracts text from files.
- `$1` – First column (server name).
- `$2` – Second column (IP address).
- `$3` – Third column (server status).
- `print` – Displays the selected column.

**Expected Output:**

```text
192.168.1.10
192.168.1.11
192.168.1.20
192.168.1.21
192.168.1.30
```

## Step 7: Extract only the server names

**Command:**

```bash
awk '{print $1}' servers.txt
```

**Explanation:**

- `awk` – Processes text column by column.
- `$1` – Extracts the first column containing server names.

**Expected Output:**

```text
web-server-01
web-server-02
db-server-01
db-server-02
cache-server
```

## Step 8: Show only the names of stopped servers in one command

**Command:**

```bash
awk '$3 == "stopped" {print $1}' servers.txt
```

**Explanation:**

- `awk` – Processes the file.
- `$3` – Checks the third column (server status).
- `==` – Checks whether the value matches.
- `"stopped"` – Searches for stopped status.
- `print $1` – Displays only the server name.

**Expected Output:**

```text
web-server-02
cache-server
```
![Linux File Operations](<linux file operations.png>)

## Linux Text Processing – Quick Reference

| Command | Purpose |
|---|---|
| `grep "running" servers.txt` | Shows running servers |
| `grep -v "running" servers.txt` | Shows non-running servers |
| `grep -c "running" servers.txt` | Counts running servers |
| `head -n 3 servers.txt` | Shows first 3 lines |
| `tail -n 2 servers.txt` | Shows last 2 lines |
| `awk '{print $2}' servers.txt` | Extracts IP addresses |
| `awk '{print $1}' servers.txt` | Extracts server names |
| `awk '$3 == "stopped" {print $1}' servers.txt` | Shows stopped server names |

**Assignment completed successfully.**
