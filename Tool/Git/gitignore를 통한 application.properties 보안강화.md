---
tags:
  - Git
---
# .gitignore???
깃에 파일을 커밋 시킬 때, 지정한 파일은 커밋 목록에서 제외하고 커밋시키기 위해 사용하는 git 전용 파일이다.

# application.properties??
SpringBoot에서 중요한 설정 정보를 가지고 있는데, 이 정보가 누출되면 보안에 큰 영향을 끼칠 수 있다.

---
# .gitignore와 Resource Bundle을 이용한 보안..?
### 1. application-${random}.properties 파일 만들기
**resources** 폴더에 **application-security.properties** 파일을 생성한다.
마우스 우클릭 후 **resource -> New -> Resource Bundle** 을 클릭한다.

![[스크린샷 2024-01-02 오후 7.57.36.png]]

클릭후에는 이름을 정하는 화면이 나오는데 다음과 같이 입력하고 OK를 눌러준다.

![[스크린샷 2024-01-02 오후 7.59.09.png]]

**application-security.properties**가 생성되었는지 확인
앞으로 중요한 정보가 될만한 것들은 생성한 properties에 작성하면 된다.

![[스크린샷 2024-01-02 오후 8.00.08.png]]

### 2. application.propoerties에 문구추가
**application.properties**에 가서 아래와 같은 문구를 추가해준다.

![[스크린샷 2024-01-02 오후 8.02.16.png]]

### 3. .gitignore에 등록하기
**application-security.properties**가 git에 올라가는걸 막기위해 아래와 같은 문구를 추가해야 한다.
![[스크린샷 2024-01-02 오후 8.04.05.png]]

---

물론 IDEA를 사용하여 원하는 것들만 선택해서 커밋 시킬 수 있지만, 사람일은 모르니 알아두면 좋다!!