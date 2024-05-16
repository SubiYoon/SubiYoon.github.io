---
tags:
  - SpringBoot
  - SpringSecurity
  - Login
---
# 공부 목적
개인포트폴리오 사이트를 구축하던 중 회사 동료와 Front와 Back을 나눠 각자 구현하여 같이 프로젝트를 하던 중 개개인의 데이터를 변경하기 위해 관리자 사이트를 만들어 수정하기로 결정했다.
해당 과정에서 SpringBoot 3에서와 SpringSecurity 6에서의 로그인 구현이 이전 버전과 다르기에 사용해보고자 했다.

cf. Front를 담당했던 동료가 너무 천천히 개발을 하여 관리자 부분은 내가 VueJs를 사용하여 개발하려고 한다. 사용자 홈페이지의 경우는 React로 개발중이다.

### 개발환경
* Java 17
* SpringBoot 3.2.1
* SpringSecurity 6.2.1
* DB PostgreSQL