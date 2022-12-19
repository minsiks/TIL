# 22-12-19 Java Programming Beginners - Day 05

## Inflearn 자바 프로그래밍 입문

> 박은종

## 추상 클래스와 템플릿 메서드

- 템플릿 메서드 : 추상 메서드나 구현된 메서드를 활용, 전체 기능의 흐름을 정의하는 메서드
  - final로 선언하면 하위 클래스에서 재정의 할 수 없음

```java
package gamelevel;

public abstract class PlayerLevel {

	public abstract void run();
	public abstract void jump();
	public abstract void turn();
	public abstract void showLevelMessage();
	
	final public void go(int count) {
		run();
		for (int i = 0; i < count; i++) {
			jump();
		}
		turn();
	}
}
```

## 인터페이스 선언과 구현

### 인터페이스

- 구현코드가 없는 추상 메서드만 있는 클래스
- 설계용으로 사용

#### 인터페이스 구현과 형 변환

- 인터페이스를 구현한 클래스는 인터페이스 형으로 선언한 변수로 형 변환 할 수 있음
- 상속에서의 형 변환 동일한 의미
- 단 클래스 상속과 달리 구현코드가 없기 때문에 여러 인터페이스를 구현할 수 있음
- 형 변환시 사용할 수 있는 메서드는 인터페이스에 선언된 메서드만 사용

## 인터페이스와 다형성 구현

### 인터페이스와 다형성

- 인터페이스는 "Client Code" 와 서비스를 제공하는 "객체"사이의 약속
- Interface 타입이라 함은 그 interface가 제공 하는 메서들르 구현했다는 의미
- Client는 어떻게 구현되었는지 상관없이 interface의 정의만을 보고 사용
- 다양한 구현이 필요한 인터페이스를 설계하는 일은 매우 중요

### 인터페이스의 요소

- 상수 : 모든 변수는 상수로 변환
- 추상메서드 : 모든 메서드는 추상 메서드로 구현
- 디폴트 메서드 : 기본 구현을 가지는 메서드, 구현 클래스에서 재정의 할 수 있음
- 정적 메서드 : 인스턴스 생성과 상관 없이 인터페이스 타입으로 사용할 수 있는 메서드
- private 메서드 : 인터페이스를 구현한 클래스에서 사용하거나 재정의 할 수 없음. 인터페이스 내부에서만 기능을 제공하기 위해 구현하는 메서드

## 인터페이스 활용

```java
package interfaceex;

public interface Calc {
	
	double PI = 3.14;
	int ERROR = -9999999;
	
	int add(int num1, int num2);
	int substract(int num1, int num2);
	int times(int num1, int num2);
	int divide(int num1, int num2);
	
	default void description() {
		System.out.println("정수 계산기를 구현합ㄴ디ㅏ.");
	}
	
	static int total(int[] arr) {
		int total = 0;
		
		for (int i : arr) {
			total += i;
		}
		return total;
	}
}
```

### 인터페이스 상속

- 인터페이스 간에도 상속이 가능
- 구현코드의 상속이 아니므로 형 상속(type inheritance) 라고 함

### 인터페이스 구현과 클래스 상속 함께 사용하기

- ex)` public class BookShelf extends Shelf implements Queue`

## 기본 클래스 _ 자바  JDK

### java.lang 패키지

- 프로그래밍시 improt 하지 않아도 자동으로 import됨
- 많이 사용하는 기본클래스들이 속한 패키지
- String, Integer, System 등

### Object 패키지

- 모든 클래스의 최상위 클래스
- 컴바일러가 extends Object를 함
- java.lang.Object 클래스

### toString() 메서드

- 객체의 정보를 String으로 바꾸어서 사용할 때 많이 쓰임

### equals() 메서드

- 두 인스턴스의 주소 값을 비교하여 true/false를 반환
- 재정의 하여 두 인스턴스가 논리적으로 동일함의 여부를 반환

### hashCode() 메서드

- hash : 정보를 저장, 검색하기 위해 사용 하는 자료구조
- 자료의 특정 값(키 값)에 대해 저장 위치를 반환해주는 해시 함수를 사용
- hashCode()의 반환 값 : 자바 가상 머신이 저장한 인스턴스의 주소값을 10진수로 나타냄

### clone() 메서드

- 객체의 원본 복제하는데 사용하는 메서드
- 원본을 유지해 놓고 복사본을 사용할 때
- 객체의 clone() 메서드 사용을 허용한다는 의미로 cloneable 인터페이스를 명시

### String 클래스

- String을 선언하는 두 가지 방법
  - String str1 = new String("abc"); // 생성자의 매개변수로 문자열 생성
  - String str2 = "test"; // 문자열 상수를 가리키는 방식

- 힙 메모리에 인스턴스로 생성되는 경우와 상수 풀(constant pool)에 있는 주소를 참조하는 방법 두 가지

- 한번 생성된 String 값(문자열)은 불변
- 두 개의 문자열을 연결하면 새로운 인스턴스가 생성 됨
- 문자열 연결을 계속하면 메모리에 gabage가 많이 생길 수 있음

### StringBuilder, StringBuffer 사용

- 내부적으로 가변적인 char[] 배열을 가지고 있는 클래스
- 문자열을 여러 번 연결하거나 변경할 때 사용하면 유용
- 매번 새로 생성 X 기존 배열을 변경하므로 gabage가 생기지 않음
- StringBuffer는 멀티 쓰레드 프로그래밍에서 동기화
- 단일 쓰레드 프로그램에서는 StringBuilder를 사용하기를 권장
- toString() 메서드로 String 전환

### Wrapper 클래스

- 기본 자료형(primitive data type)에 대한 클래스
- (Boloean), (Byte) 등등

### Class 클래스

- 자바 모든 클래스와 인터페이스는 컴파일 후 class 파일로 생성
- class 파일에는 객체의 정보 (멤버변수, 메서드, 생성자 등)가 포함
- Class 클래스는 컴파일된 class 파일에서 객체의 정보를 가져올 수 있음

- Class.forName("클래스 이름")메서드 사용

- reflection 프로그래밍 : Class 클래스를 이용하여 클래스의 정보(생성자, 멤버변수, 메서드)를 가져오고 이를 활용하며 인스턴스를 생성, 메서드를 호출하는 등의 프로그래밍 방식

- Class.forName() 메서드로 동적 로딩 하기
  - 동적 로딩 : 컴파일 시에 데이터 타입이 모두 binding 되어 자료형이 로딩 되는 것 (static loding)이 아니라 실행 중에 데이터 타입을 알고 binding 되는 방식

## 제네릭 프로그래밍

- 변수의 선언이나 메서드의 매개변수를 하나의 참조 자료형이 아닌 여러 자료형을 변환 될 수 있도록 프로그래밍 하는 방식
- 실제 사용되는 참조 자료형으로 변환은 컴파일러가 검증하므로 안정적인 프로그래밍 방식

- 컬렉션 프레임워크에서 많이 사용

### 제네릭 클래스 정의 하기

- 여러 참조 자료형으로 대체 될 수 있는 부분을 하나의 문자로 표현
- 이 문자를 자료형 매개 변수라고 함

```java
package generics;

public class TreeDPrinter<T> {
	private T materail;

	public T getMaterail() {
		return materail;
	}

	public void setMaterail(T materail) {
		this.materail = materail;
	}	
}
```

### 자료형 매개 변수 T

- type의 의미로 T를 많이 사용 함
- <T> 에서 <>는 다이아몬드 연산자
- stiatic 키워드는 T 에 사용할 수 없음

## 컬렉션 프레임워크

### 자료구조의 이해 Review

- Array(배열) : 같은 형의 데이터 타입을 메모리에 저장하는 선형적 자료구조
  - 논리적 구조와 물리적 구조가 동일합니다.
  - 고정 길이 (fixed length)

- Linked List : 데이터와 링크로 구성
  - Node가 다음 Node를 가리킴
- Stack : 일반적인 의미 - 쌓다, 더미 / 선형 자료구조
  - LIFO 
  - push(), pop()
- Queue :  대기열 / 선형 자료구조
  - FIFO
  - enQueue(), deQueue

- Tree 
  - Binary Search Tree : 자료의 검색에 활용
    - 자식노드의 수가 최대 2개인 트리

- Hasing 
  - 산술 연산을 이용한 검색 방식

### 컬렉션 프레임워크

- 프로그램 구현에 필요한 자료구조(Data structure)를 구현해 놓은 라이브러리
- java.util 패키지에 구현

- 개발에 소요되는 시간을 절약하면서 최적화 된 알고리즘을 사용할 수 있음
- 여러 인터페이스와 구현 클래스 사용 방법을 이해해야 함

![collection](C:\Users\김민식\Documents\TIL\assets\collection.png)

- Map과 Collection의 차이
  - Collection은 단일 객체들을 관리, Map은 쌍으로 된 객체들을 관리
- List와 Set차이
  - List는
  - Set은 

- Iterator : 순회

### Collection 인터페이스

- 하나의 객체를 관리하기 위한 메서드가 선언된 인터페이스
- List
  - 순서가 있는 자료구조, 중복 허용
  - ArrayList, Vectior, LinkedList 등
- Set
  - 중복되는 값을 넣을 수 없음, key값에 따라 특정한 index를 부여 받음
  - HashSet, TreeSet 등

- Collection 인터페이스에 선언된 주요 메서드
  - add(E e) : 객체를 추가
  - clear() : 모든 객체를 제거
  - Iterator<E> iterator : 순환할 반복자를 반환
  - remove(Object o) : 매개변수에 해당하는 인스턴스가 존재하면 제거
  - size() : 요소의 개수를 반환

### List 인터페이스

- Collection 하위 인터페이스
- 객체를 순서에 따라 저장하고 관리
- 배열의 기능을 구현하기 위한 인터페이스
- ArrayList, Vector, LinkedList 등

### ArrayList 와 Vector

- 객체 배열을 구현한 클래스
- Vector 는 자바 2 부터 제공된 클래스
- 멀티 쓰레드 상태에서 리소스에 대한 동기화가 필요한 경우 Vector를 사용
- 일반적으로 ArrayList를 더 많이 사용
- 동기화(sysnchronization) : 두 개의 쓰레드가 동시에 하나의 리소스에 접근 할 때 순서를 맞추어서 데이터에 오류가 발생하지 않도록 함

