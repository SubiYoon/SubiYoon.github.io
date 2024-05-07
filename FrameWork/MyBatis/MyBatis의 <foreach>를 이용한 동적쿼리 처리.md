---
tags:
  - DataBase
  - MyBatis
---
# 계기
개인 프로젝트 중 배열에 선언되어 있는 여러 P.K. 값을 기준으로 데이터를 꺼내와야 하는 경우가 발생해 사용하게 되었다.

다음과 같은 원인으로 foreach문을 사용해서 해결하고자 한다.
1. 반복문을 통해 여러번 쿼리를 날릴 수 있지만, DB는 커넥션을 맺는 cost가 매우 비싸 조회해 올 데이터가 많아지면 cost의 낭비가 너무 심하다.

# 사용방법
### arguments
* collection : 전달받은 인자의 데이터 타입 (list or array 소문자로만 입력해야한다.)
* item : 전달 받은 데이터를 사용할 변수명
* open : 구문이 시작될 때 삽입할 문자
* close : 구문이 끝날 때 사입할 문자
* separator : 각 데이터를 넣을 때 마다 넣어줄 구분자
* index : 반복되는 구문 번호. 0부터 순차적으로 증가한다.

### 문법
```xml
<foreach collection="${list or array}" item="alias"></foreach>
```

### 예시
```xml
SELECT STACK_SEQ,  
       ALIAS,  
       STACK_NAME,  
       STACK_LEVEL,  
       STACK_IMAGE,  
       REGIST_TS,  
       UPDATE_TS,  
       CATEGORY,  
       STACK_DETAIL,  
       CATEGORY_LEVEL,  
       PROFILE_VIEW_YN  
FROM MY_STACK  
WHERE  
STACK_SEQ IN  
<foreach collection="array" item="stackSeqs" index="index" open="(" separator="," close=")">  
    #{stackSeqs}  
</foreach>
```