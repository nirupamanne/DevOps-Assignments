# Linux Assignment – wget, curl, and Pipes

## Assignment Questions

### Download URL

Use the following URL for the assignment:

```text
https://raw.githubusercontent.com/daws-90s/notes/refs/heads/main/session-02.txt
```
### Write a command for each:
1. Download the URL to your EC2 instance and save it as `session-notes.txt`.
2. Fetch the same file using `curl` and count the number of lines without saving it.
3. Fetch the file and print only the lines containing `Linux` in one command.
4. Run the following command and explain what `$NF` represents.
   ```bash
   echo "https://raw.githubusercontent.com/daws-90s/notes/refs/heads/main/session-02.txt" | awk -F "/" '{print $NF}'
   ```
6. Fetch the file, filter lines containing `ssh`, and count the matched lines using two pipes.
---

## Solutions
# Assignment – Linux wget, curl, and Pipes

## Step 1: Download the file using wget
First, create a directory for the assignment:

```bash
mkdir -p devops/wget-curl-pipes
```

Navigate into the directory:
```bash
cd devops/wget-curl-pipes

``
Download the file and save it as `session-notes.txt`:

```bash
wget -O session-notes.txt https://raw.githubusercontent.com/daws-90s/notes/refs/heads/main/session-02.txt
```
**Explanation:**

- `wget` – Downloads files from the internet.
- `-O` – Saves the file with the specified name.
- `session-notes.txt` – Name of the downloaded file.

Verify the downloaded file:

```bash
ls -l session-notes.txt
```
Display the file content:
```bash
cat session-notes.txt
```

## Step 2: Fetch the file using curl and count the lines

**Command:**

```bash
curl -s https://raw.githubusercontent.com/daws-90s/notes/refs/heads/main/session-02.txt | wc -l
```

**Explanation:**

- `curl` – Fetches data from a URL.
- `-s` – Runs silently without showing the progress meter.
- `|` – Pipe operator; sends output from one command to another.
- `wc` – Counts lines, words, and characters.
- `-l` – Counts only lines.

**Expected Output:**

The command displays the total number of lines in the file.

**Note:** The file is fetched directly. It is not saved to disk.

## Step 3: Fetch the file and display only lines containing Linux

**Command:**

```bash
curl -s https://raw.githubusercontent.com/daws-90s/notes/refs/heads/main/session-02.txt | grep "Linux"
```

**Explanation:**

- `curl -s` – Fetches the file silently.
- `|` – Passes the output to the next command.
- `grep "Linux"` – Displays only the lines containing the word `Linux`.

**Expected Output:**

Only lines containing `Linux` will be displayed.

## Step 4: Extract the last field using awk

**Command:**

```bash
echo "https://raw.githubusercontent.com/daws-90s/notes/refs/heads/main/session-02.txt" | awk -F "/" '{print $NF}'
```

**Expected Output:**

```text
session-02.txt
```

**Explanation:**

- `echo` – Prints the given text.
- `|` – Sends the output to the next command.
- `awk` – Processes text.
- `-F "/"` – Uses `/` as the field separator.
- `NF` – Represents the total number of fields.
- `$NF` – Represents the last field.
- `print` – Displays the result.

**Answer:**

`$NF` prints the last field from the input. In this example, it displays `session-02.txt`.

## Step 5: Fetch the file, filter ssh lines, and count them using two pipes

**Command:**

```bash
curl -s https://raw.githubusercontent.com/daws-90s/notes/refs/heads/main/session-02.txt | grep "ssh" | wc -l
```

**Explanation:**

- `curl -s` – Fetches the file silently.
- First `|` – Sends the downloaded content to `grep`.
- `grep "ssh"` – Filters lines containing `ssh`.
- Second `|` – Sends the filtered output to `wc`.
- `wc -l` – Counts the matching lines.

**How it works:**

1. `curl` fetches the file from the URL.
2. `grep` filters lines containing `ssh`.
3. `wc -l` counts the matching lines.

**Expected Output:**

The command displays the number of matching lines.

## Linux wget, curl, and Pipes – Quick Reference

| Command | Purpose |
|---|---|
| `wget -O filename URL` | Downloads a file with a specified name |
| `curl -s URL` | Fetches data silently |
| `wc -l` | Counts lines |
| `grep "Linux"` | Displays lines containing Linux |
| `grep "ssh"` | Displays lines containing ssh |
| `awk -F "/"` | Uses `/` as the field separator |
| `$NF` | Extracts the last field |
| `\|` | Passes output to another command |

**Assignment completed successfully.**
