---
title: "GitHub: How to clone a repository with SSH"
date: 2025-07-27 16:50:00 +09:00
categories: [Tools,  Git/GitHub]
author: skykhs3
image:
  path: /assets/img/posts/github-logo.webp
  alt: Clone a repository with SSH
  show_in_post: true
tags:
  - git
  - github
description: GitHub doesn't recommend using HTTPS to clone repositories. Let me show you how to clone a repository with SSH.
---

> [GitHub Docs: Connecting to GitHub with SSH](https://docs.github.com/en/authentication/connecting-to-github-with-ssh){:target="_blank"}
{: .prompt-info}

| Item | Version |
|-|-|
| OS | MacOS sequoia 15.3.1 |
| Git | 2.49.0 |
| OpenSSH | 9.8p1 |

## 1. Generate SSH Key
```bash
# -t: key type, -C: comment, -f: file location, -N: passphrase
ssh-keygen -t ed25519 -C "your_email@example.com" -f ~/.ssh/id_ed25519_github_authentication -N ""
```

## 2. Add SSH Key to GitHub

### 2.1. Copy Public Key to Clipboard
```bash
cat ~/.ssh/id_ed25519_github_authentication.pub | pbcopy # macOS
```

### 2.2. Open GitHub Settings

1. Go to [GitHub Settings](https://github.com/settings/keys){:target="_blank"}
2. Click `New SSH key`
![Github Settings](/assets/img/posts/2025-07-27-clone-repository/github-settings.webp)
3. Select `Key type` as `Authentication Key` and paste the public key to the `Key` field
![New SSH key](/assets/img/posts/2025-07-27-clone-repository/new-ssh-key.webp)
4. Click `Add SSH key`

## 3. Set SSH Config 

Open `~/.ssh/config` file.

Then, add the following content to the file.
```config 
Host github-personal
  HostName github.com
  User git
  IdentityFile ~/.ssh/id_ed25519_github_authentication
```

> You can set any name for the host. Additionally, you can set multiple github accounts on the same machine, if you add public keys for each github account.
> ```config
> Host github-personal
>   HostName github.com
>   User git
>   IdentityFile ~/.ssh/id_ed25519_github_authentication
>
> Host github-company
>   HostName github.com
>   User git
>   IdentityFile ~/.ssh/id_ed25519_github_authentication
> ```
{: .prompt-tip}

## 4. Test SSH Connection
```bash
ssh -T github-personal
```

## 5. Clone Repository with SSH
```bash
git clone github-personal:<your_username>/<your_repository>.git
```

> Let's examine the default settings of GitHub SSH.
> If you set SSH key as `id_ed25519` which is default and not edited SSH config, you can clone repository with the following command.
> ```bash
> git clone git@github.com:<your_username>/<your_repository>.git
> ```
> And your remote repository name is `origin`, not `github-personal`.
{: .prompt-info}