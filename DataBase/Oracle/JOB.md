---
tags:
  - Oracle
---
# JOB

## JOB 이란??
- Java의 스케쥴과 비슷한 기능
- 일정 시간을 설정하여 주기적으로 해당 문구를 실행시켜주는 역할을 한다.

## JOB 생성
```sql
DECLARE	
	X NUMBER;
BEGIN
SYS.DBMS_JOB_SUBMIT (
	JOB => X,
	WHAT => '${프로시저} or ${반복할 구문}', -- 반복하고 하는 것
	NEX_DATE => SYSDATE + 1/24/60, -- 1분후
	INTERVAL => 'SYSDATE +${주기}', --6초 간격
	NO_PARSE => TRUE
);
END;
```

## 등록된 JOB 확인
```sql
SELECT * FROM USER_JOB;
```

## JOB 비활성화
```sql
BEGIN
	DBMS_JOB.BROKEN(${JOB NUMBER}, false); -- 비활성화
	COMMIT;
END;
```

## JOB 수정
```sql
--수정
BEGIN
	DBMS_JOB.CHANGE(${JOB_NUMBER}, null, ${변경일자}, '${주기}');
	COMMIT;
END;

--INERTVAL 수정
BEGIN
	DBMS_JOB.INTERVAL(${JOB_NUMBER}, '${주기}')
	COMMIT;
END;

--NEXT-DATE 변경
BEGIN
	DBMS_JOB.NEXT_DATE(${JOB_NUMBER}, ${변경일자})
	COMMIT;
END;

--강제 실행
BEGIN
	DBMS_JOB.RUN(${JOB_NUMBER})
	COMMIT;
END;

--제거
BEGIN
	DBMS_JOB.REMOVE(${JOB_NUMBER})
	COMMIT;
END;

--프로시저 or 구문 변경
EXECUTE DBMS_JOB.WHAT(${JOB_NUMBER}, '${프로시저} or ${반복할 구문}')
```