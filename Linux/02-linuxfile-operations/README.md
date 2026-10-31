## Assignment — Linux File Operations

1.	Create the following directory structure using a single command:   
devops/
└── projects/
    └── batch-90s/
3.	Inside batch-90s/, create a file called notes.txt. Write three lines into it, then append two more. Read it to verify.
4.	Copy notes.txt to projects/ and rename the copy to backup.txt. Verify both exist.
5.	Move backup.txt to /tmp/. What happens to files there when the server restarts?
6.	Delete the devops/ directory and everything inside it.
7.	List the contents of /var/log/ sorted by time. What do you notice? Why would a DevOps engineer check this directory?

****1.	Create the following directory structure using a single command:**
devops/
└── projects/
    └── batch-90s/

**Step 1:** Create the directory structure using a single command
```bash
mkdir -p devops/projects/batch-90s
```
**Explanation:**
- `mkdir` – Creates a directory.
- `-p` – Creates parent directories automatically.
- `devops/projects/batch-90s` – Creates the complete directory structure.
