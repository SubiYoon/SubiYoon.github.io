---
tags:
  - Oracle
  - DataBase
---

### DUAL
**DUAL**은 CREATE를 사용하지 않고 가상으로 테이블을 생성하여 데이터를 보여 주고 싶을 때 사용합니다.
다음 예시를 sql에 작성하면 다음과 같이 결과가 나오게 됩니다.

```sql
SELECT '홍길동' AS 이름 FROM DAUL;
```

<img width="70" alt="image" src="https://github.com/SubiYoon/SubiYoon.github.io/assets/117332903/9bcac54f-f91b-4977-9eed-be666ed31724">

### DECODE
**DECODE**는 조건문이라고 생각하시면 됩니다.
한가지 예를 들어 볼게요.

```sql
SELECT DECODE(NAME, '홍길동', 10, '강감찬', 20) FROM DUAL;
```

위와 같은 쿼리가 있다고 생각해 봅시다.
위에서의 코드는 만약 **NAME**이 '홍길동'이라면 10을 반환하고 '강감찬'이라면 20을 반환하는 쿼리문이 됩니다.

우리는 Java나 JavaScript에서 if - else if 문을 본적이 있을 겁니다. 위와 같은 경우가 같은 문법이라고 생각하시면 이해하시기 편할 겁니다.

그럼 우리는 여기서 한가지 궁금한점이 발생합니다. if - else if 문이 있다면 if - else 문은??
물론!! 있습니다.
```sql
SELECT DECODE(NAME, '홍길동', 10, 50) FROM DUAL;
```

위의 쿼리는 **NAME**이 '홍길동'이면 10을 반환하고 그 외에는 50을 반환하라는 쿼리문이 됩니다.
자 그러면 우리는 이것을 응용하여 if- else if - else 문을 작성할 수 있습니다.

맞습니다!! 바로 아래와 같이 작성하는 것이죠.
```sql
SELECT DECODE(NAME, '홍길동', 10, '강감찬', 20, 50) FROM DUAL;
```

### Multi row, Column Table
자! 우리는 이제 궁극적으로 생성하고 싶은 테이블을 생성할 준비가 되어 있습니다.

복잡하게 생각하지 말고 하나하나씩 생각해보면 됩니다.

**첫째**
테이블을 생성하고 컬럼을 만들고 이러한 과정이 필요할까요??? 아닙니다. 우리는 가상테이블을 사용할 거에요.

**둘째**
**DEOCE**를 사용하여 데이터를 추출할 것입니다.
우리는 테이블에서 사용하는 행의 숫자를 알고 있습니다. 이것을 **rownum**이라고 하죠.
그럼 우리는 **DECODE**에 데이터값을 넣어주는 맨 앞부분에 행의 숫자를 기준으로 판별하면 됩니다.

```sql
SELECT
	DECODE(rownum, 1, '홍길동', 2, '강감찬', 3, '세종대왕') AS 이름
	, DECODE(rownum, 1, '의적', 2, '문관/장군', 3, '조선의 왕') AS 직업
FROM
	DUAL
CONNECT BY LEVEL <= 3;
```

쿼리문이 이해가 가셨나요??

참고로 **CONNECT BY LEVEL**은 조회하고 싶은 계층의 갯수라고 생각하시면 됩니다.
위의 쿼리에서는 3개 이하로 출력한다는 뜻이죠.

이제 결과를 봐볼까요??

<img width="161" alt="image" src="https://github.com/SubiYoon/SubiYoon.github.io/assets/117332903/c54adca2-2a3c-4efe-b231-e36ef7fb453f">

CONNECT BY LEVEL이 이해가 되지 않으셨다면, 4를 한번 넣어서 다시 테스트 해보겠습니다.
```sql
SELECT
	DECODE(rownum, 1, '홍길동', 2, '강감찬', 3, '세종대왕') AS 이름
	, DECODE(rownum, 1, '의적', 2, '문관/장군', 3, '조선의 왕') AS 직업
FROM
	DUAL
CONNECT BY LEVEL <= 4;
```
<img width="163" alt="image" src="https://github.com/SubiYoon/SubiYoon.github.io/assets/117332903/cf4641a2-8dbf-444a-ab9e-cb2075dddaba">
