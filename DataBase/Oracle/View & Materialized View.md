---
tags:
  - Oracle
---
# View

## View 란?
- 실체가 존재하지 않는 다는 의미로 **가상 테이블**이라고도 불린다.
- 반복적인 조회를 단순하게 만들어 줄 수 있다.
	- ex) 특정 테이블의 특정 컬럼들만 자주 불러와서 사용하는 경우.
	- ex) 특정 테이블들을 UNION하여 가져 올 경우.
	- ex) 특정 DB LINK를 반복적으로 사용할 경우. 등 등
- **SELECT만 가능**하고 UPDATE, DELETE, INSERT는 불가능하다.

## View의 단점
- 데이터베이스의 객체로서 저장장치에 저장된다.(SELECT 구문을 저장)

## View 생성 방법
```sql
-- 컬럼명을 변경없이 상용할 경우
CREATE VIEW ${viewName} AS ${SELECT구문}

-- 컬럼명을 변경하여 사용할 경우
CREATE VIEW ${viewName}(${colName1}, ${colName2}, ...) AS ${SELECT구문}
```

## View 삭제 방법
```sql
DROP VIEW ${vuewName}
```

---
# Materialized View

## Materialized View 란??
- 8i 이후부터 사용이 가능하고 **구체화 뷰(MView)** 라고 불린다.
- 기존 View와 비슷하게 동작하지만 실제데이터를 가지고 있다는 차이가 있다.
	- 원본테이블에 **INSERT,UPDATE, DELETE**가 발생하면 MView에도 반영이 된다.
- **Index**를 생성하여 조회 성능을 향상 시킬 수 있다.

## Materialized View 생성
```sql
CREATE MATERIALIZED VIEW view_name

BUILD [IMMEDIATE | DEFERRED]

REFRESH [ FAST | COMPLETE | FORCE | NEVER ]

ON [COMMMT | DEMAND]

[[ENABLE | DISABLE] QUERY REWRITE [ON PREBUILT TABLE]]

[[ENABLE | DISABLE] ON QUERY COMPUTATION

AS

SELECT 문장;
```

### BUILD
- IMMEDIATE : MView 생성과 동시에 데이터들도 생성되는 옵션.
- DEFERRED : MView 생성은 하지만, 그 안의 데이터는 추후에 생성하도록 하는 기능.
- MView 생성시 BUILD IMMEDIATE 대신 BUILD DEFERRED 옵션을 사용하면 조회되는 데이터가 없다.

### REFRESH
- 구체화된 뷰가 생성되자마자 바로 실행 가능한 상태로 생성한다.
- REFRESH 절은 아래에 정의된 AS절에서 사용된 SELECT문 내의 원본 테이블 데이터가 변경되면 구체화된 뷰를 언제 변경할 것인지에 대한 일정을 결정하게 된다.
	- FAST : 원본 테이블에 변경된 데이터만 구체화 뷰에 갱신한다. FAST REFRESH가 동작하기 위해서는 뷰의 마스터 테이블에MATERIALIZED VIEW LOG가 있어야 한다.
		- CREATE MATERIALIZED VIEW LOG ON emp  WITH PRIMARY KEY, ROWID INCLUDING NEW VALUES;
	- COMPLETE : 원본테이블이 변경되면 전부 갱신한다.
	- FORCE : FAST와 동일한 기능을 한다.
	- NEVER : 원본테이블이 갱신되어도 뷰에 반영하지 않는다.

### ON
- COMMIT : 원본 테이블에 COMMIT이 일어날 때 REFRESH가 동작한다.
	- 이는 1 개의 테이블에 COUNT( ), SUM( )과 같은 집합 함수를 사용하거나, MVIEW에 조인만이 있는 경우, Group By 절에 사용된 컬럼에 대해 COUNT(col) 함수가 기술된 경우에만 사용 가능하다.
- DEMAND : MView가 자동으로 갱신되지 않으며 DBMS_MVIEW 패키지(REFRESH, REFRESH_ALL_MVIEWS, REFRESH_DEPENDENT)의 프로시저를 실행해서 MView를 갱신한다.

### • ENABLE/DISABLE QUERY REWRITE
- 일반 USER가 작성한 SQL 문이 구체화 된 뷰를 통해 데이터를 검색하는 것이 데이터를 더 빨리 찾을 수 있다고 분석되면 사용자의 SQL문을 구체화 뷰를 통해 검색하게 하는 기능

### ON PREBULIT TABLE
- CREATE TABLE AS SELECT…(CTAS)로 이미 테이블이 생성된 경우 이를 QUERY REWRITE가 가능하도록 MVIEW로 만드는 옵션 입니다.

### ENABLE/DISABLE ON QUERY COMPUTATION
- 오라클 13.2 이상에서 사용되며 실시간 MView를 위해서는 반드시 사용해야 한다.
- MView를 별도로 갱신하지 않더라도 MView가 마치 갱신된 것 처럼 변경된 내용을 확인 가능하다.

### AS
• 일반 View 처럼 AS 구문 뒤에 필요한 컬럼과 조건들로 SELECT문을 기술한다.
