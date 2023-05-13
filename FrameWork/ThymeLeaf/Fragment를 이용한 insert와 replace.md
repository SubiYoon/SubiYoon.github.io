# Fragment를 이용한 insert와 replace

- 템플릿을 생성하여 생성한 템플릿을 insert하거나 replace하여 사용하는 방식

**header.html**

- th:fragment=”템플릿 이름”

```html
<!DOCTYPE html>
<html lang="ko" xmlns="http://www.thymeleaf.org">
<head>
  <meta charset="UTF-8">
  <meta http-equiv="X-UA-Compatible" content="IE=edge">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Document</title>
</head>
<body>

  <h1 th:fragment="header">Header</h1>

</body>
</html>
```

**main.html**

- insert와 replace의 사용 방법은 동일하다.
    - th:insert=”~{해당 파일 경로 :: fragment이름}”
        - 해당 태그 안에 fragment를 삽입하는 방법
    - th:replace=”~{해당 파일 경로 :: fragment이름}”
        - 해당 태그를 fragment로 대체하는 방법

```html
<!DOCTYPE html>
<html lang="ko" xmlns="http://www.thymeleaf.org">
<head>
  <meta charset="UTF-8">
  <meta http-equiv="X-UA-Compatible" content="IE=edge">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Document</title>
</head>
<body>

  <div th:insert="~{layouts/header :: header}"></div>
  <div th:replace="~{layouts/header :: header}"></div>
  <h1>Main Page</h1>

</body>
</html>
```

![Untitled](https://github.com/SubiYoon/SubiYoon.github.io/assets/117332903/69cf93bc-7e87-4cf0-ab3e-c6ef40078f83)


- **데이터를 넘겨받아 템플릿에서 사용 할 수도 있다.**

**User.java**

```java
public class User {
  private String name;
  private int age;

  public User(String name, int age) {
    this.name = name;
    this.age = age;
  }

  public String getName() {
    return name;
  }
  public int getAge() {
    return age;
  }

}
```

**MainController.java**

```java
@Controller
@RequestMapping("/main")
public class MainController {

  @GetMapping("")
  public String main(Model model) {
    model.addAttribute("user", new Obj("Kim", 20));
    return "main";  
  }

}
```

**header.html**

```html
<!DOCTYPE html>
<html lang="ko" xmlns="http://www.thymeleaf.org">
<head>
  <meta charset="UTF-8">
  <meta http-equiv="X-UA-Compatible" content="IE=edge">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Document</title>
</head>
<body>

  <div th:insert="~{layouts/header :: header(${user})}"></div>
  <h1>Main Page</h1>

</body>
</html>
```

**main.html**

```html
<!DOCTYPE html>
<html lang="ko" xmlns="http://www.thymeleaf.org">
<head>
  <meta charset="UTF-8">
  <meta http-equiv="X-UA-Compatible" content="IE=edge">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Document</title>
</head>
<body>

  <h1 th:fragment="header(user)">
    <p th:text="${user.name}"></p> 
    <p th:text="${user.age}"></p>
  </h1>

</body>
</html>
```
