# 22/5/17 Spring Basic Day09

## Inflearn 스프링 기본 재수강

> 김영한
>
> 강의 목차
>
> 1. 객체 지향 설계와 스프링
> 2. 스프링 핵심 원리 이해1 - 예제 만들기
> 3. 스프링 핵심 원리 이해2 - 객체 지향 원리 적용
> 4. 스프링 컨테이너와 스프링 빈
> 5. 싱글톤 컨테이너
> 6. 컴포넌트 스캔
> 7. 의존관계 자동 주입
> 8. 빈 생명주기 콜백
> 9. 빈 스코프

## request 스코프 예제

```java
package hello.core.web;

import hello.core.common.MyLogger;
import lombok.RequiredArgsConstructor;
import org.springframework.beans.factory.ObjectProvider;
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.ResponseBody;

import javax.servlet.http.HttpServletRequest;

@Controller
@RequiredArgsConstructor
public class LogDemoController {

    private final LogDemoService logDemoService;
    private final ObjectProvider<MyLogger> myLoggerProvider;

    @RequestMapping("log-demo")
    @ResponseBody
    public String logDemo(HttpServletRequest request) throws InterruptedException {
        String requestURL = request.getRequestURL().toString();
        MyLogger myLogger = myLoggerProvider.getObject();
        myLogger.setRequestURL(requestURL);

        myLogger.log("controller test");
        Thread.sleep(1000);
        logDemoService.logic("testId");
        return "OK";
    }
}
```

## 스코프와 프록시

- @Scope(value = "request", proxyMode = ScopedProxyMode.TARGET_CLASS)
  - 적용 대상이 인터페이스가 아닌 클래스면 `TARGET_CLASS`를 선택
  - 적용 대상이 인터페이스면 `INTERFACES`를 선택

- 가짜 프록시를 만들어두고 미리 주입
  - CGLIB라는 라이브러리

## 최종 정리

