# 23-2-22 Standard of Java(Lamda-Stream)  Day02

## [자바의 정석](https://www.youtube.com/watch?v=7Kyf4mMjbTQ)

> 남궁성

## 메서드 참조(method reference)

- 하나의 메서드만 호출하는 람다식은 '메서드 참조'로 간단히 할 수 있다.

![methodR](C:\Users\김민식\Pictures\methodR.png)

- static 메서드 참조

```java
Integer method(String s)}{ // 그저 Integer.parseInt(String s)만 호출
    return Integer.parseInt(s);
}
=>
Function<String, Integer> f = (String s) -> Integer.parseInt(s);
=>
Function<String, Integer> f = Integer::parseInt // 메서드 참조
```

### 생성자의 메서드 참조

- 생성자와 메서드 참조

```java
Supplier<MyClass> s = () -> new MyClass();
=>
Suppplier<MyClass> s = MyClass::new;

Function<Integer,MyClass> s = (i) -> new MyClass(i);
=>
Function<Integer,MyClass> s = MyClass::new;
```

### 배열과 메서드 참조

```java
Fucntion<Integer, int[]>f = x -> new int[x]; // 람다식
Function<Integer, int[]>f2 = int[]::new; // 메서드 참조
```

## 스트림

- 다양한 데이터 소스를 표준화된 방법으로 다루기 위한 것

```java
List<Integer> list = Arrays.asList(1,2,3,4,5);
Stream<Integer> intStream = list.stream(); // 컬렉션.
Stream<String> strStream = Stream.of(new String[]{"a","b","c"}); // 배열
Stream<Integer> evenStream = Stream.iterate(0, n->n+2); // 0,2,4,6 ...
Stream<Double> randomStream = Stream.generate(Math::random); //람다식
IntStream intStream = new Random().ints(5); //난수 스트림(크기가 5)
```

- 스트림이 제공하는  기능 - 중간 연산과 최종 연산
  - 중간연산 - 연산결과가 스트림인 연산. 반복적으로 적용가능
  - 최종 연산 - 연산결과가 스트림이 아닌 연산. 단 한번만 적용가능 (스트림의 요소를 소모)

- stream.distinct().limit(5).sorted().formEach(System.out::println)

```java
String[] strArr = {"dd","aaa","CC","cc","b"};
Stream<String> stream = Stream.of(strArr); // 문자열 배열이 소스인 스트림
Stream<String> filteredStream = stream.filter();
Stream<
```

