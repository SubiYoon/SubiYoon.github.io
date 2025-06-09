---
tags:
  - Language
  - JavaScript
  - typeof
---
## 왜??
개발을 하던중, Object의 데이터를 for문을 통해 데이터를 검증하는 과정을 거쳐야 하는 상황이 발생했습니다.
이 때, 한 가지 문제가 발생하게 되었는데요.
하나의 텍스트문장의 데이터탐색하며 objectKeys의 key값과 동일한게 있다면 replace하는 로직이었습니다.
처음 개발을 할 때에는 한 Object안에 특성별로 한번 더 Object로 구분을 해서 탐색을 하는 로직을 구현했었습니다.
예를 들면 이런 방법이죠.

```javascript
let objectKeys = {
    case1 : {
        INTER : '\\INERT',
        OUTER : '\\OUTER',
        ...
    },

    case2 : {
        IN : '\\IN',
        OUT : '\\OUT',
    },
}
```

위의 데이터는 case에 따라 로직이 다르게 동작하여야 하는 상황이 었습니다.
저는 switch-caes문을 통해 구현을 했었는데요.

```javascript
for (let key of objectKeys) {  
    switch (key) {  
        // 대소문자 구분이 필요한 명령어  
        case 'cases1' :  ... break;
        case 'cases2' :  ... break;
```

문제는 objectKeys를 모두 탐색해야 때문에 INTER를 진행해도 결국은 IN을 다시 타고 내려오게 되어 텍스트의 데이터가 \\\\INTER에서 끝나야 하는데 \\\\\\\\INTER가 되어 버리는 현상이 발생했습니다.

이 때문에 두 케이스를 합쳐 type이 object이면 다른 동작을 하게 구현을 하게 되었습니다.
이 때, typeof를 사용하게 되었죠.

자! 서두가 길었습니다. 이제  typeof를 어찌 사용하는지 알아 볼까요?

## typeof
> typeof는 javascript에서 사용중인 변수의 데이터 타입을 string으로 반환합니다.

### 데이터 다입별 반환값
```javascript
console.log(typeof 1) //number
console.log(typeof 'javasciprt') //string
console.log(typeof true) //boolean
console.log(typeof {}) //object
```

### 사용 예시
그럼 우리는 typeof를 어떠한 방식으로 사용할 수 있을까요??
제가 발생했던 문제를 해결하기 위한 방식으로 제시했던 해당 데이터가 object이면??
이럴 때 사용할 수 있겠죠??

```javascript
let objectKeys = {
    case1 : {
        INTER : {command : '\\INERT', isOtherCase : true},
        OUTER : {command : '\\OUTER', isOtherCase : true},
        IN : '\\IN',
        OUT : '\\OUT',
        ...
    },
}

for (let keys of objectKeys) {  
    switch (keys) {  
        case 'cases' :
            for (let key of keys) {
	            if ('object' === keys) {
	               action1();
	            } else {
	               action2();
	            }
            }
            break;
```

위의 로직은 objectKeys값이 'case1'일 때,
그 안의 key값이 object라면 action1함수를 아니면 action2함수를 동작하게 하는 로직입니다.
위의 예시를 보면 else일 경우는 전부 string을 반환하겠죠? 그럼 아래와 같이 작성하셔도 무방합니다.

```javascript
for (let keys of objectKeys) {  
    switch (keys) {  
        case 'cases' :
            for (let key of keys) {
	            if ('object' === keys) {
	               action1();
	            } else if ('string' === keys) {
	               action2();
	            }
            }
            break;
```

> 제가 잘못 알고 있거나 문제가 있다면, 알려주시면 수정하겠습니다.
> 알려주시면 저도 배우는 것이니 지적은 달게 받겠습니다. 감사합니다.