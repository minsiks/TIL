# 23/6/21 Spring Security Day 02

 ## 스프링 시큐리티

> [스프링부트 시큐리티 & JWT 강의 | 학습 페이지 (inflearn.com)](https://www.inflearn.com/course/lecture?courseSlug=스프링부트-시큐리티&unitId=97760&tab=curriculum)

## Spring Security

### Config 용어정리

- CSRF - Cross site Request forgery
  - 사이트간 요청 위조
- `.sessionManagement().sessionCreationPolicy(SessionCreationPolicy.STATELESS) `
  - 세션 사용 X
- `.formLogin().disable() `
  - form태그 로그인 안함`
- `httpBasic().disable()`
  - 기본인증방식 안씀

## JWT

- Json Web Token

### JWT의 구조 이해

- JWT : 당사자간에 정보를 JSON객체로 안전하게 전송하기 위한 컴팩트하고 독립적인 방식을 정의하는 개방형 표준

- JWT의 구조

  - Header : 어떤 알고리즘을 사용해서 서명 내용

    ```javascript
    {
        "alg" : "HS256",
        "typ" : "JWT"
    }
    ```

  - Payload (정보)

    - 클레임
      - 등록 된 클레임 : 필수 X 권장되는 미리 정의 된 클레임
      - 개인 클레임 : userId 등등, 프라이머리키

    ```javascript
    {
        "sub" : "1234567890",
        "name" : "john",
        "id" : "kss"
    }
    ```

  - Signature (서명)

    - HMAC SHA256으로 암호

### jwt Bearer 인증 방식

- 기존 Http Basic방식은 ID,PW를 직접 전송
- Bearer는 토큰을 전송
  - 노출이 되도 안전
  - 유효시간이 존재 (ex_10분 후 삭제)

> JwtProperties를 만들어 주면 좋음
>
> ```java
> public interface JwtProperties {
>     String SECRET = "cos"; // 우리 서버만 알고 있는 비밀값
>     int EXPIRATION_TIME = 60000*10; // 10분
>     String TOKEN_PREFIX = "Bearer ";
>     String HEADER_STRING = "Authorization";
> }
> ```
>
> 
