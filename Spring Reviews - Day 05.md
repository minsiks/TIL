# 12/6 Spring Reviews Day 05

## Inflearn 스프링 입문

> 김영한

### Spring JAP 제공 클래스

- 인터페이스를 통한 기본적인 CRUD
- 그 외 특정한 컬럼이나 여러가지 컬럼을 조회할 때는 규칙을 가짐
  - `findByName()`, `findByEmail()` 처럼 메서드 이름만으로 조회 기능 제공
- 페이징 기능 자동 제공

```java
package hello.hellospring.repository;

import hello.hellospring.domain.Member;
import org.springframework.data.jpa.repository.JpaRepository;

import java.util.Optional;

public interface SpringDataJpaMemberRepository extends JpaRepository<Member, Long>, MemberRepository {

    //JPOL select m from Member m where m.name = ?
    @Override
    Optional<Member> findByName(String name);
    //여러개 컬럼 조회
    Optional<Member> findByAndName(String name,);
}

```

### AOP

- 문제 가정
  - 회원가입, 회원 조회등 각각 기능마다 시간을 측정하는 기능을 추가 하여라
    - 시간을 측정하는 기능은 핵심 관심 사항이 X
    - 시간을 측정하는 로직은 공통 관심 사항
    - 시간을 측정하는 로직과 핵심 비즈니스의 로직이 섞여서 유지보수가 어렵다.
    - 수정과 변경이 어려움

- AOP가 필요한 상황
  - 공통 관심 사항(cross-cutting concern) vs 핵심 관심 사항(core concern)

#### AOP 적용

- AOP : Aspect Oriented Programming
- 공통 관심 사항, 핵심 관심 사항 분리

```java
package hello.hellospring.aop;

import org.aspectj.lang.ProceedingJoinPoint;
import org.aspectj.lang.annotation.Around;
import org.aspectj.lang.annotation.Aspect;
import org.springframework.stereotype.Component;

@Aspect
@Component
public class TimeTraceAop {

    @Around("execution(* hello.hellospring..*(..))")
    public Object execute(ProceedingJoinPoint joinPoint) throws Throwable{
        long start = System.currentTimeMillis();
        System.out.println("START: " + joinPoint.toString());
        try{
            return joinPoint.proceed();
        }finally {
            long finish = System.currentTimeMillis();
            long timeMs = finish - start;
            System.out.println("END: "+ joinPoint.toString() + " " + timeMs + "ms");
        }
    }
}

```

- AOP를 따로 관리하려면 @Aspect를 입력 해줘야한다
- 또한 @Component나 SpringBean(config)에서 경로를 지정해야한다.
