# MarkDown

> 1.  마크다운의 장/단점
> 

### #1. 장점

- 문법이 쉽고 간결하다
- 관리가 쉽다
- 별도의 도구없이 작성가능하다.
- 다양한 형태로 변환이 가능하다.
- 지원 가능한 플랫폼과 프로그램이 다양하다.
- 텍스트(Text)로 저장되기 때문에 용량이 적어 보관이 용이하다.

### **#2. 단점**

- 표준이 없다.
- 표준이 없기 때문에 표현하는 도구에 따라서 동작하지 않거나, 다르게 표현 될 수 있다.
- 모든 HTML 마크업을 대신하지 못하지 못하는 한계점.

> 2.  마크다운 문법(syntax)
> 

### # 1. 제목 (Header)

- # 뒤에 띄어쓰기를 넣어주는게 권장하는 방법 이다.
- <h1> ~ <h6> 까지 표현 가능하다.

```xml
# 제목1
## 제목 2
### 제목 3
#### 제목 4
##### 제목 5
###### 제목 6
```

![https://blog.kakaocdn.net/dn/yC4Uv/btrsHGArLO1/4xk5yxpTnTAddepz1XZagk/img.png](https://blog.kakaocdn.net/dn/yC4Uv/btrsHGArLO1/4xk5yxpTnTAddepz1XZagk/img.png)

### #2. 줄바꿈 (Line Breaks)

- 띄어쓰기 2번 또는 <br/>로 표현 가능 하다.

```xml
# 줄바꿈(Line Breaks)
띄어쓰기 2번
또는 <br/>
```

![https://blog.kakaocdn.net/dn/vnaaH/btrsLxXjR01/BiFOE6Ug9s0hoOy5Icrsek/img.png](https://blog.kakaocdn.net/dn/vnaaH/btrsLxXjR01/BiFOE6Ug9s0hoOy5Icrsek/img.png)

### #3. 수평선 (Horizontal Rule) <hr/>

- 하기 코드들은 모두 수평선을 나타낸다. 가시적으로 페이지를 나누는 용도로 많이 사용된다.

```xml
---
***
___
******
```

![https://blog.kakaocdn.net/dn/bozDQY/btrsMzG6NYR/W91spik7ioxObaOjBFQWO1/img.png](https://blog.kakaocdn.net/dn/bozDQY/btrsMzG6NYR/W91spik7ioxObaOjBFQWO1/img.png)

### #4. 글자 강조 (Emphasis)

```xml
**굵은 글씨**
*이텔릭*
_이탤릭_
~~취소선~~
<u>밑줄</u>
ex)
This is the **bold** text and this is the *italic* text and <u>let's</u> do ~~strikethrough~~
```

![https://blog.kakaocdn.net/dn/CfLQD/btrsMzUEaPP/Tf0peN8z2fx2uiACdM2QxK/img.png](https://blog.kakaocdn.net/dn/CfLQD/btrsMzUEaPP/Tf0peN8z2fx2uiACdM2QxK/img.png)

### #5. 인용문 (BlockQuote)

- ">"  블럭 인용 문자를 사용하면 인용문 표현이 가능하다.

```xml
> 인용문장
>> 중첩된 인용문
>>> 중첩된 인용문 2
```

![https://blog.kakaocdn.net/dn/qCQJx/btrsLxJLd9X/h9vRJidq0FK8k6IeSPYy01/img.png](https://blog.kakaocdn.net/dn/qCQJx/btrsLxJLd9X/h9vRJidq0FK8k6IeSPYy01/img.png)

### **#6. 목록(List)**

**1) 순서가 없는 목록 ( *, +, - 지원)**

```xml
- 순서가 필요하지 않은 목록
    - 순서가 필요하지 않은 목록
        - 순서가 필요하지 않은 목록
* 순서가 필요하지 않은 목록
    * 순서가 필요하지 않은 목록
        * 순서가 필요하지 않은 목록
+ 순서가 필요하지 않은 목록
    + 순서가 필요하지 않은 목록
        + 순서가 필요하지 않은 목록

- 순서가 필요하지 않은 목록
    * 순서가 필요하지 않은 목록
    + 순서가 필요하지 않은 목록
```

![https://blog.kakaocdn.net/dn/bTgedw/btrsMWhSyfw/iornNSiVYb1NuaiICVvLX0/img.png](https://blog.kakaocdn.net/dn/bTgedw/btrsMWhSyfw/iornNSiVYb1NuaiICVvLX0/img.png)

**2) 순서가 있는 목록**

```xml
1. 순서가 필요한 목록
    1. 순서가 필요한 목록
    1. 순서가 필요한 목록
1. 순서가 필요한 목록

1. 순서가 필요한 목록
    9. 순서가 필요한 목록
    3. 순서가 필요한 목록
8. 순서가 필요한 목록
```

![https://blog.kakaocdn.net/dn/yNzXv/btrsyFB74QR/WskKNDDXXfuWKwGw6ltjv1/img.png](https://blog.kakaocdn.net/dn/yNzXv/btrsyFB74QR/WskKNDDXXfuWKwGw6ltjv1/img.png)

**3) 혼합 사용하는 예시**

```xml
- 순서가 필요하지 않은 목록
    1. 순서가 필요한 목록
    1. 순서가 필요한 목록
- 순서가 필요하지 않은 목록2
    1. 순서가 필요한 목록
    3. 순서가 필요한 목록
    8. 순서가 필요한 목록
```

![https://blog.kakaocdn.net/dn/czDTia/btrsxiAAjIo/8E6arwq7pok0hLq9izO0j1/img.png](https://blog.kakaocdn.net/dn/czDTia/btrsxiAAjIo/8E6arwq7pok0hLq9izO0j1/img.png)

### **#7. 링크 (Links)**

**1) 기본방법**

- [**Title**](**link**)

```xml
Click [here](http://goddaehee.tistory.com/)
[갓대희의 작은 공간](https://goddaehee.tistory.com)
```

![https://blog.kakaocdn.net/dn/Git4T/btrsH8KbCZf/Cpr052cCBjICMYeAUke0OK/img.png](https://blog.kakaocdn.net/dn/Git4T/btrsH8KbCZf/Cpr052cCBjICMYeAUke0OK/img.png)

**2) 참조 링크 사용 방법**

- [**link keyword**][**id**]
- [**id**]: **URL** "**Optional Title**"

**ex) title 옵션 없이 사용**

```xml
[갓대희의 작은 공간][gdh]
[gdh]: https://goddaehee.tistory.com
```

![https://blog.kakaocdn.net/dn/beO1S9/btrsMaOo7vm/xSXqPWbDwstUJrbDXTkmdk/img.png](https://blog.kakaocdn.net/dn/beO1S9/btrsMaOo7vm/xSXqPWbDwstUJrbDXTkmdk/img.png)

**ex2) title 옵션사용시 커서를 링크 위로 위치하면, title이 노출된다.**

```xml
[갓대희의 작은 공간][gdh]
[gdh]: https://goddaehee.tistory.com "Click here"
```

![https://blog.kakaocdn.net/dn/dQ2Tpl/btrsLwqCyLZ/Ymv0zQJfAT6tWRunAPGry0/img.png](https://blog.kakaocdn.net/dn/dQ2Tpl/btrsLwqCyLZ/Ymv0zQJfAT6tWRunAPGry0/img.png)

**3) 자동 링크 사용 방법**

- 인터넷 URL 혹은 이메일 주소를 적합한 형식으로 링크를 생성해준다.

```xml
https://goddaehee.tistory.com
goddaehee@kakao.com
```

[https://goddaehee.tistory.com](https://goddaehee.tistory.com/)

goddaehee@kakao.com

### **#8. 이미지 (Images)**

- Link와 문법이 유사하다. 앞에 !만 추가하면 된다.

**1) 기본문법**


```xml
 ![**대체텍스트**](**이미지주소**)

![Image Description](https://tistory1.daumcdn.net/tistory/1994430/74cb0667c02241939c5c13ec5dd1afb1)
```

![https://blog.kakaocdn.net/dn/ntsvo/btrsDQXnTqk/ononkXceqyDoKqKhTlv0vk/img.png](https://blog.kakaocdn.net/dn/ntsvo/btrsDQXnTqk/ononkXceqyDoKqKhTlv0vk/img.png)

**2) 참조 링크 사용 방법**

- [**대체텍스트**][**id**]
- [**id**]: **이미지주소** "**Optional Title**"

```xml
![Image Description](gdh)
[gdh]:https://tistory1.daumcdn.net/tistory/1994430/74cb0667c02241939c5c13ec5dd1afb1
```

![https://blog.kakaocdn.net/dn/bC5RFG/btrsNxh1B1Q/Rv5BJnFCtKXwc83b0UL2kK/img.png](https://blog.kakaocdn.net/dn/bC5RFG/btrsNxh1B1Q/Rv5BJnFCtKXwc83b0UL2kK/img.png)

**3) 이미지 노출과 동시에 링크처리**


```xml
[![**대체텍스트**](**이미지주소**)](**링크주소**)

[![Image Description](https://tistory1.daumcdn.net/tistory/1994430/74cb0667c02241939c5c13ec5dd1afb1)](http://goddaehee.tistory.com)
```

![https://blog.kakaocdn.net/dn/durRGO/btrsFutBAwq/aA9ndAqrCy6Ij9ymZxDojk/img.png](https://blog.kakaocdn.net/dn/durRGO/btrsFutBAwq/aA9ndAqrCy6Ij9ymZxDojk/img.png)

### **#9. 표 (Table)**

- |(vertical bar) 기호를 통해 테이블을 표현 가능. (가장 좌측, 우측 생략 가능)
- 헤더와 셀을 구분할 때 3개 이상의 -(하이픈, 대시)가 필요 하다.
- : (콜론) 기호를 통해 정렬할 수 있다.

```xml
| Header | value | Description |
| --: | :-- | :--: |
| 정렬 | --: | 우측정렬 |
| 정렬 | :-- | 좌측정렬 |
| 정렬 | :--: | 가운데정렬 |
```

![https://blog.kakaocdn.net/dn/cREbqv/btrszlRIAC4/2F6XQtlqPH7SXEaUwPJIRk/img.png](https://blog.kakaocdn.net/dn/cREbqv/btrszlRIAC4/2F6XQtlqPH7SXEaUwPJIRk/img.png)

### **#10. 코드 (Code)**

**1) 인라인 코드(Inline Code)**

- 백틱(` : 숫자 1번 키 왼쪽에 위치)으로 강조할 내용을 감싸면 된다.

```xml
`해당 코드`는 강조할 부분 이다.
```

![https://blog.kakaocdn.net/dn/biWwIX/btrsA7r9ynD/qxOJ7IEstkVyjsDnWhanYk/img.png](https://blog.kakaocdn.net/dn/biWwIX/btrsA7r9ynD/qxOJ7IEstkVyjsDnWhanYk/img.png)

**2) 블럭 코드(Block Code)**

- ``` html, css, javascript, bash, plaintext 등등
- 코드의 종류를 명시 하지 않은 경우

![https://blog.kakaocdn.net/dn/blhPyD/btrsL99MPpG/3OWLXbDCkLYKpLzrmKjJzk/img.png](https://blog.kakaocdn.net/dn/blhPyD/btrsL99MPpG/3OWLXbDCkLYKpLzrmKjJzk/img.png)

- **html**

![https://blog.kakaocdn.net/dn/bKw2LF/btrsMaATpcJ/3MjSjlo3zaSmeXlBHBDO7K/img.png](https://blog.kakaocdn.net/dn/bKw2LF/btrsMaATpcJ/3MjSjlo3zaSmeXlBHBDO7K/img.png)

- **CSS**

![https://blog.kakaocdn.net/dn/rMomg/btrsLwqEHzu/MAPAxue50YiIT7UbWXX4yk/img.png](https://blog.kakaocdn.net/dn/rMomg/btrsLwqEHzu/MAPAxue50YiIT7UbWXX4yk/img.png)

- **javascript**

![https://blog.kakaocdn.net/dn/coqne7/btrsMATC9F1/0lKgmWYy80SqZRkXet7D31/img.png](https://blog.kakaocdn.net/dn/coqne7/btrsMATC9F1/0lKgmWYy80SqZRkXet7D31/img.png)

- **bash**

![https://blog.kakaocdn.net/dn/uobz2/btrsLwjRJki/8vipKuwmWt7xQQxHuQyeu1/img.png](https://blog.kakaocdn.net/dn/uobz2/btrsLwjRJki/8vipKuwmWt7xQQxHuQyeu1/img.png)

- **이외 텍스트**

![https://blog.kakaocdn.net/dn/wyh7y/btrsFsP6OnG/bfPTXwWjQuT8mDi7MNMpo1/img.png](https://blog.kakaocdn.net/dn/wyh7y/btrsFsP6OnG/bfPTXwWjQuT8mDi7MNMpo1/img.png)

### **#11. 원시 HTML (Raw HTML)**

- MarkDown 환경에서는 결국 표현의 한계가 있고, 이런 경우 순수 html문법을 사용할 수 있다.

**ex) image를 표현할 때 MarkDown으로는 width 지정이 불가능 하다.**

```xml
<img width="70" src="" alt="gods" />
```

**ex) 링크를 표현할 때 target="_blank" 지정이 불가능 하다.**

**ex) 밑줄을 표현할 수 없어 <u></u>를 사용하거나 다음과 같이 style을 직접 지정 해줘야 한다.**

```xml
<span style="text-decoration: underline;">갓대희</span> 티스토리 입니다.
```