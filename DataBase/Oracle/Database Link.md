---
tags:
  - Oracle
---

- Oracle에서는 DB Link를 통해 다린 DB에서 데이터를 조회해 가져 올 수 있다.

### DB Link 생성 쿼리

- 비밀번호 입력시에 특수문자고 존재한다면 쌍따옴표를 사용해야 한다.
- USING절 뒤에 문구는 **`tnsnames.ora`** 파일에 작성해서 저장해 두어도 된다.

```sql
CREATE ["PUBLIC or PRIVATE"] DATABASE LINK ["DB링크 이름"]
CONNECT TO ["연결할 계정"] IDENTIFIED BY ["연결할 계정의 비밀번호"]
USING '
	(DESCRIPTION = (ADDRESS=(PROTOCOL = TCP)(HOST = ["HOST IP"])(PORT = 1521))
  (CONNECT_DATA = (SERVER = DEDICATED)(SERVICE_NAME = ["DB NAME(SID)"])))
';
```

### DB Link 삭제 쿼리

```sql
DROP DATABASE LINK ["DB링크 이름"];
```

### DB Link List 조회

```sql
SELECT * FROM ALL_DB_LINKS;
```

### DB Link 사용법

```sql
SELECT * FROM "TABLE_NAME"@"DB_LINK_NAME"
```