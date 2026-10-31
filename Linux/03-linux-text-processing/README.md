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

### Step 1: Create the file

```bash
cat > servers.txt <<EOF
web-server-01 192.168.1.10 running
web-server-02 192.168.1.11 stopped
db-server-01  192.168.1.20 running
db-server-02  192.168.1.21 running
cache-server  192.168.1.30 stopped
EOF
```

Verify:

```bash
cat servers.txt
```

### 1. Show only running servers

```bash
grep 'running$' servers.txt
```

**Explanation:** `grep` searches for matching text. Here, it displays only lines ending with `running`.

### 2. Show servers that are NOT running

```bash
grep -v 'running$' servers.txt
```

**Explanation:** `-v` reverses the search and displays lines that do not match `running`.

### 3. Count running servers

```bash
grep -c 'running$' servers.txt
```

**Explanation:** `-c` counts the matching lines.

**Output:**
```text
3
```

### 4. Show the first 3 lines and last 2 lines

First 3 lines:

```bash
head -n 3 servers.txt
```

Last 2 lines:

```bash
tail -n 2 servers.txt
```

**Explanation:**
- `head` displays the beginning of a file.
- `tail` displays the end of a file.
- `-n` specifies the number of lines.

### 5. Extract only IP addresses

```bash
awk '{print $2}' servers.txt
```

**Explanation:** `$2` represents the second column, which contains IP addresses.

**Output:**
```text
192.168.1.10
192.168.1.11
192.168.1.20
192.168.1.21
192.168.1.30
```

### 6. Extract only server names

```bash
awk '{print $1}' servers.txt
```

**Explanation:** `$1` represents the first column, which contains server names.

**Output:**
```text
web-server-01
web-server-02
db-server-01
db-server-02
cache-server
```

### 7. Show only names of stopped servers in one command

```bash
awk '$3 == "stopped" {print $1}' servers.txt
```

**Explanation:**
- `$3` checks the third column (server status).
- `== "stopped"` checks whether the status is stopped.
- `print $1` displays only the server name.

**Output:**
```text
web-server-02
cache-server
```

---

## Commands Summary

| Task | Command |
|---|---|
| Running servers | `grep 'running$' servers.txt` |
| Non-running servers | `grep -v 'running$' servers.txt` |
| Count running servers | `grep -c 'running$' servers.txt` |
| First 3 lines | `head -n 3 servers.txt` |
| Last 2 lines | `tail -n 2 servers.txt` |
| Extract IP addresses | `awk '{print $2}' servers.txt` |
| Extract server names | `awk '{print $1}' servers.txt` |
| Stopped server names | `awk '$3 == "stopped" {print $1}' servers.txt` |

## What I Learned

1. How to create and read files in Linux.
2. How to search and filter data using `grep`.
3. How to count matching lines using `grep -c`.
4. How to display specific lines using `head` and `tail`.
5. How to extract columns using `awk`.
6. How to filter data and extract server names in a single command.

**Conclusion:** These Linux text-processing commands are useful in real-time DevOps projects for log analysis, server monitoring, and troubleshooting.
