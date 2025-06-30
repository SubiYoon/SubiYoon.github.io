---
tags:
  - DataBase
  - PostgreSQL
---
## Postgres 진입하기
* SQL을 작성할 수 있는 화면으로 진입을 해야한다.
* 필자의 경우 docker를 통해 생성하였기 때문에 다음과 같은 명령어를 사용한다.
```bash
docker exec ${컨테이너명} -it /bin/bash

# 위 명령어로 도커 컨테이너에 진입 후 postgreSQL에 진입한다.
psql -U ${유저}
```

## pg_hba.conf 파일 위치 확인
* 비밀번호를 입력하기 위한 설정은 해당 파일에 존재한다.
```bash
show data_directory;

# /var/lib/postgresql/data
```

## pg_hba.conf 파일을 수정
* vim, cat 등을 이용하여 해당 내용을 수정하자.
```bash
vi /var/lib/postgresql/data/pg_hba.conf
```

* 아래 표시한 부분을 수정한다. `tust -> md5`
![image][https://velog.velcdn.com/images/soohun9909/post/ffa174d4-1bdc-4769-8d51-0fada1c860cb/image.png]

## PostgreSQL 재시작
* 변경한 내용을 적용 시키기 위해 해당 서비스를 재시작해준다.
* 필자의 경우 docker를 재시작하였고, 컨테이너의 이름은 `postgres`이다. 각자 환경에 맞게 서비스를 재시작 해주면 된다.
```bash
docker restart postgres
```

---
## 기타 PostgreSQL 팁...?!
### 스키마 없이 테이블명으로 조회하기
```sql
ALTER USER ${유저} SET search_path = ${스키마},public;
```