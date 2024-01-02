# What is JWT??
모바일이나 웹의 사용자 인증을 위해 암호화된 토큰을 의미한다.
하지만, 우리는 이미 이러한 기능을 하는 녀석들을 알고 있다.
**cookie**와 **session**이다.

**cookie**와 **session**은 각각 장단점이 존다한다.

**cookie**의 가장 큰 단점은 바로 보안성에 있다.
**JavaScript**를 통해 브라우저에 저장되므로 손쉽게 조작이 가능하다는 것이다.

반면, **session**의 경우 해당 정보를 브라우저가 아닌 서버(메모리, DB 등)에 저장하기 때문에 조작이 어렵다.
하지만, 해당 정보가 계속 저장되고 쌓이기 때문에 서버의 성능에 의존적이라고 할 수 있다.

**JWT**는 일종의 토큰방식 텍스트인데 **dot**(.)을 기준으로 총 3가지의 데이터를 담는다.
구조는 다음과 같다.

### 1. HEADER
헤더에는 토큰의 타입과 해시 암호화 알고리즘에 대한 정보가 담겨있다.
```JavaScript
const header = {
	alg: 'sha256',
	typ: 'jwt'
};
```

### 2. payload
페이로드에는 로그인된 사용자의 단편적인 정보를 저장한다.
단편적인 정보를 담는 이유는 **payload**부분을 전달할 때 암호화하여 전달하는 것이 아닌 16진수로 변환하여 전달하는 방식을 채택하기 때문이다. 따라서, 정보 유출과 조작이 간편하다.
이러한 이유로 보안을 위해 예민한 정보들은 담아두지 않는 것이 좋다.
```JavaScript
const payload = {
	userId: userVo.userId,
	userAlias: userVo.userAlias
};
```

### 3. signature
시그니처는 위의 두가지(**HEADER**와 **payload**) 데이터와 서버개발자가 지정한 양념(?)을 잘 조잡하여 hash화한 값이다.
서버개발자가 지정한 양념은 두 데이터가 조작이 되었는지 안되었는지 확인 할 수 있는 매우 중요한 수단으로 사용되는데, 유출의 위험을 방지하기 위해 환경변수로 선언한다.

# JWT구현 예시
### 1. 서버에서 구현
Maven에 라이브러리를 추가한다.
```xml
<dependency>
	<groupId>io.jsonwebtoken</groupId>
	<artifactId>jjwt-api</artifactId>
	<version>0.11.2></version>
</dependency>
<dependency>
	<groupId>io.jsonwebtoken</groupId>
	<artifactId>jjwt-impl</artifactId>
	<version>0.11.2></version>
</dependency>
<dependency>
	<groupId>io.jsonwebtoken</groupId>
	<artifactId>jjwt-jackson</artifactId>
	<version>0.11.2></version>
</dependency>
```

class를 생성한다.
```Java
import io.jsonwebtoken.Jwts;
import io.jsonwebtoken.SignatureAlgorithm;
import javax.crypto.SecretKey;
import java.nio.charset.StandardCharsets;
import java.util.Date;

public class CreateJwt {
	//JWT secretKey
	private static final String SECRET_KEY = "${유출되면 안되는 비밀스러운 키!!!!}";

	public static void main(String args[]) {
		//JWT 토큰 생성
		String token = createJwtToken("user@gmail.com");
		System.out.println("JWT Token: " + token);

		//생성된 JWT 토큰 검증
		boolean isValid = validateJwtToken(token);
		System.out.println("JWT Token Validation: " + isValid);
	}

	//JWT 토큰 생성
	public static String createJwtToken(String subject) {
		SecretKey secretKey = Keys.hmacShaKeyFor(SECRET_KEY.getBytes(StandardCharsets.UTF_8));

		String token = Jwts.builder()
						.setSubject(subject)
						.setIssueAt(new Date())
						.setExpiration(new Date(System.cruuentTimeMillis() + 3600000)) //1시간
						.signWith(secretKey, SignatureAlgorithm.SHA256)
						.compact();
		return token;
	}

	//JWT 토큰 검증
	public static boolean validateJwtToken(String token) {
		SecretKey secretKey = Keys.hmacShaKeyFor(SECRET_KEY.getBytes(StandardCharsets.UTF_8));

		try {
			Jwts.parserBuilder()
				.setSigningKey(secretKey)
				.build()
				.parseClaimsJws(token);
			return true; //서명이 유요한경우 true 리턴
		} catch(Exception e) {
			return false; //서명이 유효하지 않아 Exception 발생시 false 리턴
		}
	}
}
```

### 2. 프론트에서 구현
**Header**와 **payload**를 만든다.
```JavaScript
const header = {
	alg: 'sha256',
	typ: 'jwt'
};

const payload = {
	userId: 'ABCD',
	userAlias: 'NYou'
};
```

위에서 만든 **Header**와 **payload**를 64진수로 변환한다.
빈값을 뜻하는 `=`를 제거해 준다.

```JavaScript
const encodingHeader = Buffer.from(JSON.stringify(header))
							.toString('base64')
							.replace(\=\, '');

const encodingPayload = Buffer.from(JSON.stringify(payload))
							.toString('base64')
							.replace(\=\g, '');
```

그 이후 인코딩된 header와 payload를 데리고 salt를 이용하여 hash화 해준다.
다음은 직접 hash화 하지 않고, crypto라는 모듈을 이용한다.

```JavaScript
const signature = crypto.crateHmac('sha256', Buffer.from(`${양념}`))
						.update(`${encodingHeader},${encodingPayload}`)
						.digest('base64')
						.replace(\=\g, '');
```

양념은 맛있게 잘 버부릴수 있게 이것저것 마구 넣어 섞으면 된다.

마지막으로 만들어놓은 녀석들을 모두 합쳐 쿠키로 넣어주면 끝!!

```JavaScript
const jwt = `${encodingHeader}.${encodingPayload}.${signature}`;
```