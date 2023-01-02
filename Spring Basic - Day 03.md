# 23/1/2 Spring Basic Day03

## Inflearn 스프링 입문

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

## 싱글톤 컨테이너

### 싱글톤 컨테이너

- 스프링 컨테이너는 싱글톤 패턴의 문제를 해결하면서, 객체 인스턴스를 싱글톤(1개만 생성)으로 관리
- 스프링 컨테이너는 싱글턴 패턴을 적용하지 않아도, 객체 인스턴스를 싱글톤으로 관리
- 스프링 컨테이너는 싱글톤 컨테이너 역할, 싱글톤 객체를 생성하고 관리하는 기능을 싱글톤 레지스트리
- 스프링 컨테이너는 싱글턴 패턴의 모든 장점을 해결하면서 객체를 싱글톤으로 유지 가능
  - 싱글톤 패턴을 위한 지저분한 코드가 들어가지 않아도 된다.
  - DIP, OCP, 테스트, private 생성자로 부터 자유롭게 싱글톤을 사용 가능

#### 싱글톤 컨테이너 적용 후 

- 스프링 컨테이너 덕분에 고객의 요청이 올 때 마다 객체를 생성하는 것이 아니라, 이미 만들어진 객체를 공유해서 효율적으로 재사용 가능

### 싱글톤 방식의 주의점

- 싱글톤 패턴 또는 스프링 같은 싱글톤 컨테이너를 사용하던, 객체 인스턴스를 하나만 생성해서 공유하는 싱글톤 방식은 여러 클라이언트가 하나의 같은 객체 인스턴스를 공유하기 때문에 싱글톤 객체는 상태를 유지하게 설계하면 안됨
- 무상태로 설계
  - 특정 클라이언트에 의존적인 필드가 있으면 안됨
  - 특정 클라이언트가 값을 변경할 수 있는 필드가 있으면 안된다.
  - 가급적 읽기만 가능
  - 필드 대신에 자바에서 공유되지 않는, 지역변수, 파라미터, ThreadLocal 등을 사용해야 한다.
- 스프링 빈의 필드에 공유 값을 설정하면 정말 큰 장애가 발생 할 수 있음

```java
package hello.core.singleton;

import org.junit.jupiter.api.Assertions;
import org.junit.jupiter.api.Test;
import org.springframework.context.ApplicationContext;
import org.springframework.context.annotation.AnnotationConfigApplicationContext;
import org.springframework.context.annotation.Bean;

import static org.assertj.core.api.Assertions.assertThat;
import static org.junit.jupiter.api.Assertions.*;

class StatefulServiceTest {

    @Test
    void statefulServiceSingleton(){
        ApplicationContext ac = new AnnotationConfigApplicationContext(TestConfig.class);
        StatefulService statefulService1 = ac.getBean(StatefulService.class);
        StatefulService statefulService2 = ac.getBean(StatefulService.class);

        //ThreadA : A사용자 10000원 주문
        statefulService1.order("userA",10000);
        //ThreadB : B사용자 20000원 주문
        statefulService2.order("userB",20000);

        //ThreadA : 사용자A 주문 금액 조회
        int price = statefulService1.getPrice();
        System.out.println("price = " + price);

        assertThat(statefulService1.getPrice()).isEqualTo(20000);

    }

    static class TestConfig{
        @Bean
        public StatefulService statefulService(){
            return new StatefulService();
        }
    }
}
```

- `StateFulService`의 `price` 필드는 공유되는 필드인데, 특정 클라이언트가 값을 변경
- 사용자 A의 주문금액은 10000원이 되어야 하는데, 20000원이라는 결과가 나옴
- 공유필드는 조심, 스프링 빈은 항상 무상태(stateless)로 설계

### @Configuration과 싱글톤

```java
package hello.core;

import hello.core.discount.DiscountPolicy;
import hello.core.discount.FixDiscountPolicy;
import hello.core.discount.RateDiscountPolicy;
import hello.core.member.MemberService;
import hello.core.member.MemberServiceImpl;
import hello.core.member.MemoryMemberRepository;
import hello.core.order.OrderService;
import hello.core.order.OrderServiceImpl;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

@Configuration
public class AppConfig {

    @Bean
    public MemberService memberService(){
        return new MemberServiceImpl(memberRepository());
    }
    @Bean
    public MemoryMemberRepository memberRepository() {
        return new MemoryMemberRepository();
    }
    @Bean
    public OrderService orderService(){
        return new OrderServiceImpl(memberRepository(), discountPolicy());
    }
    @Bean
    public DiscountPolicy discountPolicy(){
        return new RateDiscountPolicy();
    }

}
```

- memberServie -> memberRepository() -> new MemoryMemberRepository()
- orderService -> memberRepository() -> new MemoryMemberRepository()
  - 결과적으로 각각 다른 2개의 MemoryMemberRepository가 생성되면서 싱글톤이 깨지는 것처럼 보임

```java
package hello.core.singleton;

import hello.core.AppConfig;
import hello.core.member.MemberRepository;
import hello.core.member.MemberService;
import hello.core.member.MemberServiceImpl;
import hello.core.order.OrderService;
import hello.core.order.OrderServiceImpl;
import org.assertj.core.api.Assertions;
import org.junit.jupiter.api.Test;
import org.springframework.context.ApplicationContext;
import org.springframework.context.annotation.AnnotationConfigApplicationContext;

public class ConfigurationSingletonTest {

    @Test
    void configuration(){
        ApplicationContext ac = new AnnotationConfigApplicationContext(AppConfig.class);

        MemberServiceImpl memberService = ac.getBean("memberService",MemberServiceImpl.class);
        OrderServiceImpl orderService = ac.getBean("orderService", OrderServiceImpl.class);
        MemberRepository memberRepository = ac.getBean("memberRepository", MemberRepository.class);

        MemberRepository memberRepository1 = memberService.getMemberRepository();
        MemberRepository memberRepository2 = orderService.getMemberRepository();

        System.out.println("memberService -> memberRepository1 = " + memberRepository1);
        System.out.println("memberService -> memberRepository2 = " + memberRepository2);
        System.out.println("memberRepository = " + memberRepository);

        Assertions.assertThat(memberService.getMemberRepository()).isSameAs(memberRepository);
        Assertions.assertThat(orderService.getMemberRepository()).isSameAs(memberRepository);
    }
}
```

- 확인해보면 memberRepository 인스턴스는 모두 같은 인스턴스가 공유되어 사용

### @Configuration과 바이트코드 조작

- 스프링 컨테이너는 싱글톤 레지스트리. 그래서 스프링은 바이트코드를 조작하는 라이브러리를 사용
  - `@Configuration`을 적용한 `AppConfig`에 답이 있다.

```java
    @Test
    void configurationDeep(){
        ApplicationContext ac = new AnnotationConfigApplicationContext(AppConfig.class);
        AppConfig bean = ac.getBean(AppConfig.class);

        System.out.println("bean = " + bean.getClass());
    }
```

- 직접 만든 클래스가 아니라 스프링이 CGLIB라는 바이트코드 조작 라이브러리를 사용해서 AppConfig 클래스를 상속받은 임의의 다른 클래스를 만들고, 그 다른 클래스를 스프링 빈으로 등록
- @Bean이 붙은 메서드마다 이미 스프링 빈이 존재하면 존재하는 빈을 반환, 스프링 빈이 없으면 생성해서 스프링 빈으로 등록하고 반환하는 코드가 동적으로 생성
- 덕북에 싱글톤이 보장되는 것

#### @Configuration을 적용하지 않고 @Bean만 적용한다면?

- 순수한 AppConfig로 스프링 빈에 등록
- MemberRepository가 총 3번 호출

#### 정리

- @Bean만 사용해도 스프링 빈으로 등록되지만, 싱글톤을 보장하지 않는다.
  - memberRepository()처럼 의존관계 주입이 필요해서 메서드를 직접호출 할 때 싱글톤을 보장하지 않는다.
- 크게 고민할 것 없이 스프링 설정 정보는 항상 @Configuration을 사용

## 컴포넌트 스캔

### 컴포넌트 스캔과 의존관계 자동 주입 시작하기

- 지금까지 스프링 빈을 등록할 때는 자바 코드의 @Bean이나 XML <bean> 등을 통해서 설정 정보에 직접 등록할 스프링 빈을 나열
- 스프링 빈이 수십, 수백개가 되면 일일이 등록하기 어렵다.
- 그래서 스프링은 설정 정보가 없어도 자동으로 스프링 빈을 등록하는 컴포넌트 스캔이라는 기능을 제공
- 또 의존관계도 자동으로 주입하는 `@Autowired`라는 기능도 제공

```java
package hello.core;

import org.springframework.context.annotation.ComponentScan;
import org.springframework.context.annotation.Configuration;
import org.springframework.context.annotation.FilterType;

@Configuration
@ComponentScan(
        excludeFilters = @ComponentScan.Filter(type = FilterType.ANNOTATION, classes = Configuration.class) // @Configuration이 붙은 클래스는 제외하는 코드
)
public class AutoAppConfig {

}
```

- 컴포넌트 스캔을 사용하려면 먼저 @ComponentScan 을 설정 정보에 붙여주면 된다.
- 기존의 AppConfig와는 다르게 @Bean으로 등록할 클래스가 하나도 없다.

```java
package hello.core.scan;

import hello.core.AutoAppConfig;
import hello.core.member.MemberService;
import org.assertj.core.api.Assertions;
import org.junit.jupiter.api.Test;
import org.springframework.context.annotation.AnnotationConfigApplicationContext;

public class AutoAppConfigTest {

    @Test
    void basicScan(){
        AnnotationConfigApplicationContext ac = new AnnotationConfigApplicationContext(AutoAppConfig.class);

        MemberService bean = ac.getBean(MemberService.class);
        Assertions.assertThat(bean).isInstanceOf(MemberService.class);
    }
}
```

- `AnnotationConfigApplicationContext`를 사용하는 것은 기존과 동일
- 설정 정보로 `AutoAppConfig` 클래스를 넘겨준다.
- 실행해보면 기존과 동일

- `ComponentScan`은 `@Component`가 붙은 모든 클래스를 스프링 빈으로 등록
- 스프링 빈의 기본 이름은 클래스명을 사용하되 맨 앞글자만 소문자를 사용
  - 빈 이름 기본 전략 : MemberServiceImpl 클래스 -> memberServiceImpl
  - 빈 이름 직접 지정 : 만약 스프링 빈의 이름을 직접 지정하고 싶으면 `@Component("memberService2")` 이런식으로 지정
- @Autowired 의존관계 자동 주입
  - 생성자에 `@Autowired`를 지정하면 스프링 컨테이너가 자동으로 해당 스프링 빈을 찾아서 주입한다.
  - 이때 기본 조회 전략은 타입이 같은 빈을 찾아서 주입한다.
    - `getBean(MemberRepository.class)` 와 동일하다고 이해하면 된다.
  - 생성자에 파라미터가 많아도 다 찾아서 주입
