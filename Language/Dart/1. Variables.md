# 1. Variables

# 1-1) Hello World

### void main()

- java의 main메서드와 같은 역할 

### ;(세미콜론)을 붙여주어야 실행이 된다

- 세미콜론을 사용하지 않는 경우가 있기 때문

---

# 1-2) The Var Keyword

### var는 모든 DataType을 추측하여 해당 자료형으로 선언됨

### var를 사용할 경우

- 함수나 메소드 내부에 지역변수를 선언할 때 사용

### 명시적으로 자료형을 작성할 경우

- class안에서의 변수를 선언 할 때 사용

---

# 1-3) Dynamic Type

- 다양한 타입의 데이터를 변수에 담을 수 있음
- 사용하는데에는 유용하지만 추천하진 않음

```dart
var name;
// or
dynamic name;
name = 'nico';
name = 12;
name = true;
```

```dart
//dynamic type 활용
void main(){
  dynamic name;
 
  if(name is String){
    name.toString();
  }else if(name is int){
	  name.isOdd;
  }
}
```

---

# 1-4) Nullable Variables

- 개발자가 null을 참조할 수 없게 함

ex) null safety가 없는 코드

- NoSuchMethodError 발생함
    - null에는 길이(length)가 없기에 발생하는 에러

```dart
bool isEmpty(String string) => string.length == 0;

main(){
  isEmpty(null);
}
```

ex) null safety가 있는 코드

- DataType에 ‘?’로 붙여 null값이 들어 올 수 있다는 것을 명시
- var에는 ‘?’를 붙일 수 없다.

```dart
void main(){
  String? nico = 'nico';
  nico = null;
//
  if(nico != null){
    nico.isNotEmpty;
  }
//위 if문과 같은 뜻으로 아래와 같이 사용 가능
	nico?.isNotEmpty;
}
```

---

# 1-5) Final Variables

- DataType을 ‘final’로 선언
- javascript 또는 typescript의 const와 같음

```dart
final name = "nico";

//타입을 선언해서 사용 할 수 있음
final String name = "nico";
final int num = 13;
```

---

# 1-6) Late Variables

- DataType 앞에 쓰여지는 예약어
- 나중에 변수의 값을 선언할 수 있게 해주는 예약어

```dart
late final String name;

//late 예약어가 사용되어 변수값을 설정해주기 전까지는 사용 할 수 없어 오류가 발생
print(name); //오류 발생

name = "nico";
print(name); //정상 작동
```

---

# 1-7) Constant Variables

- final과 같이 상수역할을 함
- const예약어는 javascript 또는 typescript의 const와는 다름
- compile-time constant를 만들어줌
    - 컴파일 전에 값을 알고 있어야함
    - if API로 해당 값을 가져 올 경우 컴파일 이후에 값을 가져오기 때문에 사용 불가