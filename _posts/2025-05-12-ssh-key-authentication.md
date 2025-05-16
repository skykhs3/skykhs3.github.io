---
title: "SSH Key Authentication: A Secure Way to Connect to Servers"
date: 2025-05-12 16:50:00 +09:00
categories: [Development,  Security]
author: skykhs3
image:
  path: /assets/img/posts/2025-05-12-ssh-key-authentication/ssh-keygen.webp
  alt: ssh-keygen
  show_in_post: true
  background_color: black
tags:
  - ssh
  - ssh-keygen
description: Let me explain how to connect via SSH using a private key.
---

| Item | Version |
|-|-|
| SSH Client OS | MacOS sequoia 15.3.1 |
| SSH Server OS | Ubuntu 24.04 |
| OpenSSH | 9.8p1 |

## 1. Generate a new user to login (Optional)

#### Server
Generate a new user named `alice`
```bash
sudo adduser alice
```

![Screenshot: Creating a new user](/assets/img/posts/2025-05-12-ssh-key-authentication/adduser.webp)
*Creating a new user with sudo privileges*

> If you want to delete the user and the user's home directory, run the following command:
>```bash
> sudo deluser --remove-home alice
>```
{: .prompt-tip}

> If you want to see users who can connect via SSH, run the following command:
> ```bash
> sudo awk -F: '$3 >= 1000 && $7 != "/usr/sbin/nologin" && $7 != "/bin/false" {print $1}' /etc/passwd
>```
{: .prompt-tip}

## 2. Make a public key and a private key.
#### Client
```bash
# -t: key type (ed25519), -C: comment/description
ssh-keygen -t ed25519 -C "your_command"
```

![Screenshot: Generating SSH keys](/assets/img/posts/2025-05-12-ssh-key-authentication/ssh-keygen.webp)
*Generating a new SSH key pair using ED25519 algorithm*

>[ssh-keygen official docs](https://man.openbsd.org/ssh-keygen){:target="_blank"}
{: .prompt-info}

---

> **Your private key should not be exposed to others!!!**
> 
> Keys may be created under the folder you designated 
> ![I deleted these keys before posting](/assets/img/posts/2025-05-12-ssh-key-authentication/pri-pub.webp)
> (`your_key_name`: private key, `your_key_name.pub`: public key.)
> 
>  These keys are only used for demonstration purposes and have been deleted.
{:.prompt-danger}

> [Public-key cryptography](https://en.wikipedia.org/wiki/Public-key_cryptography){:target="_blank"}
{:.prompt-info}

## 3. Copy the public key to the server
There are two ways to copy. Choose the one you prefer.

### 3-1. Using ssh-copy-id
#### Client
Use the following command to copy your public key to the server in one command:
```bash
ssh-copy-id -i ~/.ssh/your_key_name.pub -p 22 user@your.server.ip 
```

---

### 3-2. Manual Method
#### Client
```bash
cat ~/.ssh/your_key_name.pub | ssh -p 22 user@your.server.ip 'mkdir -p ~/.ssh && chmod 700 ~/.ssh && cat >> ~/.ssh/authorized_keys && chmod 600 ~/.ssh/authorized_keys'
```

## 4. Verify connecting to a server using a private key

#### Client
The system will first attempt to authenticate using your private key before requesting a password.

```bash
ssh -p 22 user@your.server.ip
```

![Screenshot: SSH connection without password](/assets/img/posts/2025-05-12-ssh-key-authentication/ssh-without-password.webp)
*Successfully connected to the server using SSH key authentication*

## 5. Disable password authentication (Optional)

#### Server
To enhance security, you can disable password authentication and allow only key-based authentication. Edit the SSH configuration file:

```bash
sudo nano /etc/ssh/sshd_config
```

Find and modify these lines:
```bash
# Disable password authentication
PasswordAuthentication no
# Disable root login
PermitRootLogin no
# Enable public key authentication
PubkeyAuthentication yes
```

After making changes, restart the SSH service:
```bash
# Restart the service
sudo systemctl restart sshd

# Verify the service status
sudo systemctl status sshd
```

> Make sure you have successfully set up key-based authentication before disabling password authentication. Otherwise, you might get locked out of your server.
{:.prompt-warning}
