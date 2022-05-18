# 쿠키, 세션, 토큰
- [인증이 왜 필요한가?](#인증이-왜-필요한가)
- [인증 방식](#인증-방식)
- [쿠키](#쿠키)
- [세션](#세션)
- [토큰 기반 인증 (JWT)](#토큰-기반-인증-jwt)
- [참고](#참고)
</br>

# 인증이 왜 필요한가?

- HTTP 프로토콜은 Connectionless하고 Stateless하다.
- 즉, 서버가 클라이언트를 식별할 수 없다. → 매 번 요청할 때마다 서버는 새로운 사용자로 인식하기 때문에 인증 절차에 문제가 생긴다.
</br>

# 인증 방식

- HTTP 기본 인증
    - Base64로 데이터를 인코딩하여 Header에 담는 방법이다. 사용자의 정보를 Header에 담아서 전송하므로 보안에 취약하다.
- 서버 기반 인증
    - 서버의 세션을 이용한 방법이다. 쿠키의 단점을 보완해줄 수 있다.
- 토큰 기반 인증
    - 필요한 정보가 담겨있는 토큰을 활용하는 방법이다.
</br>

# 쿠키

- stateless라는 client server 구조의 문제를 해결하기 위해서 탄생한 것으로 서버를 통해 클라이언트의 브라우저에 저장된다.
- key value 형식의 문자열이다.
</br>

### 쿠키 속성

- HttpOnly
    - javascript에서 cookie를 조작할 수 없게 한다.
    - 자바스크립트로 쿠키를 조작할 수 있으면 XSS 공격에 대해 더욱 취약해진다.
- Secure
    - 해당 쿠키는 HTTPS 통신에서만 접근할 수 있다.
- SameSite
    - 사이트 외부에서 해당 쿠키에 접근하지 못하도록 막는다.
    - CSRF 공격을 막기 위해 사용된다. (**공격자가** 해당 사이트 외부에서 사용자가 **원치 않는 요청을 유도**할 때 쿠키의 접근을 **막는다**.)
</br>

### 쿠키 종류

- Session Cookie
    - expires 속성이 없어서 브라우저 종료시 삭제되는 쿠키
- Persistent Cookie
    - expires 속성이 있어서 장기간 유지되는 쿠키
- Secure Cookie
    - HTTPS에서만 사용되는 쿠키
    - HTTPS에서는 쿠키 정보도 암호화되어 전송되므로 전송 도중에 빼앗길 걱정을 덜 수 있다.
- Third Party Cookie
    - 방문한 도메인과 다른 도메인의 쿠키
</br>

### 쿠키의 문제점

- 쿠키는 헤더에 포함되어 보내지기 때문에 트래픽을 더 발생시킨다.
- 쿠키는 데이터가 노출되어있어서 보안에 취약하다.
- 쿠키의 최대 크기는 4kb이고 개수는 300개까지만 가능하다.
- 브라우저마다 쿠키에 대한 지원 형태가 다르기 때문에 브라우저간 공유가 불가능하다.
</br>

# 세션

- 서버의 메모리에 생성되는 저장공간이다.
- 세션에 저장된 정보에는 고유의 세션ID가 부여된다.
- 이 세션 ID만 알고 있으면 메모리에서 데이터를 볼 수 있다.
- 서버는 쿠키에 이 세션ID를 실어서 보내준다.
- 쿠키와 다르게 서버에서 데이터를 저장해두니 데이터를 누구나 볼수는 없고 일정 시간이 지나거나 접근에 문제가 생겼을 때 세션을 삭제할 수 있으니 보안적으로는 더 우수하다.
</br>

### 세션의 문제점

- 서버 부하 문제
    - 세션은 서버의 메모리 내부에 저장이된다.
    - 유저가 많아지면 세션도 많아져서 메모리에 부하가 걸릴 수 있다.
- 확장성의 문제
    - 서비스의 규모가 커져서 서버를 여러대로 확장 및 분산하게 된다면 세션 정보도 분산하는 방법을 고려해야한다.
</br>

# 토큰 기반 인증 (JWT)

- 인증 받은 사용자들에게 토큰을 발급하고, 서버에 요청할 때마다 토큰을 보낸다.
- 서버가 직접 상태 정보를 유지하지 않아서 조금 더 stateless한 모습을 볼 수 있다.
</br>

### 토큰 기반 인증의 장점

- stateless
    - 토큰은 클라이언트 측에 저장되어서 서버를 stateless하게 유지한다.
    - 서버가 stateless하다는 것은 서버 확장성이 좋다는 뜻이다.
- CORS 문제 해결
    - 여러 플랫폼, 도메인에서 사용 가능하다.
</br>

### 토큰 기반 인증의 특징

- 토큰 자체는 유효기간이 끝나지 않으면 계속 사용이 가능하다.
    - 유효기간을 짧게 설정해서 해킹의 위험을 방지한다.
    - 유효기간이 짧아서 계속 만료하면 재 로그인해야하는 번거로움이 생겨서 refresh token을 따로 사용하기도 한다.
- 토큰은 Cookie 혹은 localstorage에 저장하는 방법이 있다.
    - local storage는 javascript에서 접근이 가능해서 XSS에 더 취약하다.
    - 그래서 보통 Cookie(HttpOnly)를 많이 사용한다.
</br>

### 토큰 기반 인증의 단점

- 토큰은 유효기간이 만료될 때까지 계속 사용 가능해서 탈취당했을 때 대처가 어렵다. (세션기반이라면 서버에서 세션을 없애면 됨)
- 토큰은 주로 JWT를 사용하는데, 토큰의 길이가 길어서 인증 요청이 많아질 수록 네트워크 부하가 심해진다.
</br>

### Refresh Token

- access token를 짧게 설정했을 때 재 로그인을 방지하기 위해 사용된다.
- refresh token은 검증을 위해 서버가 갖고 있는 경우가 많아서 강제로 만료시키는 방법이 가능하다.
    - 하지만 refresh token을 저장한다는 뜻은 I/O 작업이 필요 없는 JWT의 장점을 완벽히 누릴 수 없다는 뜻이다.
- access token으로 인증을 하는데 만료가 되면 refresh token으로 새 access token을 얻는다.
    - 이 때 access token을 재발급받을 때 refresh token을 변경하지 않으면 refresh token이 빼앗겼을 때 안전을 보장하지 못한다.
</br>

### 방어 예시

- 방어예시 1)
    - 공격자가 access token을 빼앗았다.
    - 곧 만료되어 더 이상 사용하지 못했다.
- 방어예시 2)
    - 공격자가 refresh token을 빼앗았다.
    - 기존 사용자가 refresh token으로 새로운 access token을 요청한다.
    - 그러면 서버는 기존 사용자에게 새로운 refresh token과 access token을 준다.
    - 공격자가 예전 refresh token로 요청을 하면 서버는 공격자와 유저를 구별하지 못하니 둘의 refresh token을 사용하지 못하게 한다.
- 방어예시 3)
    - 공격자가 refresh token을 빼앗고 새로운 refresh, access token을 요청한다.
    - 사용자가 예전 refresh token으로 새 access token을 요청하면 서버는 공격자와 사용자를 구별 못하니 둘 다 정지시킨다.
- 공격 예시 2, 3의 대처법은 이미 사용된 refresh token이 무엇인지 구별할 수 있어야한다.
- 방어예시 4)
    - access token과 refresh token을 db에 저장해둠
    - access token이 만료되어 refresh token으로 재발급이 필요할 때 둘 다 맞는지 db랑 확인해본다.
- **결론**
    - access token과 refresh token을 다 뺏기면 어쩔 도리가 없다.
    - 필요한 보안 수준과 서버 비용을 잘 고려해서 선택해야한다.
</br>

# 참고

[What Are Refresh Tokens and How to Use Them Securely](https://auth0.com/blog/refresh-tokens-what-are-they-and-when-to-use-them/)

[JWT 혹은 OAuth2 의 refresh 토큰을 어디다 저장해야 할까? | 두글 블로그](https://doogle.link/jwt-%ED%98%B9%EC%9D%80-oauth2-%EC%9D%98-refresh-%ED%86%A0%ED%81%B0%EC%9D%84-%EC%96%B4%EB%94%94%EB%8B%A4-%EC%A0%80%EC%9E%A5%ED%95%B4%EC%95%BC-%ED%95%A0%EA%B9%8C/)

[JWT에서 Refresh Token은 왜 필요한가?](https://velog.io/@park2348190/JWT%EC%97%90%EC%84%9C-Refresh-Token%EC%9D%80-%EC%99%9C-%ED%95%84%EC%9A%94%ED%95%9C%EA%B0%80)