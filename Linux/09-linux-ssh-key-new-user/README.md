# Assignment – Key-Based SSH for a New User

## Assignment Questions

1. Create a user `deploy` with no password.
2. Generate a new SSH key pair for this user on your local machine.
3. Set up `.ssh/` and `authorized_keys` on the server for `deploy` with the correct permissions and ownership.
4. Connect to the server as `deploy` using the private key.
5. Try connecting without specifying the key. What happens and why?
6. Disable password authentication on the server. Verify that a password login is rejected. Why does this matter for production?

---

## Solutions

### Step 1: Create a user deploy with no password

Connect to your EC2 instance using your existing SSH connection.

**1. Create the deploy user**

```bash
sudo useradd -m -s /bin/bash deploy
```

**Explanation:**

- `useradd` – Creates a new Linux user.
- `-m` – Creates a home directory.
- `-s /bin/bash` – Sets Bash as the login shell.
- `deploy` – Username.

**2. Check the user details**

```bash
id deploy
```

**3. Check the home directory**

```bash
ls -ld /home/deploy
```

**Expected Output Example:**

```text
drwx------ 2 deploy deploy 62 Nov 1 10:00 /home/deploy
```

The exact permissions may vary.

![Create a Deploy](<create a deploy-1.png>)

### Step 2: Generate a new SSH key pair on your local machine

Open a new **Git Bash terminal on your Windows machine**. Do not run this command on the EC2 server.

**1. Generate an SSH key pair**

```bash
ssh-keygen -t ed25519 -C "deploy@ec2" -f ~/.ssh/deploy_key
```

**Explanation:**

- `ssh-keygen` – Generates an SSH key pair.
- `-t ed25519` – Specifies the Ed25519 key type.
- `-C` – Adds a comment to identify the key.
- `-f` – Specifies the key filename.
- `deploy_key` – Private key filename.
- `deploy_key.pub` – Public key filename.

When prompted for a passphrase, you can enter one for additional protection or press Enter to leave it empty for this practice exercise.

**2. Verify the generated keys**

```bash
ls -l ~/.ssh/deploy_key*
```

**Expected Output:**

```text
deploy_key
deploy_key.pub
```

**Important:**

- `deploy_key` – Private key. Keep it on your local machine. Never share it.
- `deploy_key.pub` – Public key. This is the key you copy to the EC2 server.
  
![Verify Generated Keys](<verify generated keys-2.png>)

### Step 3: Configure .ssh/ and authorized_keys on the server

Return to your existing EC2 terminal.

**1. Create the .ssh directory**

```bash
sudo mkdir -p /home/deploy/.ssh
```

**2. Set directory ownership**

```bash
sudo chown deploy:deploy /home/deploy/.ssh
```

**3. Set directory permissions**

```bash
sudo chmod 700 /home/deploy/.ssh
```

**Explanation:**

- `700` – Only the owner can read, write, and access the directory.
- `deploy:deploy` – Sets the owner and group to deploy.

**4. Create the authorized_keys file**

```bash
sudo touch /home/deploy/.ssh/authorized_keys
```

**5. Set file ownership**

```bash
sudo chown deploy:deploy /home/deploy/.ssh/authorized_keys
```

**6. Set file permissions**

```bash
sudo chmod 600 /home/deploy/.ssh/authorized_keys
```

**7. Copy the public key from your local machine**

On your Windows Git Bash terminal, run:

```bash
cat ~/.ssh/deploy_key.pub
```

Copy the complete output. It should be one long line beginning with `ssh-ed25519`.

Return to the EC2 terminal and open the authorized_keys file:

```bash
sudo nano /home/deploy/.ssh/authorized_keys
```

Paste the copied public key as a single line.

Save and exit Nano:

- Press `Ctrl + O`.
- Press `Enter`.
- Press `Ctrl + X`.

**8. Verify ownership and permissions**

```bash
sudo ls -ld /home/deploy/.ssh
sudo ls -l /home/deploy/.ssh/authorized_keys
```

**Expected Output Example:**

```text
drwx------ 2 deploy deploy 29 Nov 1 10:00 .ssh
-rw------- 1 deploy deploy 99 Nov 1 10:00 authorized_keys
```
![Deploy Ownership Files](<deploy ownership files-3.png>)

### Step 4: Connect to the EC2 server as deploy using the private key

Return to your local Windows Git Bash terminal.

**1. Connect using the private key**

```bash
ssh -i ~/.ssh/deploy_key deploy@YOUR_EC2_PUBLIC_IP
```

Replace `YOUR_EC2_PUBLIC_IP` with your actual EC2 public IP address.

**Explanation:**

- `ssh` – Connects to a remote server.
- `-i` – Specifies the private key.
- `deploy_key` – Private key file.
- `deploy` – Remote Linux username.
- `YOUR_EC2_PUBLIC_IP` – Public IP of your EC2 instance.

**2. Verify the login**

After connecting, run:

```bash
whoami
```

**Expected Output:**

```text
deploy
```

Check the hostname:

```bash
hostname
```

You are now connected to your EC2 instance as the deploy user using SSH key-based authentication.

### Step 5: Try connecting without specifying the key

From your local Windows Git Bash terminal, run:

```bash
ssh deploy@YOUR_EC2_PUBLIC_IP
```

**Expected Result:**

The connection should fail if SSH does not find another accepted key and the deploy account has no usable password.

You may see:

```text
Permission denied (publickey,password).
```

The exact error depends on your SSH server configuration.

![Connecting Key](<connecting key-5.png>)

**Explanation:**

In Step 4, we explicitly provided the private key using `-i`.

In this step, we did not specify the key. Since our custom key is named `deploy_key`, SSH does not automatically select it as a default identity.

Therefore, authentication fails if no other accepted key or usable authentication method is available.

### Step 6: Disable password authentication on the server

**Important:** Keep your existing working SSH session open while making these changes. First confirm that key-based login works in another terminal.

**1. Open the SSH server configuration**

On your EC2 server, run:

```bash
sudo nano /etc/ssh/sshd_config
```

**2. Find or add these settings**

```text
PasswordAuthentication no
KbdInteractiveAuthentication no
PubkeyAuthentication yes
```

Make sure there are no conflicting settings in the SSH configuration files under `/etc/ssh/sshd_config.d/`.

**Explanation:**

- `PasswordAuthentication no` – Disables password-based SSH authentication.
- `KbdInteractiveAuthentication no` – Disables keyboard-interactive authentication.
- `PubkeyAuthentication yes` – Enables public-key authentication.

**3. Validate the SSH configuration**

```bash
sudo sshd -t
```

If there is no output, the configuration syntax is valid.

**4. Restart the SSH service**

```bash
sudo systemctl restart sshd
```

**5. Verify the effective SSH settings**

```bash
sudo sshd -T | grep -E 'passwordauthentication|pubkeyauthentication|kbdinteractiveauthentication'
```

Expected output:

```text
pubkeyauthentication yes
passwordauthentication no
kbdinteractiveauthentication no
```

**6. Test key-based login again**

From your local Windows Git Bash terminal:

```bash
ssh -i ~/.ssh/deploy_key deploy@YOUR_EC2_PUBLIC_IP
```

It should connect successfully.

**7. Test password-based login**

From your local terminal:

```bash
ssh -o PubkeyAuthentication=no -o PreferredAuthentications=password deploy@YOUR_EC2_PUBLIC_IP
```

**Expected Result:**

```text
Permission denied (publickey).
```

The exact message may vary.

![Display Password](<display password-6.png>)

**Answer: Why does this matter in production?**

Password-based SSH can be targeted by brute-force attacks, where attackers repeatedly try different passwords.

Key-based authentication is generally more secure because it uses a cryptographic key pair.

Disabling password authentication reduces the risk of unauthorized access and helps protect production servers.

---

## Quick Reference – Commands Used

| Command | Purpose |
|---|---|
| `sudo useradd -m -s /bin/bash deploy` | Creates deploy user |
| `ssh-keygen -t ed25519` | Generates SSH key pair |
| `cat ~/.ssh/deploy_key.pub` | Displays public key |
| `mkdir -p /home/deploy/.ssh` | Creates SSH directory |
| `chmod 700 .ssh` | Sets secure directory permissions |
| `chmod 600 authorized_keys` | Sets secure file permissions |
| `chown deploy:deploy` | Changes ownership |
| `ssh -i ~/.ssh/deploy_key` | Connects using private key |
| `whoami` | Checks the current username |
| `sudo sshd -t` | Validates SSH configuration |
| `sudo systemctl restart sshd` | Restarts SSH service |
| `sudo sshd -T` | Displays effective SSH configuration |


## Assignment Completed
