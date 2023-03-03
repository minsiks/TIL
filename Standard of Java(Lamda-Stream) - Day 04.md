# 23-3-3 Standard of Java(Lamda-Stream)  Day 04

## 스트림의 최종연산

> [자바의 정석](https://www.youtube.com/watch?v=M_4a4tUCSPU&list=PLW2UjW795-f6xWA2_MUhEVgPauhGl3xIp&index=169)

### forEach()

```java
void forEach(Consumer<T> action) // 병렬 스트림인 경우 순서가 보장되지 않음
void forEachOrdered(Consumer<T> action)// 병렬 스트림인 경우 순서가 보장됨
```

```java
IntStream.range(1, 10).sequential().forEach(System.out::print); // 123456789
IntStream.range(1, 10).sequential().forEachOrdered(System.out::print)// 123456789
```

```java
IntStream.range(1,10).parallel().forEach(System.out::print); //683295714
IntStream.range(1,10).paralled().forEachOrdered(System.out::print) //123456789
```

### 조건 검사 - allMatch(), anyMatch(), noneMatch()

```java
boolean allMatch (predicate<T> predicate) // 모든 요소가 조건을 만족시키면 true
boolean anyMatch (predicate<T> predicate) // 한 요소라도 조건을 만족시키면 true
boolean noneMatch (predicate<T> predicate) // 모든 요소가 조건을 만족시키지 않으면 true
```

```java
boolean hasFiledStu = stuStream.anyMatch(s->s.getTotalScore()<=100); //낙제자가 있는지
```

### 조건에 일치하는 요소 찾기 - findFirst() , findAny()

```java
Optional<T> findFirst() // 첫 번째 요소를 반환. 순차 스트림에 사용
Optional<T> findAny() // 아무거나 하나를 반환. 병렬 스트림에 사용
```

```java
Optional<Student> result = stuStream.filter(s-> s.getTotalScore() <= 100).findFirst();
Optional<Student> result = parallelStream.filter(s->s.getTotalScore()<= 100).findAny();
```

### 스트림의 요소를 하나씩 줄여가며 누적연산 수행 -  reduce()

```java
Optional<T> reduce reduce(BinaruOperator<T> accumulator)
T reduce(T identity, BinaryOperator<T> accumulator)
U reduce(T identity, BinaryOperator<T> accumulator, BinaryOperator<U> combiner)
```

- identity - 초기값
- accumulator - 이전 연산결과와 스트림의 요소에 수행할 연산
- combiner - 병렬처리된 결과를 합치는데 사용할 연산 (병렬 스트림)

```java
// int reduce(int identity, IntBinaryOperator op)
int count = intStream.reduce(0, (a,b) -> a + 1); //count()
int sum = intStream.reduce(0, (a,b) -> a + b); // sum()
int max = intStream.reduce(Integer.MIN_VALUE,(a,b) -> a > b ? a : b); // max()
int min = intStream.reduce(integer.MAX_VALUE,(a,b) -> a < b ? a : b) // min()
```

### Collect()와 Collectors

- collect()는 Collector를 매개변수로 하는 스트림의 최종연산

```java
Object collect(Collector collector) // Collector를 구현한 클래스의 객체를 매개변수로
Obejct collect(Supplier supplier, BiConsumer accumulator, BiConsumer combiner)// 잘 안쓰임
```

- Collector는 수집(collect)에 필요한 메서드를 정의해 놓은 인터페이스

```java
public interface Collector<T, A, R> { // T(요소)를 A에 누적한 다음, 결과로 R로 변환해서 반환
    Supplier<A> supplier(); // StringBuilder::new // 누적할 곳
    BiConsumer<A, T> accumulator(); // (sb,s) -> ab.append(s) 누적장법
    BinaryOperator<A> combizner(); // (sb1, sb2) -> sb1.append(sb2) 결합방법(병렬)
    Function<A, R> finisher(); // sb -> sb.toString() 최종변환
    Set<Characteristics> characteristics(); // 컬렉터의 특성이 담긴 Set을 반환
}
```

- Collectors클래스는 다양한 기능의 컬렉터(Collector를 구현한 클래스)를 제공
  - 변환 - mapping() , toList(), toSet(), toMap(), toCollection(), ...
  - 통계 - counting(), summingInt(), averagingInt(), maxBy(), minBy(), summarizingInt(),...
  - 문자열 결합 - Joining()
  - 리듀싱 - reducing()
  - 그룹화와 분할 - groupingBy(), pratitioningBy(), collectionAndThen()

### 스트림을 컬렉션, 배열로 변환

- 스트림을 컬렉션으로 변환 - toList(), toSet(), toMap(), toCollection()

```java
List<String> names = stuStream.map(Studen::getName) // Stream<Student>->Stream<String>
						.collect(Collectors.toList()); //Stream<String>->List<String>
ArrayList<String> list = names.stream()
    .collect(Collectors.toCollection(ArrayList::new)); //Stream<String>->ArrayList<String>
Map<String,Person> map = personStream
    .collect(Collectors.toMap(p->p.getRegId(),p->p));//Steream<Person>->Map<String,Person>
```

- 스트림을 배열로 변환 - toArray()

```java
Student[] stuNames = studentStream.toArray(Student[]::new); // OK
Student[] stuNames = studentStream.toArray(); //에러
Object[] stuNames = studentStream.toArray(); //OK
```

### 스트림의 통계 - counting(), summingInt()

- 스트림의 통계정보 제공 - counting(), summingInt(), maxBy(), minBy(), ...

```java
long count = stuStream.count();
long count = stuStream.collect(counting()); // Collectors.counting() 그룹별로 나누기 가능
```

```java
long totalScore = stuStream.mapToInt(student::getTotalScore).sum(), // IntStream 의 sum()
long totalScore = stuStream.collect(summingInt(Student::getTotalScore));
```

```java
OptionalInt topScore = studentStream.mapToInt(Student::getTotalScore).max();
Optional<Student> topStudent = stuStream
    .max(Comparator.comparingInt(Student::getTotalScore));
Optional<Student> topStudent = stuStream
    .collect(maxBy(Comparator.comparingInt(Student::getTotalScore)));
```

### 스트림을 리듀싱 - reducing()

- 그룹별 리듀싱

```java
Collector reducing(BinaryOperator<T> op)
Collector reducing(T identity, BinaryOperator<T> op)
Collector reducing(T identity, Function<T,U> mapper, BinaryOperator<U> op) // map+reduce
```

```java
IntStream intStream = new Random().ints(1,46).distinct().limit(6);

OptionalInt max = intStream.reduce(Integer::max); // 전체 리듀싱
Optional<Integer> max = intStream.boxed().collect(reducing(Integer::max));
```

```java
long sum = intStream.reduce(0,(a,b) ->a + b);
long sum = intStream.boxed().collect(reducing(0,(a,b)->a + b))
```

```java
int grandTotal = stuStream.map(Student::getTotalScore).reduce(0, Integer::sum);
int grandTotal = stuStream.collect(reducing(0, Student::getTotalScore,Integer::sum));
```

- 문자 스트림의 요소를 모두 연결 - joining ()

```java
String studentNames = stuStream.map(Student::getName).collect(joining());
String studentNamse = stuStream.map(Student::getName).collect(joining(",")); // 구분자
String studentNames = stuStream.map(Student::getName).collect(joining(",","[","]"));
String studentInfo = stuStream.collect(joining(",")) // Student의 toString()으로 결합
```

### 스트림의 그룹화와 분할

- partitioningBy()는 스트림을 2분할

```java
Collector partitioningBy(Predicate predicate)
COllector partitioningBy(Predicate predicate, Collector downstream)
```

- groupingBy()는 스트림을 n분할

```java
Collector groupingBy(Function classifier)
Collector groupingBy(Function classifier, Collector downstream)
Collector groupingBy(Function classifier, Supplier mapFactory, Collector downstream)
```

- partitioningBy()
  - 스트림의 요소를 2분할

```java
Collector partitioningBy(Predicate predicate)
Collector partitioningBy(Predicate predicate, Collector downstream)
```

```java
Map<Boolean, List<Student>> stuBySex = stuStream
    .collect(partitioningBy(Student::isMale)); // 학생들을 성별로 분할
List<Student> maleStudent = stuBySex.get(true); // Map에서 남학생 목록을 얻는다.
List<Student> femaleStudent = stuBySex.get(false); // Map에서 여학생 목록을 얻는다.
```

```java
Map<Boolean, Long> stuNumBySex = stuStream
    .collect(partitioningBy(Student::isMale, counting())); // 분할 + 통계
System.out.println("남학생 수 :" + stuNumbySex.get(true)); // 남학생 수 : 8
System.out.println("여학생 수 :" + stuNumbySet.get(false)); // 여학생 수 : 10
```

```java
Map<Boolean, Optional<Student>> topScoreBySex = stuStream // 분할 + 통계
    .collect(partitioningBy(Student::isMale, maxBy(comparingInt(Student::getScore))));
System.out.println("남학생 1등 :" + topScoreBysex.get(true)); // 남학생 1등 : ~
System.out.println("여학생 1등 :" + topScoreBySex.get(false)); // 여학생 1등 : ~
```

```java
Map<Boolean, Map<Boolean, List<Student>> failedstuBySex = stuStream.collect(partitioningBy(Student::isMale //1, 성별로 분할 (남/녀)                                 ,partitioningBy(s -> s.getScore() < 150))); // 2. 성적으로 분할(불합격/합격)
List<Student> failedMaleStu = failedStuBySex.get(true).get(true);
List<Student> failedFemaleStu = failedStuBySex.get(false).get(true);
```

### 스트림의 그룹화 - groupingBy()

- 스트림의 요소를 그룹화

```java
Map<Integer, List<Student>> stuByBan = stuStream			// 학생을 반별로 그룹화
    .collect(Collectors.groupingBy(Student::getBan, toList())); // toList() 생략가능
```

```java
Map<Integer, Map<Integer,List<Student>>> stuByHakAndBan = stuStream // 다중그룹화
    .collect(groupingBy(Student::getHak,			//1. 학년별 그룹화
             groupingBy(Student::getBan)			//2. 반별 그룹화
       ));
```

```java
Map<Integer, Map<Integer, Set<Student.Level>>> stuByHakAndBan = stuStream.collect(
	groupingBy(Student::getHak, groupingBy(Student::getBan,
          mapping(s->{
              if (s.getScore() >= 200) return Student.Level.HIGH;
              else if(s.getScore() >= 100) return Student.Level.MID;
              else return Student.Level.Low;
          }, toSet())
      ))
);
```

### 스트림의 변환

![StreamChange1](C:\Users\김민식\Pictures\Screenshots\StreamChange1.png)

![ChangeStream](C:\Users\김민식\Pictures\Screenshots\ChangeStream.png)
