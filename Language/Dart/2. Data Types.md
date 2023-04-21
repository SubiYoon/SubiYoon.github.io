# 2. Data Types

# 2-0) Basic Data Types

## num

![스크린샷 2023-04-05 오전 11.29.17.png](2%20Data%20Types%20c0be9ca9f99b4a9d8882b32abcb21100/%25EC%258A%25A4%25ED%2581%25AC%25EB%25A6%25B0%25EC%2583%25B7_2023-04-05_%25EC%2598%25A4%25EC%25A0%2584_11.29.17.png)

![스크린샷 2023-04-05 오전 11.29.28.png](2%20Data%20Types%20c0be9ca9f99b4a9d8882b32abcb21100/%25EC%258A%25A4%25ED%2581%25AC%25EB%25A6%25B0%25EC%2583%25B7_2023-04-05_%25EC%2598%25A4%25EC%25A0%2584_11.29.28.png)

- int 와 double은 num을 상속받고 있어 자료형 중 num을 사용하여 정수, 실수 둘다 사용 할 수 있음

---

# 2-1) List

- [ ] 대괄호를 사용하여 선언

- 생성 방법

```dart
var numbers = [1, 2, 3, 4];
numbers.add(1);
//위와 같음
List<int> numbers = [1, 2, 3, 4];
numbers.add(1);
```

- collection if

```dart
var giveMeFive = true;
  var numbers = [
    1, 
    2, 
    3, 
    4,
    if(giveMeFive) 5,
  ];
```

- collection for

```dart
var oldFriends = ['nico', 'lynn'];
var newFriends = [
  'lewis',
  'ralph',
  'darren',
  for(var friend in oldFriends) "$friend"
];

print(newFriends);
```

---

# 2-2) String Interpolation

- $ 를 이용하여 해당 변수를 호출 할 수 있음
- 연산이 필요할 경우 { } 를 사용하여 처리 할 수 있음

```dart
var name = "King";
var age = 10;
var greeting = "Hello my name is $name nad I'm ${age+2} nice to meet you!";

print(greeting);
```

---

# 2-3) Maps

- { } 중괄호를 사용하여 json 방식으로 선언

```dart
var player = {
  'name': 'nico',
  'xp': 19.99,
  'superpower': false,
};

Map<int, bool> player2 = {
  1: true,
  2: false,
  3: false,
};

Map<List<int>, bool> player3 = {
  [1, 2, 3, 4]: true,
};

List<Map<String, Object>> player4 = [
  {'name': 'nico', 'xp': 19999393.99},
  {'name': 'kim', 'xp': 12394.22}
];
```

# 2-4) Sets

- { } 중괄호를 사용하여 선언

```dart
var numbers = {1, 2, 3, 4};
```