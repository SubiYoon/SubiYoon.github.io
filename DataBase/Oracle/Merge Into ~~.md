---
tags:
  - Oracle
---
# Merge Into ~~

오라클에서 하나의 쿼리문으로 Insert, Update, Delete를 한번에 해야 하는 경우 사용한다.

- Insert, Update는 9i부터 사용이 가능
- Delete는 10g부터 사용이 가능

```sql
MERGE
 INTO job_history AS a ('테이블 || 뷰')
USING emp AS b ('테이블 || 뷰 || 서브쿼리')
   ON (a.empno = b.empno) ('조건절')
 WHEN MATCHED THEN ('일치하는 경우')
      UPDATE SET a.job = b.job
		  DELETE
 WHEN NOT MATCHED THEN ('불일치하는 경우')
      INSERT (a.empno, a.job, a.deptno)
      VALUES (b.empno, b,job, b.deptno);
```

### 단일 테이블 사용법(dual)

- 단일 테이블 사용시 가상테이블인 dual을 사용하여 처리한다.
- 일치할 경우 Update를 실행하고 불일치할 경우 Insert를 실행하는 Query문

```sql
MERGE 
 INTO emp AS a
USING dual
   ON (a.empno = 7788)
 WHEN MATCHED THEN
      UPDATE
         SET a.deptno = 20
 WHEN NOT MATCHED THEN
      INSERT (a.empno, a.ename, a.deptno)
      VALUES (7788, 'SCOTT', 20);
```

### 조인을 사용하는 방법

- USING절에 join할 Table을 입력하여 사용한다.

```sql
MERGE 
 INTO job_history AS a
USING emp AS b
   ON (a.empno = 7788 AND a.empno = b.empno)
 WHEN MATCHED THEN
      UPDATE
         SET a.job = b.job,
             a.deptno = b.deptno
 WHEN NOT MATCHED THEN
      INSERT (a.empno, a.job, a.deptno)
      VALUES (b.empno, b.job, b.deptno);
```

### 인라인뷰(서브쿼리)를 사용하는 방법

```sql
MERGE 
 INTO emp AS a
USING (SELECT aa.empno,
	            aa.job,
              aa.deptno
         FROM emp AS aa,
              dept AS bb
        WHERE aa.empno = 7788
          AND aa.deptno = bb.deptno) AS b
   ON (a.empno = b.empno)
 WHEN MATCHED THEN
      UPDATE
         SET a.job = b.job,
             a.deptno = b.deptno
 WHEN NOT MATCHED THEN
      INSERT (a.empno, a.job, a.deptno)
      VALUES (b.empno, b.job, b.deptno);
```

### WHERE절 사용(10g 이상부터 가능)

- 10g 이상부터는 Update와 DELETE절에 WHERE절을 사용 할 수 있다.

```sql
MERGE 
 INTO emp AS a
USING dual
   ON (a.empno = 7788)
 WHEN MATCHED THEN
      UPDATE
         SET a.deptno = 20
       WHERE a.job = 'ANALYST';
```

### DELETE절 사용(10g 이상부터 가능)

- 10g 이상부터는 DELETE절을 사용 할 수 있다.
- 조건절인 WHERE절을 사용하지 않으면 MATCH된 모든 데이터가 삭제된다.

```sql
MERGE 
 INTO emp AS a
USING dual
   ON (a.empno = 7788)
 WHEN MATCHED THEN
      UPDATE
         SET a.deptno = 20
       WHERE a.job = 'ANALYST'
      DELETE
       WHERE a.job <> 'ANALYST';
```

---

# 주의사항

- 조건절 ON에 사용한 컬럼을 Update하면 오류가 발생한다.

```sql
MERGE 
 INTO emp AS a
USING (SELECT aa.empno,
	            aa.job,
              aa.deptno
         FROM emp AS aa,
              dept AS bb
        WHERE aa.empno = 7788
          AND aa.deptno = bb.deptno) AS b
   ON (a.empno = b.empno AND a.job = b.job) -- a.job = b.job 에서 오류발생
 WHEN MATCHED THEN
      UPDATE
         SET a.job = b.job, -- a.job = b.job 에서 오류 발생
             a.deptno = b.deptno
```

> **SQL 오류: ORA-38104: ON 절에서 참조되는 열은 업데이트할 수 없음: "A"."JOB"**
> 
> 
> **38104. 00000 -  "Columns referenced in the ON Clause cannot be updated: %s"**
>