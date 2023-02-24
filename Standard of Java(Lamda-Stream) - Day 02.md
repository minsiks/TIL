# 23-2-22 Standard of Java(Lamda-Stream)  Day 02

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
Stream<String> filteredStream = stream.filter(); // 걸러내기 (중간 연산)
Stream<String> distinctedStream = stream.distinct(); // 중복제거 (중간 연산)
Stream<String> sortedStream = stream.sort(); // 정렬 (중간 연산)
Stream<String> limitedStream = stream.limit(5); // 스트림 자르기(중간 연산)
int total = stream.count(); // 요소 개수 세기 (최종연산)
```

#### 스트림의 특징

- 스트림은 데이터 소스로부터 데이터를 읽기만 할 뿐 변경 X

```java
List<Integer> list = Arrays.asList(3,1,5,4,2);
List<Integer> sortedList = list.stream().sorted()
    .collect(Collectors.toList());
System.out.println(list);
System.out.println(SortedList);
```

- 스트림은 Iteratort처럼 일회용(필요하면 다시 스트림을 생성)

```java
strStrea.forEach(System.out::println); //모든 요소를 화면에 출력 (최종연산)
int numofStr = strStrea.count(); // 에러. 스트림이 이미 닫혔음.
```

- 최종 연산 전까지 중간연산이 수행되지 않는다. - 지연된 연산

```java
IntStream intStream = new Random().ints(1,46); // 1~45범위의 무한 스트림
intStream.distinct().limit(6).sorted()
    .forEach(i->System.out.print(i+","))
```

- 스트림은 작업을 내부 반복으로 처리

```java
for(String str : strLisit)
    System.out.println(str);
=>
stream.forEach(System.out::println);
```

- 스트림의 작업을 병렬로 처리 - 병렬스트림

```java
Stream<String> strSteam = Stream.of("dd","aaa","CC","cc","b");
int sum = strStream.parallel() // 병렬 스트림으로 처리
    .mapToInt(s -> s.length()).sum(); // 모든 문자열의 길이의 합
```

- 기본형 스트림 - IntStream, LongStream, DoubleStream
  - 오토박싱&언박싱의 비효율이 제거됨(Stream<integer> 대신 IntStream사용)
  - 숫자와 관련된 유용한 메서드를 Stream<T>보다 더 많이 제공

