---
title: "Retrospective: Bagelcode"
date: 2024-09-04 18:00:00 +09:00
categories: [Retrospectives, Professional Reflections]
author: skykhs3
image:
  path: /assets/img/posts/2024-09-04-bagelcode-retrospective/logo.webp?v=1000
  alt: "Bagelcode: Mobile Game Company"
  show_in_post: true
tags:
  - bagelcode
  - retrospective
description: I worked at Bagelcode as a backend developer and contributed to increasing the company's revenue. Let me introduce my experience at Bagelcode.
---

<div markdown="1">

## 1. Company Introduction

**[Bagelcode](https://www.bagelcode.com/en/){:target="_blank"}** is a rapidly growing mobile game company with a global market presence.

One of Bagelcode's flagship games, **Club Vegas Slots Casino Games**, is enjoyed by over 50 million people worldwide. Notably, Club Vegas boasts the highest average playtime among social casino games and ranked in the top 100 for overall revenue on Android in the U.S. in 2022.

**Club Vegas Slots Casino Games**
- [Google Play](https://play.google.com/store/apps/details?id=com.bagelcode.slots1){:target="_blank"}
- [App Store](https://apps.apple.com/us/app/club-vegas-slots-casino-games/id1201054588){:target="_blank"}

*Note: The Casino game is not playable in Republic of Korea.*

I worked as a backend developer for the **Club Vegas Studio server team**<br/>
**Role**: Backend Developer, Club Vegas Server Team<br/>
**Work Period**: February 26th, 2024 - August 25th, 2024<br/>

## 2. Key Tasks
As a member of the Club Vegas Studio server team, my tasks included:
- Developing the game backend API server using TypeScript (**Express.js**)
- Developing an in-house admin website using **React**
- Developing a game item purchase and payment website using **Next.js** and **NestJS**
- Building a backend server to support third-party payments by integrating with third-party payment providers' webhooks
- Operating the system with **AWS and Kubernetes (EKS)**
- Implementing CI/CD pipelines using **GitHub Actions**

## 3. Key Projects
I independently undertook the development work for the following projects:

### 3.1. Implementing Challenge Missions
I implemented the logic for rewarding items to users who achieve certain milestones or frequencies on [third-party payment sites](https://store.clubvegasslots.com/){:target="_blank"} to facilitate third-party payments. I also implemented a system to send analytic data to the Data Analysis team to measure the revenue impact of these challenge missions. Additionally, I improved an in-house admin website that can manage milestones or frequencies tailored to each player. **The main goal was to make users pay more often, which would help increase the company's revenue.**

### 3.2. Website Login Page Renewal
I developed a [payment website login page](https://playclubvegas.com/){:target="_blank"} that allows users to log in by clicking a button, which automatically launches the game app and logs them in. **This task improved the login experience, allowing users to log in more easily.**

### 3.3. Refactoring Event Rewards Logic
I refactored the code related to event rewards and developed both the frontend and backend code for the in-house admin website. **This enhancement enabled game administrators to dynamically and easily configure event reward values.**

### 3.4. Building a Backend Server to Support Third-Party Payments and New Game Items
I developed the logic for selling and configuring new game items by integrating with [third-party payment webhooks and APIs](https://developers.appcharge.com/reference/getting-started-with-appcharge-api){:target="_blank"}. This integration enabled game administrators to dynamically configure and manage new game items for sale through the in-house admin website. **The main goal was to increase net revenue by avoiding the high fees** that App Store and Play Store charge.

### 3.5. DevOps
I participated in AWS workshops and the AWS Summit, where I learned about the latest trends in cloud computing. While managing Club Vegas Studio's infrastructure, **I also gained experience using Terraform and Kubernetes(EKS).**

</div>


> I had a great experience working at a wonderful company with **skillful and kind team members**. I was grateful for the opportunity to work there for six months.
{: .prompt-info}