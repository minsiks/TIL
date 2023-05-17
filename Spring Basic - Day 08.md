# 22/5/16 Spring Basic Day08

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

## 의존관계 자동 주입

### 생성자 주입

- 생성자 호출시점에 딱 1번만 호출
- 불변, 필수 의존관계에 사용
- 생성자가 딱 1개만 있다면 @Autowired 생략 가능

### 수정자 주입(setter)

- 선택, 변경 가능성이 있는 의존관계에 사용

### 필드 주입

- 코드가 간결, 하지만 외부에서 변경이 불가
- 사용하지 말자

### 일반 메서드 주입

- 한번에 여러 필드를 주입 받을 수 있다
- 일반적으로 잘 사용 x

## 옵션 처리

```java
 @Autowired(required = false)
public void setNoBean1(Member noBean1){
    System.out.println("noBean1 = " + noBean1);
}
@Autowired
public void setNoBean2(@Nullable Member noBean2){
    System.out.println("noBean2 = " + noBean2);
}

@Autowired
public void setNoBean3(Optional<Member> noBean3){
    System.out.println("noBean3 = " + noBean3);
}
```

## 생성자 주입 선택 이유

- 불변
  - 의존관계 주입은 한번 일어나면 애플리케이션 종료시점까지 변경할 일이 없다. 오히려 종료전까지 변하면 안된다.
- 누락
- final 키워드

## 롬복과 최신 트렌드

- @RequiredArgsConstructor
  - final 키워드만 있으면 알아서 생성자로 의존성 주입

## 빈이 두개이상 등록일 때 해결

### @Autowired 필드 명 매칭

- `@Autowired` 여러 빈이 있으면 필드 이름(파라미터 이름)으로 빈 이름을 추가 매칭한다.

### @Quilifier

- 추가 구분자를 붙여주는 방법

### @Primary

- 우선순위를 정함 ,@Primary 가 우선권을 정함

## 자동, 수동의 올바른 실무 운영 기준

- 편리한 자동 기능을 기본으로 사용

## 빈 생명주기 콜백

- 이벤트 라이프사이클
  - 스프링 컨테이너 생성 -> 스프링 빈 생성 -> 의존관계 주입 -> 초기화 콜백 -> 사용 -> 소멸전 콜백 -> 스프링 종료

- InitializingBean, DisposableBean
  - 초기화, 소멸 인터페이스

### 빈등록 초기화,소멸 메서드

- ```
  @Bean(initMethod = "init", destroyMethod = "close")
  ```

  - 메서드 이름을 자유롭게 줄 수 있다.

### 애노테이션 @PostConstruct, @PreDestory

- 가장 권장하는 방법

## 빈 스코프

- 싱글톤 : 기본 스코프, 시작과 종료까지 유지 넓은 범위 스코프
- 프로토타입 : 스프링 컨테이너는 프로토타입 빈의 생성과 의존관계 주입까지만 관여, 짧은 범위의 스코

### 프로토타입

- 항상 새로운 프로토타입 빈을 생성해서 반환
- 종료 메서드가 호출되지 않는다.

### Singleton과 Prototype 같이 사용시 문제 해결

- Provider로 해결
  - ObjectFactory : 기능이 단순, 별도의 라이브러리 필요 없음
  - ObjectProvider : ObjectFactory 상속, 스트림 처리등 편의 기능 다수
  - JSR-330 Provider
    - 실행해보면 항상 새로운 프로토타입 빈이 생성
    - provider.get() 메서드 하나로 기능이 단순
    - 별도의 라이브러리 필요

### 웹 스코프

- 프로토타입과 다르게 스프링이 해당 스코프의 종료시점까지 관리
- request : HTTP 요청 하나가 들어오고 나갈 때 까지 유지되는 스코프, 각각의 HTTP 요청마다 별도의 빈 인스턴스가 생성되고, 관리
- session : HTTP Session과 동일한 생명주기를 가지는 스코프
- application : 서블릿 컨텍스트와 동일한 생명주기를 가지는 스코프
- websocket : 웹소켓과 동일한 생명주기를 가지는 스코프

