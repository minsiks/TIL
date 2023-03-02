# 23-2-22 Standard of Java(Lamda-Stream)  Day 03

### 스트림 생성

- Collection인터페이스의 stream()으로 컬렉션을 스트림으로 변환

```java
Stream<E> stream(); // Collcetion인터페이스 메서드

List<Integer> list = Arrays.asList(1,2,3,4,5);
Stream<Integer> intStream = list.stream(); // list를 스트림으로 변환
// 스트림의 모든 요소를 출력
intStream.forEach(System.out::print); //12345
```

### 스트림 생성 - 배열

- 객체 배열로부터 스트림 생성

```java
Stream<T> Stream.of(T.. values); // 가변 인자
Stream<T> Stream.of(T[]);
Stream<T> Arrays.stream(T[]);
Stream>T> Arrays.stream(T[] array, int startInclusive, int endExclusive);

Stream<String> strStream = Stream.of("a","b","c");
Stream<String> strStream = Stream.of(new String[]{"a","b","c"});
Stream<String> strStream = Arrays.stream(new String[]{"a","b","c"});
Stream<String> strStream = Arrays.stream(new String[]{"a","b","c"},0,3); // index가 0부터 3 0<=i<3 (0,1,2)
```

- 기본형 배열로부터 스트림 생성

```java
IntStream IntStream.of(int...values);
IntStream IntStream.of(int[]);
IntStream Arrays.stream(int[]);
IntStream Arrays.stream(int[] array,int startInclusive, int endExclusive);
```

### 스트림 생성 - 임의의 수

```java
IntStreamintStream = new Random().ints(); // 무한스트림
intStream.limit(5).forEach(System.out::println); // 5개의 요소만 출력한다.

IntStream intStream = new Random().ints(5); // 크기가 5인 난수 스트림을 변한
```

### 스트림 생성 - 특정 범위의 정수

- 특정 범위의 정수를 요소로 갖는 스트림 생성 (IntStream, LongStream)

```java
IntStream.range(1,5);
IntStream.rangeClosed(1,5);
```

### 스트림 생성 - 람다식

- 람다식을 소스로 하는 스트림 생성

```java
static <T> Stream<T> interate(T seed, UnaryOperator<T> f) // 이전 요소에 종속적
static <T> Stream<T> generate(Supplier<T> s) // 이전 요소에 독립적
```

- iterate()는 이전 요소를 seed (초기값) 로 해서 다음 요소를 제거

```java
Stream<Integer> evenStream = Stream.iterate(0, n->n+2);
```

- generate()는 seed (초기값) 를 사용 X

```java
Stream<Double> randomStream = Stream.generate(Math::random);
Stream<Integer> oneStream = Stream.generate(()->1);
```

### 스트림 생성 - 파일과 빈 스트림

- 파일을 소스로 하는 스트림 생성

```java
Stream<Path> Files.list(Path dir) // Path는 파일 또는 디렉토리

Stream<String> Files.lines(Path path)
Stream<String> Files.lines(Path path, Charset cs)
Stream<String> lines() // bufferedReader클래스의 메서드
```

- 비어있는 스트림 생성하기

```java
Stream emptyStream = Stream.empty();
```

## 스트림의 연산

- 스트림이 제공하는 기능 - 중간 연산과 최종 연산

> 중간 연산 - 연산결과가 스트림인 연산. 반복적으로 적용가능
>
> 최종 연산 - 연산결과가 스트림이 아닌 연산. 단 한번만 적용가능(스트림의 요소를 소모)

`stream.distinct().limit(5).sorted().forEach(System.out::println)`

### 중간연산

![midCur](C:\Users\김민식\Pictures\Screenshots\midCur.png)

### 최종 연산

![fiCur](C:\Users\김민식\Pictures\Screenshots\fiCur.png)

## 스트림의 중간연산

### 스트림 자르기 - skip(), limit()

```java
Stream<T> skip(long n) // 앞에서부터 n개 건너뛰기
Stream<T> limit(long maxSize) // maxSize 이후의 요소는 잘라냄
    
IntStream intStream = IntStream.rangeClosed(1,10); //1,2,3,4,5,6,7,8,9,10
intStream.skip(3).limit(5).forEach(System.out::print); //45678
```

### 스트림의 요소 걸러내기 - filter(), distinct()

```java
Stream<T> filter() //조건에 맞지 않는 요소 제거
Stream<T> distinct() //중복제거
```

```java
IntStream intStream = IntStream.of(1,2,2,3,3,3,4,5,5,6);
intStream.distinct().forEach(System.out::print); //123456
```

```java
IntStream intStream = IntStream.rangeClosed(1,10); // 12345678910
intStream.filter(i->i%2==0).forEach(System.out.print); //246810
```

```java
intStream.filter(i->i%2!=0 && i%3!=0).forEach(System.out::print);
intStream.filter(i->i%2!=0).filter(i->i%3!=0).forEach(System.out::print);
```

### 스트림 정렬하기 - sorted()

```java
Stream<T> sorted() // 스트림 요소의 기본 정렬(Comparable)로 정렬
Stream<T> sorted(Comparator<? super T> comparator) //지정된 Comparator로 정렬 
```

![StreamSort](C:\Users\김민식\Pictures\Screenshots\StreamSort.png)

### Comparator의 comparing()으로 정렬 기준을 제공

```java
comparing(Function<T, U> keyExtractor)
comparing(Function<T, U> keyExtractor, Comparator<U> keyComparator)
```

```java
sudentStream.sorted(Comparator.comparing(Student::getBan)) // 반별로 정렬
    .forEach(System.out::println);
```

### 추가 정렬 기준을 제공할 때는 thenComparing() 을 사용

```java
studentStream.sorted(Comparator.comparing(Student::getBan) //반별로 정렬
                    .thenComparing(Student::getTotalScore) // 총점별로 정렬 
                    .thenComparing(Student::getName)) // 이름 별로 정렬
                    .forEach(System.out.println);
```

### 스트림 요소 변환하기 - map()

```java
Stream<R> map(Function<T,R> mapper) // Stream<T> -> Stream<R>
```

```java
Stream<File> fileStream = Stream.of(new File("Ex1.java"), new File("Ex1")
                                   new File("Ex1.bak"), new File ("Ex2.java"), new File("Ex1.txt"));

Stream<String> filenameStream = fileStream.map(File::getName);
filenameStream.forEach(System.out::println); // 스트림의 모든 파일의 이름을 출력
//Stream<File> ->(map(File::getName))) -> Stream<String>
```

- ex) 파일 스트림(Stream<File>에서 파일 확장자(대문자)를 중복없이 뽑아내기)

```java
fileStream.map(File::getName)					// Stream<File> -> Stream<String>
    .filter(s->s.indexOf('.')!=-1)				// 확장자가 없는 것은 제외
    .map(s->s.substring(s.indexOf('.')+1))		// Stream<String> -> Stream<String>
    .map(String::toUpperCase)					// Stream<String> -> Stream<String>
    .distinct() // 중복 제거
   	.forEach(System.out::print) //JAVABAKTXT
```

### 스트림의 요소를 소비하지 않고 엿보기 - peek()

```java
Stream<T> peek(Consumer<T> action) // 중간 연산 (스트림을 소비 X)
void ForEach(Consumer<T> action) // 최종 연산 (스트림을 소비 O)
```

```java
fileStream.map(file::getName) // Stream<File> -> Stream<String>
    .filter(s-> s.indexOf('.')!=-1) // 확장자가 없는 것은 제외
    .peek(s->System.out.printf("filename%s%n",s))//파일명을 출력
    .map(s->s.substring(s.indexOf('.')+1)) // 확장자만 추출
    .peek(s->System.out.printf("extension=%s%n",s)) // 확장자를 출력
    .forEach(System.out::println); // 최종연산 스트림을 소비
```

### 스트림의 스트림을 스트림으로 변환 - flatMap()

```java
Stream<String[]> strArrStr = Stream.of(new String[]{"abc","def","ghi"},
                                       new String[]{"ABC", "GHI","JKLMN"});
```

```java
Stream<Stream<String>> strStrStrm = strArrStrm.map(Arrays::stream);
```

```java
Stream<String> strStrStrm = strArrStrm.flatMap(Arrays::stream); // Arrays.stream(T[])
```

## Optional<T>

### T 타입 객체의 래퍼클래스 - Optional<T>

```java
public final class Optional<T> {
    private final T value; // T타입의 참조변수 <- 모든 종류의 객체 저장
    ...
}
```

- 사용 이유 
  - null을 직접 다루는것은 위험(NullPointException 때문) -> 간접적으로 null 다루기
  - null체크 생략 (null체크시 if문 필수, 코드가 지저분)

```java
Object result = getResult(); // null or Object
result.toString(); // nullPointException 발생 가능성 有
if(result!=null)
    println(result.toString); // if문으로 Check 항상 사용 必
```

### Optional<T>객체 생성

- 다양한 방법

```java
String str = "abc";
Optional<String> optVal = Optional.of(str);
Optional<String> optVal = Optional.of("abc");
Optional<String> optVal = Optional.of(null);          // NullPointException 발생
Optional<String> optVal = Optional.ofNullable(null);  // OK
```

- null대신 빈 Optional<T>객체를 사용

```java
Optional<String> optVal = null; // 널로 초기화. 바람직 하지 않음
Optional<String> optVal = Optional.<String>empty(); // 빈 객체로 초기화
```

### Optional<T> 객체의 값 가져오기

- get(), orElse(), orElseGet(), orElseThrow()

```java
Optional<String> optVal = Optional.of("abc");
String str1 = optVal.get();  // optVal에 저장된 값을 반환. null이면 예외 발생
String str2 = optVal.orElse(""); // optVal에 저장된 값이 null일 때는, ""를 반환
String str3 = optVal.orElseGet(String::new); // 람다식 사용가능 () -> new String()
String str4 = optVal.orElseThorw(NullPointerException::new); // 널이면 예외발생
```

- isPresent() - Optional객체의 값이 null 이면 false, 아니면 true를 반환

```java
if(Optional.ofNullable(str).isPresent()){ // if(str!=null)
    System.out.println(str);
}
// ifPresent(Consumer) - 널이 아닐때만 작업 수행, 널이면 아무 일도 안 함
Optional.ofNullable(str).ifPresent(System.out::println); // <- value가 null이 아닐때만
```

### OptionalInt, OptionalLong, OptionalDouble

- 기본형 값을 감싸는 래퍼클래스

```java
public final class OptionalInt {
    private final boolean isPresent; // 값이 저장되어 있으면 true
    private final int value; // int 타입의 변수
}
```

- OptionalInt의 값 가져오기 - int getAsInt()

```java
Optional<T> -> get();
OptionalInt -> getAsInt();
OptionalLong -> getAsLong();
OptionalDouble -> getAsDouble();
```

- 빈 Optional객체와의 비교

```java
OptionalInt opt = OptionalInt.of(0); // OptionalInt에 0을 저장
OptionalInt opt2 = OptionalInt.empty(); // OptionalInt에 0을 저장

System.out.println(opt.isPresent()); //true
System.out.println(opt2.isPresent());//false
System.out.println(opt.equals(opt2));//false
```

