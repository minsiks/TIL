# 11/3 Spring Reviews Day 03

## Inflearn 스프링 입문

> 김영한

### 회원 도메인과 리포지토리 만들기

#### Stream API

- 자바8부터 람다식과 함께 제공된 함수형 프로그래밍을 위한 기능으로, 컬렉션, 배열안에 있는 요소들을 하나씩 참조하며 반복적인 처리를 할 수 있는 것

** Stream쓰고 안쓰고 비교를 위해 리스트 정렬과 출력처리를 해보기.*

```
String[] fruits = {apple, grape, orange};

//Stream 쓰지 않을 때
Arrays.sort(fruits); //원본 그대로의 데이터 정렬
//출력
for(String x: fruits) {
	System.out.println(x);
    }
    
//Stream을 썼을 때
Stream<String> fruits_stream = Arrays.stream(fruits); //Stream 객체 생성
//정렬 후 출력
fruits_stream.sorted().forEach(System.out::println);
```

\1. Stream을 쓰지 않을 때

**- 원본의 데이터가 변경된다.**

**- for문의 사용으로 코드가 길어진다.**

 

\2. Stream을 쓸 때

**- 원본 데이터의 변경이 없다.**

**- 일시적이며 재사용이 불가하다.**

**- forEach문에 반복문이 들어가 있기에 코드가 간결하다.**

#### 람다식이란?

- 프로그래밍 언어에서 사용되는 개념으로 익명 함수를 지칭하는 용어

```java
public String SayHello(){
    return "안녕하세요?";
}
```

-> 람다식 예시)

`()->"안녕하세요?"

- 메소드에 대한 정의없이도 바로 간결하게 사용할 수 있도록 해주는 것이 바로 람다식
- 함수에 대한 이름이 없기에 익명 함수라 일컫음
- 장점
  - 코드가 간결
  - 함수를 만드는 과정이 생략되기에 생산성이 올라감
  - 병렬 프로그램에 사용될 수 있다
- 단점
  - 재사용이 불가능
  - 남용을 하게 될 경우 오히려 코드가 더욱 복잡해진다.
  - 디버깅이 어렵다

#### [:: 메소드 참조]

- 람다표현식에서 불필요한 매개변수를 제거

```java
()->System.out.println(); // 람다식 출력
System.out::println; // 참조형 메서드 출력
```

