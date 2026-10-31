# Linux Assignment – Text Processing

**Create a file with this content:**
web-server-01 192.168.1.10 running
web-server-02 192.168.1.11 stopped
db-server-01  192.168.1.20 running
db-server-02  192.168.1.21 running
cache-server  192.168.1.30 stopped
Write a command for each:

1.Show only lines where the server is running.
2.Show only lines where the server is NOT running.
3.Count how many servers are running.
4. Show the first 3 lines. Then show the last 2.
5.Extract only the IP addresses.
6.Extract only the server names.
7.Show only the names of stopped servers — in one command.

---

## Step 1: Create a file with server details

First, create a file called `servers.txt`.

**Command:**

```bash
cat > servers.txt <<EOF
web-server-01 192.168.1.10 running
web-server-02 192.168.1.11 stopped
db-server-01  192.168.1.20 running
db-server-02  192.168.1.21 running
cache-server  192.168.1.30 stopped
EOF
```

**Human explanation:**

- `cat` – Used to create and display file content.
- `>` – Creates a new file or overwrites an existing file.
- `EOF` – Marks the end of the input.

**Verify the file:**

```bash
cat servers.txt
```

**Expected output:**

```text
web-server-01 192.168.1.10 running
web-server-02 192.168.1.11 stopped
db-server-01  192.168.1.20 running
db-server-02  192.168.1.21 running
cache-server  192.168.1.30 stopped
```

---

## Step 2: Show only servers that are running

**Command:**

```bash
grep 'running$' servers.txt
```

**Human explanation:**

`grep` searches for specific text inside a file.

Here, we are searching for lines that end with the word `running`.

**Expected output:**

```text
web-server-01 192.168.1.10 running
db-server-01  192.168.1.20 running
db-server-02  192.168.1.21 running
```

---

## Step 3: Show servers that are NOT running

**Command:**

```bash
grep -v 'running$' servers.txt
```

**Human explanation:**

`-v` means reverse the search.

Instead of showing running servers, it displays all lines that do not end with `running`.

**Expected output:**

```text
web-server-02 192.168.1.11 stopped
cache-server  192.168.1.30 stopped
```

---

## Step 4: Count how many servers are running

**Command:**

```bash
grep -c 'running$' servers.txt
```

**Human explanation:**

`-c` counts how many lines match the search.

Here, it counts the number of running servers.

**Expected output:**

```text
3
```

There are 3 running servers.

---

## Step 5: Show the first 3 lines and last 2 lines

**First 3 lines:**

```bash
head -n 3 servers.txt
```

**Human explanation:**

`head` displays the beginning of a file.

`-n 3` means show only the first 3 lines.

**Expected output:**

```text
web-server-01 192.168.1.10 running
web-server-02 192.168.1.11 stopped
db-server-01  192.168.1.20 running
```

**Last 2 lines:**

```bash
tail -n 2 servers.txt
```

**Human explanation:**

`tail` displays the ending of a file.

`-n 2` means show only the last 2 lines.

**Expected output:**

```text
db-server-02  192.168.1.21 running
cache-server  192.168.1.30 stopped
```

---

## Step 6: Extract only the IP addresses

**Command:**

```bash
awk '{print $2}' servers.txt
```

**Human explanation:**

`awk` is used to extract specific columns from a file.

Here:

- `$1` represents the first column (server name).
- `$2` represents the second column (IP address).
- `$3` represents the third column (server status).

We are printing only the second column.

**Expected output:**

```text
192.168.1.10
192.168.1.11
192.168.1.20
192.168.1.21
192.168.1.30
```

---

## Step 7: Extract only the server names

**Command:**

```bash
awk '{print $1}' servers.txt
```

**Human explanation:**

We use `awk` to extract the first column.

Here, the first column contains the server names.

**Expected output:**

```text
web-server-01
web-server-02
db-server-01
db-server-02
cache-server
```

---

## Step 8: Show only the names of stopped servers using one command

**Command:**

```bash
awk '$3 == "stopped" {print $1}' servers.txt
```

**Human explanation:**

Here we use `awk` to check the third column.

- `$3 == "stopped"` checks whether the server status is stopped.
- `print $1` displays only the server name.

Both filtering and extracting happen in a single command.

**Expected output:**

```text
web-server-02
cache-server
```

---

## Step 9: Complete Command Revision

| Requirement | Linux command |
|---|---|
| Show running servers | `grep 'running$' servers.txt` |
| Show non-running servers | `grep -v 'running$' servers.txt` |
| Count running servers | `grep -c 'running$' servers.txt` |
| First 3 lines | `head -n 3 servers.txt` |
| Last 2 lines | `tail -n 2 servers.txt` |
| Extract IP addresses | `awk '{print $2}' servers.txt` |
| Extract server names | `awk '{print $1}' servers.txt` |
| Names of stopped servers | `awk '$3 == "stopped" {print $1}' servers.txt` |

---

## Conclusion

In this assignment, I learned how to:

1. Create and read text files.
2. Filter running and stopped servers using `grep`.
3. Count matching lines.
4. Display the first and last lines using `head` and `tail`.
5. Extract specific columns using `awk`.
6. Filter and extract server names using a single command.

These Linux text-processing commands are useful for daily DevOps activities, log analysis, and troubleshooting.
