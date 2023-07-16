# Symbol

# Symbol( )

- 심볼은 생성자가 없으므로 new를 사용하여 객체를 생성하지 않는다.
- Symbol( ) 함수를 호출하면 매번 새로운(고유한) 심볼이 생성된다.
- 심볼은 전역 심볼 레지스트리(Global Symbol Resgistry)란 심볼들이 저장되는 전역공간에 저장된다.

```jsx
const sym1 = Symbol();
const sym2 = Symbol();
const sym3 = Symbol('foo');
const sym4 = Symbol('foo');

console.log(sym1 === sym1);  // true

console.log(sym1 === sym2);  // false
console.log(sym3 === sym4);  // false
```

- 일반적으로 심볼은 객체의 프로퍼티 키로 사용된다.
- JavaScript에서 객체의 프로포티 키는 대게 문자열 값이다. 숫자로 사용하는 것도 결국 문자로 변환됨

```jsx
const obj = {};

obj.propertyKey1 = 'propertyValue1';
obj['propertyKey2'] = 'propertyValue2';

obj[3] = 'propertyValue3';  // obj['3'] = 'propertyValue3'으로 변환

console.log(obj);
// {
//  propertyKey1: 'propertyValue1', 
//  propertyKey2: 'propertyValue2',
//  3: 'propertyValue3'
// }
```

- 문자열 값 대신 심볼로도 프로퍼티 키를 사용할 수도 있다는 것이다.

```jsx
const obj = {};

const sym1 = Symbol();
const sym2 = Symbol('foo');
const sym3 = Symbol('foo');

obj[sym1] = 'propertyValue1';
obj[sym2] = 'propertyValue2';
obj[sym3] = 'propertyValue3';  // no conflict with sym2

console.log(obj);  
// {
//  Symbol(): 'propertyValue1',
//  Symbol(foo): 'propertyValue2',
//  Symbol(foo): 'propertyValue3'
// }

console.log(obj[sym1]);  // propertyValue1
console.log(obj[sym2]);  // propertyValue2
console.log(obj[sym3]);  // propertyValue3
```

### 내장 심볼(Built-in Symbol)

- JavaScript 엔진 내에 미리 생성되어 상수로 존재하고 있는 내장 심볼(Bulit-inSymbol)
- 대표적인 예시가 Symbol.iterator이다.
- **예시**

```jsx
Array.prototype[Symbol.iterator];

String.prototype[Symbol.iterator];

Map.prototype[Symbol.iterator];

Set.prototype[Symbol.iterator];

arguments[Symbol.iterator];

NodeList.prototype[Symbol.iterator];

HTMLCollection.prototype[Symbol.iterator];
```

### Symbol.for( ) 메서드

- 인자로 전달받은 문자열 값을 키로 갖는 Symbol을 전역 심볼 레지스트리에서 찾아 반환
- But!! 탐색에 실패하면 해당 인자를 갖는 새로운 Symbol을 생성하고 이를 반환한다.

```jsx
const sym1 = Symbol.for('foo');  // Cretate symbol
const sym2 = Symbol.for('foo');  // Reuse symbol

console.log(sym1 === sym2);  // true
```

### Symbol.keyFor( ) 메서드

- 인자로 전달받은 Symbol을 전역 심볼 레지스트리에서 찾고, 그 심볼의 key를 반환
- But!! 탐색에 실패하면 undefined를 번환한다.

# 주의사항

### for … in 문법과 JSON.stringify( ) 메서드에서의 심볼

- for .. in 문법에서 키가 심볼인 프로퍼티들은 열거되지 않는다.

```jsx
const obj = {};

obj[Symbol('a')] = 'a';
obj[Symbol.for('b')] = 'b';
obj['c'] = 'c';
obj.d = 'd';

for (const propertyKey in obj) {
   console.log(propertyKey);  // logs 'c' and 'd'
}
```

- 객체를 JSON으로 만들 때(JSON.stringify( ) 메서드 호출)도 키가 Symbol이면 무시된다.

```jsx
const sym = Symbol('foo');
const obj = {
    [sym]: 'propertyValue1',
    propertyKey2: 'propertyValue2'
};

JSON.stringify(obj);  // {"propertyKey2":"propertyValue2"}
```