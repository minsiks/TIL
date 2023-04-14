# 23-3-6 Standard of Java(Object oriented)  Day 01

## 객체지향 개념

> [자바의 정석](https://www.youtube.com/watch?v=M_4a4tUCSPU&list=PLW2UjW795-f6xWA2_MUhEVgPauhGl3xIp&index=169)

### 하나의 소스파일에 여러 클래스 작성

```java
public class Hello2 {}
	   class Hello3 {}
```

- pulic class가 있는 경우, 소스 파일의 이름은 반드시 public class의 이름과 일치해야한다.

```java
class Hello2{}
class Hello3{}
```

- public class가 하나도 없는 경우, 소스파일의 이름은 'Hello2.java','Hello3.java' 둘 다 가능

### static 메서드와 인스턴스 메서드

```java
class MyMath2{
    long a, b;
    
    long add(){ // 인스턴스 메서드
        return a+b;
    }
    static long add(long a, long b){ //클래스 메서드(static메서드)
    	return a+b;
    }
}
```

- 인스턴스 메서드
  - 인스턴스 생성 후, '참조변수.메서드이름()'으로 호출
- static 메서드 (클래스메서드)
  - 객체생성없이 '클래스이름.메서드이름()'으로 호출

https://lky1.tistory.com/8
