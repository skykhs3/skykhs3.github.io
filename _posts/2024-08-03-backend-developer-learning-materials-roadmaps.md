---
title: "백엔드 개발자 학습 자료 & 로드맵 (KOR)"
date: 2024-08-03 00:03:05 +09:00
categories: [Development, Development]
tags:
  - backend developer
  - backend
  - developer
---

<script type="text/javascript" async
  src="https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.7/MathJax.js?config=TeX-MML-AM_CHTML">
</script>

<div markdown="1">

>백엔드 개발을 혼자 공부하면서, 내가 올바른 방향으로 학습하고 있는지, 판단하기가 어려웠다. 이 글은 내가 백엔드 개발자로 성장하기 위해 사용한 학습 자료와 로드맵을 정리했다.
>
>지금도 부족한 점이 많지만 지금까지 공부한 것들을 기록해보려고 한다. 백엔드를 독학 하는 사람들에게 도움이 되었으면 좋겠다. 

(**난이도, 실용성, 최신성**) 이 세 가지 기준으로 주관적으로 평가하며, 계속 업데이트 중.

**난이도**: 학습에 있어 수준이 어려운가?<br/>
**실용성**: 백엔드 개발자로서 현업에 되는 자료인가?<br/>
**최신성**: 옛날 자료가 아니고, 최신 업데이트가 문서에 반영이 있는가?<br/>
<br/>

---

## 1. 알고리즘/코딩테스트
알고리즘을 직접적으로 백엔드 개발에서 사용할 일은 없지만, 왜 많은 회사들이 알고리즘에 관련된 코딩테스트를 본다.

이는 코딩테스트에서는 **코드의 정확성, 빠른 실행 시간, 빠른 코드 작성 시간**을 정량적으로 측정 가능하기 때문이다. 하지만 코딩테스트 만으로는 **코드가 간결하고, 가독성이 높고, 재사용성이 높은 지**는 평가가 불가능한 한계점이 있다. 문제 해결 스타일과 코드의 간결성과 가독성을 파악하기 위해 라이브로 코딩테스트를 진행하기도 한다.

### Upper Immediate
- **[[site] solved.ac](https://solved.ac/problems/level){:target="_blank"}<br/>(난이도:★★★☆☆, 실용:★★★☆☆, 최신:★★★★☆)**<br/>본인의 수준을 판단하기 좋으며, 낮은 수준부터 높은 수준까지 차례대로 풀면 필수적인 알고리즘과 자료 구조를 학습할 수 있다. 코딩테스트를 대비한다면 골드 레벨 문제를 원할히 풀 수 있도록 노력해야한다.

- **[[site] 프로그래머스](https://school.programmers.co.kr/learn/challenges?order=recent&page=1){:target="_blank"}<br/>(난이도:★★★☆☆, 실용:★★★☆☆, 최신:★★★★☆)**<br/>공개된 카카오 코딩테스트 기출들 풀어보는 것 추천

### Advanced
- **[[book] 프로그래밍 대회에서 배우는 알고리즘 문제해결 전략 1,2](https://www.aladin.co.kr/shop/wproduct.aspx?ItemId=21089176){:target="_blank"}<br/>(난이도:★★★★☆, 실용:★★☆☆☆, 최신:★★☆☆☆)**<br/>프로그래밍 대회의 바이블, 이 책을 이해하는 수준이라면 코딩테스트는 걱정 안 해도 되는 레벨이다. 프로그래밍 대회를 대비하기 위해서는 필요하지만, 코딩 테스트를 대비 하기에는 수준이 높은 내용이 많다.

- **[[book] 프로그래밍 콘테스트 챌린징](https://www.aladin.co.kr/shop/wproduct.aspx?ItemId=13459601){:target="_blank"}<br/>(난이도:★★★★☆, 실용:★☆☆☆☆, 최신:★★☆☆☆)**<br/> ACM ICPC, IOI/KOI 대비용

- **[[book] 알고리즘 트레이닝: 자료구조, 알고리즘 문제 해결 핵심 노하우](https://www.aladin.co.kr/shop/wproduct.aspx?ItemId=110847940){:target="_blank"}<br/>(난이도:★★★★☆, 실용:★☆☆☆☆, 최신:★★☆☆☆)**<br/>ACM ICPC, IOI/KOI 대비용

<br/>

---

## 2. CS
### Basic
- **[[book] 혼자 공부하는 컴퓨터 구조 + 운영체제](https://www.aladin.co.kr/shop/wproduct.aspx?ItemId=299014282){:target="_blank"}<br/>(난이도:★★☆☆☆, 실용:★★★★☆, 최신:★★★☆☆)**
- **[[book] 혼자 공부하는 네트워크](https://www.aladin.co.kr/shop/wproduct.aspx?ItemId=337483817&start=slayer){:target="_blank"}<br/>(난이도:★★☆☆☆, 실용:★★★★☆, 최신:★★★☆☆)**<br/>비전공자에게 추천하는 책. 학과 내용을 너무 깊지 않게 개발에 필요한 지식들만 딱 한 권에 끝낼 수 있다.
  
<br/>

---

## 3. JavaScript/TypeScript
### Basic (Language)
- **[[site] Javascript Tutorial](https://www.javascripttutorial.net/){:target="_blank"}<br/>(난이도:★★☆☆☆, 실용:★★★★☆, 최신:★★★☆☆)** 
- **[[site] 모던 JavaScript 튜토리얼](https://ko.javascript.info/){:target="_blank"}<br/>(난이도:★★☆☆☆, 실용:★★★★☆, 최신:★★★☆☆)**<br/> 내용이 중복되기 때문에 위, 아래 사이트 둘 중 하나만 읽어도 충분하다.
  
- **[[site] Typescript Tutorial](https://www.typescripttutorial.net/){:target="_blank"}<br/>(난이도:★★☆☆☆, 실용:★★★★☆, 최신:★★★☆☆)** 

### Basic (Framework)

- **[[site] Express Starter](https://expressjs.com/en/starter/installing.html){:target="_blank"}<br/>(난이도:★★☆☆☆, 실용:★★★★☆, 최신:★★★★★)**<br/>한글 문서도 있으나 번역이 느려 영어 문서보다 업데이트가 느리므로 영어 문서로 직접 읽는 것이 좋다.

- **[[blog] How to set up TypeScript with Node.js and Express](https://blog.logrocket.com/how-to-set-up-node-typescript-express/){:target="_blank"}<br/>(난이도:★☆☆☆☆, 실용:★★★★☆, 최신:★★★☆☆)** 
  
- **[[site] NestJS Documentation](https://docs.nestjs.com/){:target="_blank"}<br/>(난이도:★★☆☆☆, 실용:★★★★☆, 최신:★★★★★)**<br/>공식 문서
  
<br/>

---

## 4. MySql
### Basic
- **[[site] MySQL Tutorial](https://www.mysqltutorial.org/){:target="_blank"}<br/>(난이도:★★★☆☆,실용:★★★★☆, 최신:★★★★☆)**

<br/>

---

## 5. Redis
### Basic
- **[[우아한테크세미나] 191121 우아한레디스 by 강대명님](https://youtu.be/mPB2CZiAkKM?feature=shared){:target="_blank"}<br/>(난이도:★★☆☆☆, 실용:★★★★☆, 최신:★★★☆☆, )**

<br/>

---
## 6. Git
### Basic
- **[[site] LearnGitBranching](https://learngitbranching.js.org/?locale=ko){:target="_blank"}<br/>(난이도:★★☆☆☆, 실용:★★★☆☆, 최신:★★★★☆)**<br/>무작정 실습하면서 Git을 공부하기 좋다.

### Intermediate
- **[[site] 생활코딩 - 지옥에서 온 Git](https://opentutorials.org/course/2708){:target="_blank"}<br/>(난이도:★★★☆☆, 실용:★★★☆☆, 최신:★★★★☆)**<br/>여기 내용만 숙지해도 사용에 문제가 없다.

<br/>

---
## 7. Docker/Kubernetes

<br/>

---
## 8. AWS
### Basic
- **[[site] AWS Cloud Practitioner Essentials](https://explore.skillbuilder.aws/learn/course/external/view/elearning/1928/aws-cloud-practitioner-essentials-korean-sub-hangug-eo-jamag){:target="_blank"}<br/>(난이도:★★☆☆☆, 실용:★★☆☆☆, 최신:★★★★★)**<br/>AWS에서 제공하는 입문 강의. 애니메이션으로 비유적으로 표현해 이해하기 쉬우며 한글 자막이 있다.

- **[[site] AWS Technical Essentials](https://explore.skillbuilder.aws/learn/course/internal/view/elearning/15366/aws-technical-essentials-korean-na-hangug-eo-gang-ui){:target="_blank"}<br/>(난이도:★★☆☆☆, 실용:★★☆☆☆, 최신:★★★★★)** <br/> 한국어 강의. 위 강의와 내용이 대부분 겹친다. 

<br/>

---
## 9. CI/CD
### Basic
- **[[site] GitHub Actions Quick Start](https://docs.github.com/ko/actions/writing-workflows/quickstart){:target="_blank"}<br/>(난이도:★★★☆☆, 실용:★★★★☆, 최신:★★★★★)**<br/>공식 문서

<br/>

---
## 10. IaC (Infrastructure as Code)

### Basic

- **[[site] Terraform Tutorials](https://developer.hashicorp.com/terraform/tutorials){:target="_blank"}<br/>(난이도:★★★☆☆, 실용:★★★☆☆, 최신:★★★★★)**<br>공식 문서

<br/>

---
## 11. 리팩토링



### Intermediate

- **[[book] 클린 코드 Clean Code](https://www.aladin.co.kr/shop/wproduct.aspx?ItemId=34083680){:target="_blank"}**
- **[[site] 클린코드 : 자바스크립트 (한국어 번역)](https://github.com/sbyeol3/clean-code-javascript-kr){:target="_blank"}**
- **[[book] 리팩토링 2판 (리팩토링 개정판)](https://www.aladin.co.kr/shop/wproduct.aspx?ItemId=236186172){:target="_blank"}**<br>
단순히 빠르고 정확한 코드를 넘어, 좋은 코드에 관한 책들. 좋은 코드가 무엇인지 사람마다 기준이 다르지만, 오랜 시간 경험이 쌓인 시니어 개발자들이 제시하는 기준들을 알 수 있다. 이것이 왜 좋은 코드인지, 그리고 코드를 왜 이렇게 짜야 하는지 근거가 생긴다.

<br/>

---
## 12. 개발 문화
- **[[blog] 효과적인 코드리뷰를 위한 리뷰어의 자세](https://tech.kakao.com/posts/498){:target="_blank"}**
- **[[blog] 코드 리뷰를 어떻게 하는 게 좋을까?](https://smartstudio.tech/how-to-make-a-good-code-review/){:target="_blank"}** 
- **[[blog] 코드 리뷰 in 뱅크샐러드 개발 문화](https://blog.banksalad.com/tech/banksalad-code-review-culture/){:target="_blank"}**
  <br/>프로덕션은 혼자 만들지 못하고, 개발자들의 협업이 필요하다. 코드 리뷰어의 기본적인 마인드셋

<br/>

---
## 13. 소프트 스킬
코드 몽키가 아닌 개발자로서의 발전에 도움을 주는 책

- **[[book] 소프트 스킬](https://www.aladin.co.kr/shop/wproduct.aspx?ItemId=298621616){:target="_blank"}**<br/>(난이도:★☆☆☆☆, 실용:★★★★★, 최신:*)<br/>전문가가 되기 위한 조건 3가지(의사소통, 지식 전문성, 리더십)

<br/>

---
<br/>
**좋은 자료가 있다면 추천 부탁드립니다.**

</div>
