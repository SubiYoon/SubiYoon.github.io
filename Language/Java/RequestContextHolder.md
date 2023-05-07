# RequestContextHolder

RequestContextHolder는 Spring 컨텍스트에서 HttpServletRequest에 직접 접근 할 수 있도록 도와주는 역할을 한다. 활용 시 HttpServletRequest를 사용하기 위해 이를 메소드 파라미터로 연이어 넘겨받을 필요가 없다.

**동작원리**

1. ThreadLocal을 사용하여 servlet이 호출되면 key, value(thread, HttpServletRequest)를 보관
2. 호출된 servlet과 동일한 thred내에서는 어느 곳에서든 같은 HttpServletRequest를 꺼낼 수 있음
3. servlet이 종료될 때 해당 thread를 key로 갖는 쌍을 제거

**HttpServletRequest를 사용하는 경우**

- Parameter로 데이터를 받아 주어야 함

```java
@RestController
@RequiredArgsConstructor
@Log4j2
public class TestController {

    private final TestService testService;

    @GetMapping("hello")
    public void hello(HttpServletRequest request){

        testService.hello(request);
    }    
}

@Service
@Log4j2
public class TestService {

    public void hello(HttpServletRequest request){

       log.info(request.getRequestURI());
    }    
}
```

**RequestContextHloder를 사용하는 경우**

- Parameter로 데이터를 받아 줄 필요가 없음

```java
@RestController
@RequiredArgsConstructor
@Log4j2
public class TestController {

    private final TestService testService;

    @GetMapping("hello")
    public void hello(){

        testService.hello(request);
    }    
}

@Service
@Log4j2
public class TestService {

    public void hello(){

        HttpServletRequest req = TestUtils.getCurrReq();

        log.info(req.getRequestURI());
    }    
}

public class TestUtils {

    public static HttpServletRequest getCurrReq(){

        return ((ServletRequestAttributes)RequestContextHolder.currentRequestAttributes()).getRequest();
    }
}
```

**RequestContextHolder에서 RequestAttribute를 구해오는 메서드**

- currentRequestAttributes()
    - 구해오려는 RquestAttributes가 없을 경우 예외를 발생 시킴

- getRequestAttributes()
    - 구해오려는 RequestAttributes가 없을 경우 null을 리턴