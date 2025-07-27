---
title: "GitHub: How to push verified commits on GitHub"
date: 2025-05-15 16:50:00 +09:00
categories: [Tools,  Git/GitHub]
author: skykhs3
image:
  path: /assets/img/posts/2025-05-15-signing-commits/github-commits.webp
  alt: Signed Git Commits
  show_in_post: true
  background_color: '#f7f8fa'
tags:
  - git
  - github
  - commits
description: Let me explain how to sign commits.
---

> [GitHub Docs: Managing commit signature verification](https://docs.github.com/en/authentication/managing-commit-signature-verification){:target="_blank"}
{: .prompt-info}

| Item | Version |
|-|-|
| OS | MacOS sequoia 15.3.1 |
| Git | 2.49.0 |
| OpenSSH | 9.8p1 |

## 1. Overview

```bash
git config user.name = "your_name"
git config user.email ="your_email"
```

As you know, you can freely change the author of git commits. However, signing git commits prevents author forgery.

## 2. Generating a new ssh key.
```bash
# -t: key type, -C: comment, -f: file location, -N: passphrase
ssh-keygen -t ed25519 -C "Git Signing Key" -f ~/.ssh/id_ed25519_signing -N ""
```

![ssh-keygen](/assets/img/posts/2025-05-15-signing-commits/ssh-keygen.webp)
*ssh-keygen*

>[ssh-keygen official docs](https://man.openbsd.org/ssh-keygen){:target="_blank"}
{: .prompt-info}

## 3. Verify the public key
```bash
cat ~/.ssh/id_ed25519_signing.pub
```
**Copy the public key in the red box.**
![print a public key](/assets/img/posts/2025-05-15-signing-commits/cat-pub.webp)
*print a public key*

## 4. Add the SSH key to GitHub

**[Go to 'SSH and GPG keys' menu.](https://github.com/settings/keys) and click `New SSH key`.**
![print a public key](/assets/img/posts/2025-05-15-signing-commits/github-settings.webp)
*print a public key*

**Select Key type as `Signing Key` and paste your SSH key.**
![print a public key](/assets/img/posts/2025-05-15-signing-commits/github-add-new-ssh-key.webp)
*Add new SSH key.*

## 5. Set up settings on your local computer

```bash
git config --global gpg.format ssh
git config --global user.signingkey ~/.ssh/id_ed25519_signing.pub
git config --global commit.gpgsign true
```

| Command               | Description                            |
| --------------------- | -------------------------------------- |
| `gpg.format ssh`      | Configure to use SSH key instead of GPG for signing |
| `user.signingkey`     | Specify the path to the public key (must be `.pub` file) |
| `commit.gpgsign true` | Automatically sign all commits (no need to use `-S` flag.)<br/>cf. `git commit -S -m "feat: my commit` |

> Of course, you can apply config to one specific repository.
> ```bash
> # Navigate to your repository
> cd your-repository
> 
> # Configure Git to use SSH for signing
> git config --local gpg.format ssh
> git config --local user.signingkey ~/.ssh/id_ed25519_signing.pub
> git config --local commit.gpgsign true
> ```
{: .prompt-tip}

## 6. Done

![Signed Git Commits](/assets/img/posts/2025-05-15-signing-commits/github-commits.webp)
*Signed Git Commits*

Your commits will be signed and verified by the GitHub account where you registered the SSH key.