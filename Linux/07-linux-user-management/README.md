# Assignment – Linux User Management

## Assignment Questions

1. Create a user `alice`. What primary group was automatically created for her?
2. Set a password for `alice`. Configure the server to allow password-based SSH and connect as `alice`.
3. Create a group `devops-team`. Add `alice` as a secondary member. Verify.
4. Create a user `bob`. Add both `alice` and `bob` to `devops-team`.
5. Give `alice` sudo access. Verify she can run commands as root.
6. Remove `alice`'s sudo access. Confirm she can no longer run sudo commands.
7. Add a rule to `/etc/sudoers` that allows only `devops-team` to run `useradd` and `usermod` — nothing else.

---

## Solutions

### Step 1: Create a user alice and identify her primary group

**Command:**

```bash
sudo useradd -m alice
```

**Explanation:**

- `sudo` – Runs the command with administrator privileges.
- `useradd` – Creates a new user.
- `-m` – Creates a home directory for the user.
- `alice` – Username.

Check the user details:

```bash
id alice
```

Check the primary group:

```bash
groups alice
```

Check the `/etc/passwd` entry:

```bash
grep "^alice:" /etc/passwd
```

**Expected Output Example:**

```text
uid=1001(alice) gid=1001(alice) groups=1001(alice)
```
![Create User](<create user.png>)

![Alice 2](<alice 2.png>)

**Answer:**

On a typical Amazon Linux setup, a private primary group named `alice` is automatically created for the user. The actual group ID depends on your system.

### Step 2: Set a password for alice and enable password-based SSH

**1. Set a password for alice**

```bash
sudo passwd alice
```

Enter the new password when prompted and confirm it.

**2. Configure SSH password authentication**

Open the SSH configuration file:

```bash
sudo vi /etc/ssh/sshd_config
```

Find or add the following settings:

```text
PasswordAuthentication yes
UsePAM yes
```
![Alice 2](<alice 2.png>)

Make sure there is no conflicting setting in the SSH configuration files under `/etc/ssh/sshd_config.d/`.

**3. Validate the SSH configuration**

```bash
sudo sshd -t
```

If there is no output, the configuration syntax is valid.
![Alice 2](<alice 2.png>)

**4. Restart the SSH service**

```bash
sudo systemctl restart sshd
```

Check the service:

```bash
sudo systemctl status sshd
```
![Bob DevOps Team](<bob devops team-4.png>)

**5. Connect as alice**

Open another terminal on your computer and run:

```bash
ssh alice@YOUR_EC2_PUBLIC_IP
```

Enter alice's password when prompted.
![Create User](<create user.png>)

**Important:**
- Keep your existing EC2 SSH session open while testing.
- Do not disable your existing key-based SSH access.
- Ensure the EC2 security group allows inbound SSH on port 22.
- Password-based SSH is less secure than key-based authentication. Use it only for this practice assignment and disable it afterward.

### Step 3: Create devops-team and add alice as a secondary member

**1. Create the group**

```bash
sudo groupadd devops-team
```

**2. Add alice to the group**

```bash
sudo usermod -aG devops-team alice
```

**Explanation:**

- `usermod` – Modifies an existing user.
- `-a` – Appends the new group without removing existing groups.
- `-G` – Specifies secondary groups.
- `devops-team` – Group name.
- `alice` – Username.

**3. Verify group membership**

```bash
id alice
```

Or:

```bash
groups alice
```

**Expected Output Example:**

```text
alice : alice devops-team
```

### Step 4: Create bob and add both users to devops-team

**1. Create bob**

```bash
sudo useradd -m bob
```

**2. Add bob to devops-team**

```bash
sudo usermod -aG devops-team bob
```

**3. Verify alice's group membership**

```bash
id alice
```

**4. Verify bob's group membership**

```bash
id bob
```

**Expected Output Example:**

```text
alice : alice devops-team
bob : bob devops-team
```

Both users should have `devops-team` as a secondary group.

### Step 5: Give alice sudo access and verify

**1. Add alice to the sudo group**

On Amazon Linux, the administrator group is usually `wheel`:

```bash
sudo usermod -aG wheel alice
```

**2. Verify membership**

```bash
groups alice
```

Expected output should include:

```text
alice devops-team wheel
```

**3. Switch to alice**

```bash
su - alice
```

**4. Verify sudo access**

```bash
sudo whoami
```

**Expected Output:**

```text
root
```

**Explanation:**

- `su - alice` – Switches to alice's account.
- `sudo whoami` – Runs the `whoami` command with administrator privileges.
- `root` – Confirms the command ran as the root user.
![Alice Wheel Group](<alice wheel group-5.png>)

### Step 6: Remove alice's sudo access and verify

First, switch back to your administrator account or EC2 user.

**1. Remove alice from the wheel group**

```bash
sudo gpasswd -d alice wheel
```

**2. Verify group membership**

```bash
groups alice
```

The output should no longer contain `wheel`.

**3. Switch to alice again**

```bash
su - alice
```

**4. Test sudo access**

```bash
sudo whoami
```

**Expected Result:**

Alice should receive a message saying she is not allowed to run sudo commands.

**Explanation:**

- `gpasswd -d` – Removes a user from a group.
- `wheel` – Administrator group on Amazon Linux.
- Removing alice from `wheel` removes the sudo access granted through that group.

**Note:** This assumes alice has no other sudo permissions through another group or sudoers rule.
![Remove Alice Sudo](<remove alice sudo-6.png>)

### Step 7: Allow only devops-team to run useradd and usermod

**1. Find the command paths**

```bash
which useradd
which usermod
```

Example output:

```text
/usr/sbin/useradd
/usr/sbin/usermod
```

Use the actual paths returned by your system.

**2. Open the sudoers file safely**

```bash
sudo visudo
```

Add the following rule at the bottom of the file, using the paths found above:

```text
%devops-team ALL=(root) /usr/sbin/useradd, /usr/sbin/usermod
```

**Explanation:**

- `%` – Indicates a group in sudoers.
- `devops-team` – Group allowed to execute the commands.
- `ALL` – Applies on all hosts.
- `(root)` – Allows running the commands as root.
- `/usr/sbin/useradd` – Allows the useradd command.
- `/usr/sbin/usermod` – Allows the usermod command.

**3. Save and exit**

In `visudo`, save and exit using the editor commands. If it opens in Vim, type:

```vim
:wq
```

**4. Verify the sudoers configuration**

```bash
sudo visudo -c
```

**Expected Output:**

```text
/etc/sudoers: parsed OK
```
![Sudoers Configuration](<sudoers configuration.png>)

**Important security note:**

This rule allows members of `devops-team` to execute these two commands through sudo, but it does not restrict their arguments. Unrestricted `useradd` and `usermod` options can potentially be abused to gain additional privileges. For a production server, use tightly restricted command arguments or a controlled script instead.

Also, this rule does not grant permission to run other sudo commands.

---

## Quick Reference – Commands Used

| Command | Purpose |
|---|---|
| `sudo useradd -m alice` | Creates alice and her home directory |
| `sudo passwd alice` | Sets alice's password |
| `id alice` | Displays user and group information |
| `sudo groupadd devops-team` | Creates a group |
| `sudo usermod -aG devops-team alice` | Adds alice to a secondary group |
| `sudo useradd -m bob` | Creates bob |
| `sudo usermod -aG wheel alice` | Grants sudo access through wheel |
| `sudo gpasswd -d alice wheel` | Removes alice from wheel |
| `sudo visudo` | Safely edits sudoers |
| `sudo visudo -c` | Checks sudoers syntax |
| `sudo whoami` | Verifies sudo access |

## Assignment Completion

Questions 1–7 are covered with commands and explanations. Question 8 will be added once its full text is available.
