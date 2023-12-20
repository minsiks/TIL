# 11/2 Spring Reviews Day 02

## Inflearn 스프링 입문
> 김영한

### 프로젝트 생성  

- gradle로 설정
	- gradle은 버전을 땡겨온다고 생각

- 스프링 부트는 톰캣을 내장하고 있어 자체적으로 같이 올라온다.

- 기본적으로 자동으로 땡겨지는 로깅
	- 로그백
	- sif4j

- 스프링 라이브러리 살펴보기
	- Spring-boot-starter-web
		- 톰캣
		- 스프링 웹 MVC
	- Spring-boot-starter-thymeleaf
	- Spring-boot-starter(공통)
		- Spring boot
		- Spring boot starter logging

*테스트 라이브러리*
	- spring-boot-starter-test
		- junit : 테스트 프레임워크
		- mockito : 목 라이브러리
		- assertj : 테스트 코드를 좀 더 편하게 작성하게 도와주는 라이브러리
		- spring-test : 스프링 통합 테스트 지원

### View 환경설정

#### Welcome page 만들기

```<!DOCTYPE HTML>
<html>
<head>
 <title>Hello</title>
 <meta http-equiv="Content-Type" content="text/html; charset=UTF-8" />
</head>
<body>
Hello
<a href="/hello">hello</a>
</body>
</html>
```

#### thymeleaf 템플릿 엔진

- Controller 단
``` 
package hello.hellospring.controller;

import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.GetMapping;

@Controller
public class HelloController {

    @GetMapping("hello")
    public String hello(Model model){
        model.addAttribute("data","hello!!");
        return "hello";
    }
}
```

- hello.html

```
<!DOCTYPE HTML>
<html xmlns:th="http://www.thymeleaf.org">
<head>
    <title>static content</title>
    <meta http-equiv="Content-Type" content="text/html; charset=UTF-8" />
</head>
<body>
<p th:text="'안녕하세요.'+${data}">안녕하세요. 손님</p>
</body>
</html>
```

	- 컨트롤러에서 리턴 값으로 문자를 반환하면 뷰 리졸버(`viewResolver`)가 화면을 찾아서 처리한다.

### 빌드하고 실행하기

- window cmd로 빌드하고 실행하기

https://shoney.tistory.com/entry/Spring-gradle-%EB%B9%8C%EB%93%9C-jar-%EC%8B%A4%ED%96%89%ED%95%98%EB%8A%94-%EB%B0%A9%EB%B2%95

### 정적 컨텐츠 (Static Content)

- 파일을 웹브라우저에 그대로 가져다 놓는 컨텐츠

![image-20221102162553904](C:\Users\김민식\Documents\TIL\assets\image-20221102162553904.png)

### MVC와 템플릿 엔진

- MVC : Model, View, Controller
  - Controller

```java
package hello.hellospring.controller;

import org.springframework.stereotype.Controller;
import org.springframework.stereotype.Repository;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RequestParam;

@Controller
public class HelloController {

    @GetMapping("hello")
    public String hello(Model model){
        model.addAttribute("data","hello!!");
        return "hello";
    }

    @GetMapping("hello-mvc")
    public String helloMvc(@RequestParam("name") String name, Model model){
        model.addAttribute("name", name);
        return "hello-template";
    }
}
```

hello-template.html

```html
<html xmlns:th="http://www.thymeleaf.org">
<body>
<p th:text="'안녕하세요.'+${name}">안녕하세요. 손님</p>
</body>
</html>
```

### API

- HelloController.java

```java
@GetMapping("hello-string")
    @ResponseBody
    public String helloString(@RequestParam("name") String name){
        return "hello" + name; //"hello spring"
    }
    @GetMapping("hello-api")
    @ResponseBody
    public Hello helloApi(@RequestParam("name") String name){
        Hello hello = new Hello();
        hello.setName(name);
        return hello;
    }

    static class Hello{
        private String name;

        public String getName() {
            return name;
        }

        public void setName(String name) {
            this.name = name;
        }
    }
```

- @ResponseBody 사용 원리
  - 객체를 JSON으로 바꿔 그대로 웹 브라우저에 응답
  - HTTP의 BODY에 문자 내용을 직접 반환
  - `viewResolver` 대신에 `HttpMessageConverter` 가 동작
  - 기본 문자처리 : `StringHttpMessageConverter`
  - 기본 객체처리 : `MappingJackson2HttpMessageConverter`
  - byte 처리 등등 기타 여러 HttpMessageConverter가 기본으로 등록되어 있음

### 비즈니스 요구사항 정리

- 데이터 : 회원ID, 이름
- 기능 : 회원 등록, 조회
- 아직 데이터 저장소가 선정되지 않음(가상의 시나리오)

#### 일반적인 웹 애플리케이션 계층 구조

- 컨트롤러 : 웹 MVC의 컨트롤러 역할
- 서비스 : 핵심 비즈니스 로직 구현
- 리포지토리 : 데이터베이스에 접근, 도메인 객체를 DB에 저장하고 관리
- 도메인 : 비즈니스 도메인 객체, 예)회원, 주문 등등 주로 데이터베이스에 저장하고 관리됨



? RequestParam을 사용하지 않고 Thymeleaf의 `th:href="@{/movielist/detail(id=${m.id})}"` 와 같이 처리할 때의 차이점