---
title: "Types and Options of Git Merge"
date: 2024-09-11 01:29:00 +09:00
categories: [Development, Git]
author: skykhs3
image:
  path: /assets/img/posts/2024-09-11-types-and-options-of-git-merge/no-ff.webp
  alt: 3-way merge
  show_in_post: true
  background_color: white
tags:
  - git
  - git merge
  - merge
---
<div markdown="1">

>When using Git to create multiple branches, you will eventually need to merge them. There are various methods available for merging branches.
>
>Let's begin by assuming familiarity with concepts such as commit, push, pull, and fetch, as well as the ability to create branches.

## 1. The Difference between Merge and Rebase

### 1.1 Merge
`Merge` combines two branches while preserving the flow from both branches.
(specifically, 3 way-merge).

**Pros**
- **Merge History Preservation**: This method clearly shows in the Git history that the tow branches have been merged.
- **Conflict Resolution**: If conflicts occur, a merge commit is created, allowing the conflict resolution to be recorded.

**Cons**

- **Cluttered History**: The history can become cluttered. If there are many branches or frequent merges, the commit log may become messy.

**Example**
<img src="/assets/img/posts/2024-09-11-types-and-options-of-git-merge/base.webp" alt="base"/>
```zsh
git checkout main
```
<img src="/assets/img/posts/2024-09-11-types-and-options-of-git-merge/checkout-merge.webp" alt="checkout"/>
```zsh
git merge feature-branch
```
<img src="/assets/img/posts/2024-09-11-types-and-options-of-git-merge/merge1.webp" alt="merge"/>

---

### 1.2 Rebase
`Rebase` moves the current branch's commits onto the latest commit of the target branch.

**Pros**
- **Clear History**:  It doesn't leave extra Git commit histories, keeping the commit log linear and clean.


**Cons**
- **Creating New Commit**: It generates new commits with new commit hashes, which can make tracking history more difficult.

**Example**
<img src="/assets/img/posts/2024-09-11-types-and-options-of-git-merge/base.webp" alt="rebase"/>
```zsh
git checkout feature-branch
```

<img src="/assets/img/posts/2024-09-11-types-and-options-of-git-merge/checkout-rebase.webp" alt="rebase"/>
```zsh
git rebase main
```

<img src="/assets/img/posts/2024-09-11-types-and-options-of-git-merge/rebase1.webp" alt="rebase"/>

```zsh
git checkout main
git merge feature-branch
```
It should be a fast-forward merge.

<img src="/assets/img/posts/2024-09-11-types-and-options-of-git-merge/rebase2.webp" alt="rebase"/>

---

## 2. Types of Git Merge
If you don't specify a particular merge method, Git will automatically choose the appropriate method.

### 2.1 Fast-forward merge
A fast-forward merge is used when the two branches being merged can simply be added to the end of the current branch. 
- **No Merge Commit**: The branch pointer is simply moved to new one.
- **Occurs in specific situations**: This occurs when the branches have not progressed in parallel.


### 2.2 3-way merge
An 3-way merge is used when the the two branches have different commit. 

- **Merge Commit**: A merge commit is created. It compares the common ancestor commit of the two branches with the latest commit of each branch to generate a new merge commit.

---

## 3. Options of Git Merge


<h3>--ff</h3>
You can merge branches by a fast-forward with the `--ff` option.

**Example**
<img src="/assets/img/posts/2024-09-11-types-and-options-of-git-merge/before.webp" alt="before"/>
```zsh
git checkout main
git merge feature-branch --ff
```
<img src="/assets/img/posts/2024-09-11-types-and-options-of-git-merge/ff.webp" alt="ff"/>

---

<h3>--no-ff</h3>
You can merge branches using a 3-way merge with the `--no-ff` option. This creates a merge commit, preserving the merge history.

**Example**
<img src="/assets/img/posts/2024-09-11-types-and-options-of-git-merge/before.webp" alt="before"/>
```zsh
git checkout main
git merge feature-branch --ff
```
<img src="/assets/img/posts/2024-09-11-types-and-options-of-git-merge/no-ff.webp" alt="no-ff"/>

---

<h3>--squash</h3>
You can squash the commits from a branch into a single commit. This does not preserve the merge history.

**Example**

<img src="/assets/img/posts/2024-09-11-types-and-options-of-git-merge/before.webp" alt="before"/>
```zsh
git checkout main
git merge feature-branch --squash
```
<img src="/assets/img/posts/2024-09-11-types-and-options-of-git-merge/squash.webp" alt="squash"/>

</div>