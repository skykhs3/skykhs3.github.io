---
title: "백엔드 개발자 학습 자료 & 로드맵 (KOR)"
date: 2024-08-03 00:03:05 +09:00
categories: [Book Report, Development]
tags:
  - backend developer
  - backend
  - developer
---

<script type="text/javascript" async
  src="https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.7/MathJax.js?config=TeX-MML-AM_CHTML">
</script>

<div markdown="1">
백엔드 공부를 어떻게 했고, 어떻게 해야하는 지, 스스로 잘하고 있는 지 점검하기 위한 백엔드 개발자 학습 자료.

(**실용성, 최신성, 난이도**) 이 세 가지 기준으로 평가합니다. 직접 다 읽어보면서 좋은 자료만 계속 업데이트 합니다.

## 1. 알고리즘/코딩테스트
알고리즘을 직접적으로 백엔드 개발에서 사용할 일은 없지만, 왜 많은 회사들이 알고리즘에 관련된 코딩테스트를 본다.

이는 코딩테스트에서는 **코드의 정확성, 빠른 실행 시간, 빠른 코드 작성 시간**을 정량적으로 측정 가능하기 때문이다. 하지만 코딩테스트 만으로는 **코드가 간결하고, 가독성이 높고, 재사용성이 높은 지**는 평가가 불가능한 한계점이 있다. 문제 해결 스타일과 코드의 간결성과 가독성을 파악하기 위해 라이브로 코딩테스트를 진행하기도 한다.

- **[[site] solved.ac](https://solved.ac/problems/level){:target="_blank"} (실용:★★★★★, 최신:★★★★★, 난이도:)**<br/>본인의 수준을 판단하기 좋으며, 낮은 수준부터 높은 수준까지 차례대로 풀면 필수적인 알고리즘과 자료 구조를 학습할 수 있다.


- **[[site] 프로그래머스](https://school.programmers.co.kr/learn/challenges?order=recent&page=1){:target="_blank"} (실용:★★★★☆, 최신:★★★★☆, 난이도:★★★☆☆)**<br/>실제 카카오 코딩테스트 기출들이 있어 좋다.

- **[[book] 프로그래밍 대회에서 배우는 알고리즘 문제해결 전략 1,2](https://www.aladin.co.kr/shop/wproduct.aspx?ItemId=21089176){:target="_blank"} (실용:★★★☆☆, 최신:★★★☆☆, 난이도:★★★★★)**<br/>프로그래밍 대회의 바이블, 이 책을 이해하는 수준이라면 코딩테스트는 걱정 안 해도 되는 레벨이다. 프로그래밍 대회에서는 필요하지만, 코딩 테스트에는 수준이 높은 내용이 많다.

- **[[book] 프로그래밍 콘테스트 챌린징](https://www.aladin.co.kr/shop/wproduct.aspx?ItemId=13459601){:target="_blank"} (실용:★☆☆☆☆, 최신:★★☆☆☆, 난이도:★★★★★)**<br/> ACM ICPC, IOI/KOI 대비용

- **[[book] 알고리즘 트레이닝: 자료구조, 알고리즘 문제 해결 핵심 노하우](https://www.aladin.co.kr/shop/wproduct.aspx?ItemId=110847940){:target="_blank"} (실용:★☆☆☆☆, 최신:★★☆☆☆, 난이도:★★★★★)**<br/>ACM ICPC, IOI/KOI 대비용
  
## 2. CS Basic

- **[[book] 혼자 공부하는 컴퓨터 구조 + 운영체제](https://www.aladin.co.kr/shop/wproduct.aspx?ItemId=299014282){:target="_blank"} (실용:★★★☆☆, 최신:★★★☆☆, 난이도:★★★☆☆)**
- **[[book] 혼자 공부하는 네트워크](https://www.aladin.co.kr/shop/wproduct.aspx?ItemId=337483817&start=slayer){:target="_blank"} (실용:★★★☆☆, 최신:★★★☆☆, 난이도:★★★☆☆)**<br/>비전공자에게 추천하는 책. 학과 내용을 너무 깊지 않게 개발에 필요한 지식들만 딱 한 권에 끝낼 수 있다.
  

## 3. JavaScript 기반 프레임 워크
JavaScript/TypeScript/Express/Nestjs
컴퓨터 언어와 프레임 워크는 끊임 없이 개선이 되기에, 새로운 업데이트를 팔로업하면 좋다.

## 4. MySql

## 5. Git

- **[[site] LearnGitBranching](https://learngitbranching.js.org/?locale=ko){:target="_blank"} (실용:★★★☆☆, 최신:★★★☆☆, 난이도:★★☆☆☆)**<br/>무작정 실습하면서 Git을 공부하기 좋다.

- **[[site] 생활코딩 - 지옥에서 온 Git](https://opentutorials.org/course/2708){:target="_blank"} (실용:★★★☆☆, 최신:★★★★☆, 난이도:★★☆☆☆)**<br/>여기 내용만 숙지해도 버젼 관리와 협업에 문제 없다.

## 6. Docker

## 7. AWS
클라우드 서버의 첫걸음
- **[[site] AWS Skill Builder](https://explore.skillbuilder.aws/learn) (실용:★★★★☆,최신:★★★★★, 난이도:)**<br/>AWS에서 제공하는 무료 강의들

- **[[blog] 후기 서비스 AWS Opensearch 도입기](https://helloworld.kurly.com/blog/2023-review-opensearch/)**

## 8. CI/CD
- **[[site] GitHub Actions Quick Start](https://docs.github.com/ko/actions/writing-workflows/quickstart) (실용:★★★★☆,최신:★★★★★, 난이도:)**
  
## 9. IaC (Infrastructure as Code)
- **[[site] Terraform Tutorials](https://developer.hashicorp.com/terraform/tutorials)**<br>

## 10. 리팩토링

단순히 빠르고 정확한 코드를 넘어, 좋은 코드에 관한 책들

- **[[book] 클린 코드 Clean Code](https://www.aladin.co.kr/shop/wproduct.aspx?ItemId=34083680){:target="_blank"}**

- **[[site] 클린코드 : 자바스크립트 (한국어 번역)](https://github.com/sbyeol3/clean-code-javascript-kr){:target="_blank"}**

- **[[book] 리팩토링 2판 (리팩토링 개정판)](https://www.aladin.co.kr/shop/wproduct.aspx?ItemId=236186172){:target="_blank"}**

## 11. 개발 문화
- **[[blog] 효과적인 코드리뷰를 위한 리뷰어의 자세](https://tech.kakao.com/posts/498){:target="_blank"}** : 리뷰어의 기본적인 마인드셋, 원할한 협업 또한 중요한 능력이다.

- **[[blog] 코드 리뷰를 어떻게 하는 게 좋을까?](https://smartstudio.tech/how-to-make-a-good-code-review/){:target="_blank"}** 
  
- **[[blog] 코드 리뷰 in 뱅크샐러드 개발 문화](https://blog.banksalad.com/tech/banksalad-code-review-culture/){:target="_blank"}** : Pn 룰 아이디어가 좋음


## 12. 소프트 스킬
코드 몽키가 아닌 개발자로서의 발전에 도움을 주는 책

- **[[book] 소프트 스킬](https://www.aladin.co.kr/shop/wproduct.aspx?ItemId=298621616){:target="_blank"}** - 전문가는 의사소통, 지식 전문성, 리더십을 갖춰야 한다.

<br/>
**좋은 자료가 있다면 추천 부탁드립니다.**

</div>
