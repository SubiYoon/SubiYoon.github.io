---
tags:
  - DataBase
  - Oracle
---
### 개요
회사에서 개발하던 중 스키마하나의 비밀번호가 만료되는 상황이 발생하였습니다.
오늘은 이 문제를 해결해 보고자 합니다.

비밀번호가 만료되면 다음과 같은 에러문구가 발생합니다.
```java
ORA-28001: The password has expired
```

현재 제가 파악한 방법은 2가지가 있습니다.

### 새로운 비밀번호를 부여해 수정하는 방법
```sql
ALTER USER ${user_id} IDENTIFIED BY ${new_password};
```

### 다음 로그인시 비밀번호를 변경하도록 하는 방법
```sql
ALTER USER ${user_id} PASSWORD expire;
```

### 마무리
현재 사용하는 개발서버에서는 차후 비밀번호가 만료되는 것을 방지하기 위해 expire하는 기능을 끄도록 했습니다. 웬만하면 건딜지 않는 것을 추천합니다.
```sql
 ALTER PROFILE DEFAULT LIMIT PASSWORD_LIFE_TIME UNLIMITED;
```