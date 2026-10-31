## Assignment — Linux and SSH Setup

**1. Launch an Amazon Linux 2023 EC2 instance in HYD region. Generate an SSH key pair. What is the difference between the two files created?**

### Enable the Hyderabad region 
*Step 1:* Open AWS Console
1. Open AWS Management Console.
2. Log in to your AWS account.
3. On the top-right corner, select the AWS Region.
4. Select Asia Pacific (Hyderabad) – ap-south-2.
5. Search for EC2 in the AWS search bar.
6. Open EC2.
7. Click Launch instance.
![Instance running in ap-south-2](Instance.png)

### Generate the key pair (on my laptop)
*Step-2:*
1. Click Create new key pair.
2. Key pair name: linux-assignment-key
3. Key pair type: RSA.
4. Private key file format: .pem.
5. Click Create key pair
![Linux SSH Key Pair](Linux_SSH_Key%20Pair.png)

*Step-3:* Create a new security group.
Security Group name: linux-ssh-sg, Inbound type: SSH, Protocol: TCP, Port: 22, Source: Anywhere-IPv4 (0.0.0.0/0) – click on create it.
![Linux Security Group](Linux%20Security%20group.png)

### Import the key and launch the instance
*Step-4:* Launch the EC2 instance and choose Name: Linux ,AMI: Amazon Linux 2023 AMI, Instance type: t3.micro, and select keypair: linux-ssh-key, security existing group: select the group(linux-ssh-sg) -> Click on Launch instance.
[Click here to download Linux Assignments.docx](Linux%20Assignments.docx)


### What is the difference?
- **Private key**: Used by you to prove your identity when connecting to the EC2 instance. Never share it.
- **Public key**: Installed on the EC2 instance. SSH uses it to verify that you have the matching private key.


## Nirupa Manne
