# 23-2-22 Standard of Java(Lamda-Stream)  Day01

## [자바의 정석](https://www.youtube.com/watch?v=7Kyf4mMjbTQ)

> 남궁성

### 람다식(Lambda Expression)

- Java : oop언어 +  함수형 언어 (람다)를 추가 
- 유명한 함수형 언어 
  - Haskell
  - Erlang
  - Scala
  - 빅데이터와 같은것들때문에 함수형 언어가 각광

- 함수(메서드)를 간단한 '식(expression)'으로 표현

```java
int max(int a, int b){
    return a > b ? a : b;
}
=>
(a, b) -> a > b ? a: b
```

- 익명 함수(이름이 없는 함수, anonymous function)

```java
int max(int a, int b){
    return a > b ? a : b;
}
=>
(int a, int b)->{
    return a > b ? a : b;
}
```

- 함수와 메서드의 차이
  - 근본적으로 동일. 함수는 일반적 용어, 메서드는 객체지향개념 용어
  - 함수는 클래스에 독립적, 메서드는 클래스에 종속적

### 람다식 작성하기

- 메서드의 이름과 반한톼입을 제거하고 '->'를 블록{} 앞에 추가

```java
int max(int a, int b){
    return a > b ? a : b;
}
=>
(int a, int b)->{
    return a > b ? a : b;
}
```

- 반환값이 있는 경우, 식이나 값만 적고 return문 생략 가능(끝에';'안 붙임)

```java
(int a, int b) -> {
    return a > b ? a : b;
}
=>
(int a, int b) -> a > b ? a : b
```

- 매개변수의 타입이 추론 가능하면 생략가능(대부분의 경우 생략 가능)

```java
(int a, int b)-> a > b ? a : b
=>
(a, b) -> a > b ? a : b
```

#### 주의사항

1. 매개변수가 하나인 경우, 괄호() 생략가능(타입이 없을 때만)

```java
(a) -> a * a
(int a)-> a * a
=>
a -> a * a //OK
int a -> a * a // 에러
```

2. 블록 안의 문장이 하나뿐 일 때, 괄호{}생략가능(끝에 ';' 안 붙임)

```java
(int i)->{
    System.out.println(i);
}
(int i)-> System.out.println(i)
```

- 단, 하나뿐인 문장이 return문이면 괄호{} 생략 불가

```java
(int a, int b) -> {return a > b ? a : b; } //OK
(int a, int b) -> return a > b ? a : b //에러
```

#### 람다식 예

```java
	int max(int a, int b) {
		return a>b?a:b;
	}
	int printVar(String name, int i) {
		System.out.println(name+"="+i);
	}
	int square(int x) {
		return x * x;
	}
	//=>람다식
	(a,b) -> a > b ? a : b 
	
	(name,i)->System.out.println(name+"="+i);			
	
	x -> x*x
```

#### 람다식은 익명 객체

- 람다식은 익명 함수가 아니라 익명 객체이다.

```java
(a, b) -> a > b ? a : b
=>
new Object(){
    int max(int a, int b){
        return a > b ? a : b;
    }
}
```

- 람다식(익명 객체)을 다루기 위한 참조변수가 필요. 참조변수 타입은?

  ```java
  Object obj = new Object(){
      int max(int a, int b){
          return a > b ? a : b;
      }
  }
  ```

### 함수형 인터페이스

- 함수형 인터페이스 - 단 하나의 추상 메서드만 선언된 인터페이스

```java
interface MyFunction{
    public abstract int max(int a, int b);
}
```

#### example

- 익명 객체를 람다식으로 대체

```java
List<String> list = Arrays.asList("abc", "aaa", "bbb", "ddd", "aaa");

Collection.sort(list, new Comparator<String>(){
   public int compare(String s1, String s2){
       return s2.compareTo(s1);
   } 
});
==>
List<String> list = Arrays.asList("abc","aaa","bbb","ddd","aaa");
Collections.sort(list,(s1,s2)->s2.compareTo(s1))
```

#### 함수형 인터페이스 타입의 매개변수, 반환타입

```java
void aMethod(MyFunction f){
    f.myMethod(); // MyFunction에 정의된 메서드 호출
}
@FunctionalInterface
interface MyFunction {
    void myMethod();
}
```

- 함수형 인터페이스 타입의 반환타입

```java
MyFunction myMethod(){
    MyFunction f = ()->{};
    return f;
}
```

### java.util.function 패키지

- 자주 사용되는 다양한 함수형 인터페이스를 제공

![functionPack](C:\Users\김민식\Pictures\functionPack.png)

- 매개변수가 2개인 함수형 인터페이스

![FunctionalInterface](C:\Users\김민식\Pictures\FunctionalInterface.png)

- 매개변수의 타입과 반환타입이 일치하는 함수형 인터페이스

![FIn](C:\Users\김민식\Pictures\FIn.png)

### Predicate(함수형 인터페이스)의 결합

- and() &&, or() ||, negate() !로 두 predicate를 하나로 결합(default메서드)

```java
Predicate<Integer> p = i-> i<100;
Predicate<Integer> q = i-> i<200;
Predicate<Integer> r = i-> i%2==0;

Predicate<Integer> notP = p.negate(); // i>= 100
Predicate<Integer> all = notP.and(q).or(r); // 100<=i&&i<200||i%2==0
Predicate<Integer> all2 = notP.and(q.or(r)); // 100<=i&&(i<200||i%2==0)

System.out.println(all.test(2)); // true
System.out.println(all2.test(2)); // false
```

- 등가비교를 위한 Predicate의 작성에는 isEqual()를 사용(static메서드)

### 컬렉션 프레임웍과 함수형 인터페이스

- 함수형 인터페이스를 사용하는 컬렉션 프레임웍의 메서드(와일드 카드 생략)

![collectionFramework](C:\Users\김민식\Pictures\collectionFramework.png)

### 스트림(Stream)

- 다양한 데이터 소스를 표준화된 방법으로 다루기 위한 것

```java
List<Integer> list = Arrays.asList(1, 2, 3, 4, 5);
Stream<Integer> intStream = list.stream(); // 컬렉션
Stream<String> strStream = Stream.of(new String[]{"a","b","C"}); //배열
Stream<Integer> evenStream = Stream.iterate(0, n->n+2); // 람다식
Stream<Double> randomStream = Stream.generate(Math::random); // 람다식
IntStream intStream = new Random().init(5); // 난수 스트림
```

- 스트림이 제공하는 기능 - 중간 연산과 최종 연산
  - 중간 연산 - 연산결과가 스트림인 연산. 반복적으로 적용 가능
  - 최종 연산 - 연산결과가 스트림이 아닌 연산. 단 한번만 적용 가능(스트림의 요소를 소모)
  - Stream.distinct().limit(5).sorted().forEach(System.out::println)

​					중간연산                                     최종연산

ex

```java
String[] starArr = {"dd","aaa","CC","cc","b"};
Stream<String> Stream = Stream.of(strArr); // 문자열 배열이 소스인 스트림
Stream<String> filteredStream = stream.filter(); // 걸러내기 (중간 연산)
Stream<String> distinctedStream = stream.distinct(); // 중복제거(중간 연산)
Stream<String> sortedStream = stream.sort(); // 정렬(중간 연산)
Stream<String> limitedStream = stream.limit(5); // 스트림 자르기 (중간 연산)
int total = stream.count(); // 요소 개수 세기 (최종 연산)
```

- 스트림의 특징
  - 스트림은 데이터 소스(원본)로부터 데이터를 읽기만 할 뿐 변경하지 않는다.
	- ```java
    List<Integer> List = Arrays.asList(3,1,5,4,2);
    List<Integer sortedList = list.stream().sorted()
    					.collect(Collectors.toList());
    System.out.println(list); //[3,1,5,4,2]
    System.out.println(sortedList); // [1,2,3,4,5]
    ```
  
  - 스트림은 Iterator처럼 일회용 (필요하면 다시 스트림을 생성해야 함)
  
  - ```java
    strStream.forEach(System.out::println); // 모든 요소를 화면에 출력 (최종 연산)
    int numOfStr = strStream.count(); // 에러. 스트림이 이미 닫혔음
    ```
  
  - 최종 연산 전까지 중간연산이 수행되지 않는다 - 지연된 연산
  
  - ```java
    IntSream intStream = new Random().ints(1,46); // 1~45범위의 무한 스트림
    IntSream.distinct().limti(6).sorted() // 중간연산
            .forEach(i->System.out.print(i+",")); //최종 연산
    ```
  
  - 스트림은 작업을 내부 반복으로 처리
  
  - ```java
    for(String str : strList)
        System.out.println(str);
    stream.forEach(System.out::println);
    ```
  
  - 스트림의 작업을 병렬로 처리 - 병렬스트림
  
  - ```java
    Stream<String> strStream = Stream.of("dd","aaa","CC","cc","b");
    int sum = strStream.parallel() // 병렬 스트림으로 전환 (속성만 변경)
        .mpaToInt(s -> s.length()).sum(); // 모든 문자열의 길이의 합
    ```
  
  - 기본형 스트림 - IntStream, LongStream, DoubleStream
  
  - 오토박싱&언박싱의 비효율이 제거됨(Stream<Integer>대신 IntStream사용)
  
  - 숫자와 관련된 유용한 메서드를 Stream<T>보다 더 많이 제공

### 스트림 만들기

####  컬렉션

- Collection인터페이스의 stream()으로 컬렉션을 스트림으로 변환

- Stream<E> stream()

- ```java
  List<Integer> list = Arrays.asList(3,1,5,4,2);
  Stream<Integer> intStream = list.stream(); //리스트를 스트림으로 변환
  		
  intStream.forEach(System.out::print); //3,1,5,4,2
  intStream.forEach(System.out::print); // 에러. 스트림이 이미 닫혔다.
  ```

#### 배열

- 객체 배열로부터 스트림 생성

  - ```java
    Stream<T> Strea.of(T... values) // 가변인자
    Stream<T> Stream.of(T[])
    Stream<T> Arrays.Stream(T[])
    Stream<T> Arrays.stream(T[] array. int startInclusive, int endExclusive)
        
    Stream<String> strStream = Stream.of("a","b","c"); //가변인자
    Stream<String> strStream = Stream.of(new String[]{"a","b","c"}); //가변인자
    Stream<String> strStream = Arrays.stream(new String[]{"a","b","c"}); //가변인자
    Stream<String> strStream = Arrays.stream(new String[]{"a","b","c"},0,3); //가변인자
    ```

  - 개본형 배열로부터 스트림 생성

  - ```java
    IntStream IntStream.of(int... values) //Stream이 아니라 IntStream
    IntStream IntStream.of(int[])
    IntSream Arrays.stream(int[])
    IntStream Arrays.stream(int[] array,int startInclusive, int endExclusive)
    ```

#### 임의의 수

- 난수를 요소로 갖는 스트림 생성

  ```java
  IntSreamintStream = new Random().ints(); // 무한 스트림
  intStrea.limit(5).forEach(System.out::println); // 5개의 요소만 출력
  
  IntStream intStream = new Random().ints(5); // 크기가 5인 난수 스트림을 반환
  ```

  ```java
  Integer.MIN_VALUE <= ints() <= Integer.MAS_VALUE
  	Long.MIN_VALUE <= longs() <= Long.MAX_VALUE
      	0.0 <= doubles() < 1.0
  ```

- 지정된 범위의 난수를 요소로 갖는 스트림을 생성하는 메서드(Random 클래스)

  ```java
  IntStream ints(int begin, int end)	// 무한 스트림
  LongStrem longs(long begin, long end)
  DoubleStream doubles(double Begin, double end)
  
  IntStream ints(Long streamSize, int begin, int end) // 유한 스트림
  LongStream longs(long streamSize, long begin, long end)
  DoubleStream doubles(long streamSize, double begin, double end)
  ```

- 특정 범위의 정수를 요소로 갖는 스트림 생성 (IntStream, LongStream)

```java
IntStream intStream = IntStream.range(1, 5); // 1,2,3,4
IntStream intStream = IntStream.rangeClosed(1, 5); // 1,2,3,4,5,
```

#### 람다식 iterate(), generate()

- 람다식 소스로 하는 스트림 생성

```java
static <T> Stream<T> iterate(T seed,UnaryOperator(T) f) // 이전 요소에 종속적
static <T> Stream<T> generate (Supplier<T> s) // 이전 요소에 독립적
```

- iterate()는 이전 요소를 seed로 해서 다음 요소를 계산

```java
Stream<Integer> evenStream = Stream.iterate(0, n->n+2); // 0, 2, 4, 6, ...
```

- generate()는 seed를 사용하지 않는다.

```java
Stream<Double> randomStream = stream.generate(Math::random);
Stream<Integer> oneStream = Stream.generate(()->1);
```

#### 파일과 빈 스트림

- 파일을 소스로 하는 스트림 생성하기

```java
Stream<path> Files.list(Path dir)

Stream<String> Files.lines(Path path) // 파일 내용을 라인단위로 나눔
Stream<String> Files.lines(Path path, Charset cs)
Stream<String> lines() // BufferedReader클래스의 메서드
```

- 비어있는 스트림 생성

```java
Stream emptyStream = Stream.empty(); // empty()는 빈 스트림을 생성해서 반환
long count = emptyStream.count(); // count의 값은 0
```

