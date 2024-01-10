# HandlerInterceptor, HandlerInterceptorAdapter
애노테이션의 선언 없이 사용 하는 Spring에서의 **Class(HandlerInterceptorAdapter)** 와 **Interface(HandlerInterceptor)** 이다. 당연스럽게도 Class이므로 상속받아 사용하고, Interface이므로 구현해 사용해야 한다.
차이점 이라면 PreHandle, PostHandle, afterCompletion 세가지 메서드를 모두 구현해 주여야하는 Interface와 선택해서 Override할 수 있는 Class의 차이라고 할 수 있다.

### preHandle
**preHandle**은 Controller가 실행 되기 이전에 url을 식별 or 매순간에 중간에 캐치하여 작성한 로직을 우선적으로 실행시키는 interceptor Method이다.
false를 반환하면 원래 목적지(API)의 Controller로 향하지 않게 된다.
true를 반환하면 원래 목적지(API)의 Controller로 향하게 된다.

### postHandle
**postHandle**은 Controller의 처리가 끝난 후 해당 front Controller에서 DispacherServelt이 화면을 처리하기 전에 동작하게 된다.

### afterCompletion
**afterCompletion**은 뷰(view)가 모두 처리된 후에 동작하게 하는 부분이다.

# 예시
preHandle을 사용한 로직의 예시를 들어보겠다.
다음 예시는 페이지에 진입시 로그인 세션이 없으면 로그인 페이지로 보내는 interceptor를 작성한 코드이다.

**LoginInterceptor.java**
```java
public class loginInterceptor extends HandlerInterceptorAdapter {
	@Override
	public boolean preHandle(HttpServletRequest reqeust, HttpServletResponse response, Object handler) throws Exception {
		UserVo user = (UserVo)reqesut.getSession().getAtrribute("user");
		if(user == null) {
			response.sendRedirect("/login");
			return false;
		}

		return true;
	}
}
```

**WebConfig.java**
```java
public class WebConfig extends WebMvcConfigurerAdapter {
	@Override
	public void addInterceptors(InterceptorRegistry registry) {
		registry.addInterceptor(new LoginInterceptor()) //여기까지 작성시 모든 ruequest에서 동작
					.excludePathPatterns("/favicon.ico") //해당 패턴의 Url은 제외하고 작동
					.addPathPatterns("/test/**"); //특정 패턴만 적용
	}
}
```