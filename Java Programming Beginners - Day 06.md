# 22-12-20 Java Programming Beginners - Day 06

## Inflearn 자바 프로그래밍 입문

> 박은종

## 컬렉션 프레임워크

### Stack과 Queue

- Stack : Last In First Out (LIFO)
  - 맨 마지막에 추가 된 요소가 먼저 꺼내지는 자료구조

```java
package arraylist;

import java.util.ArrayList;

class MyStack{
	
	private ArrayList<String> arrayStack = new ArrayList<String>();
	
	public void push(String data) {
		arrayStack.add(data);
	}
	
	public String pop() {
		
		int len = arrayStack.size();
		if(len == 0) {
			System.out.println("스택이 비었습니다.");
			return null;
		}
		
		return arrayStack.remove(len-1);

	}	
	
	public String peek() {
		int len = arrayStack.size();
		if(len ==0) {
			System.out.println("스택이 비었습니다.");
			return null;
		}
		return arrayStack.get(len-1);
	}
}

public class StackTest {

	public static void main(String[] args) {

		MyStack stack = new MyStack();
		
		stack.push("a");
		stack.push("b");
		stack.push("c");
		
		System.out.println(stack.pop());
		System.out.println(stack.pop());
		System.out.println(stack.pop());

		
		
		System.out.println(stack.pop());
		
	}

}
```

- Queue : First In First Out (FIFO)
  - 먼저 저장된 자료가 먼저 꺼내지는 선착순

```java
package arraylist;

import java.util.ArrayList;

class MyQueue {
	
	private ArrayList<String> arrayQueue = new ArrayList<String>();
	
	public void enQueue(String data) {
		arrayQueue.add(data);
	}
	
	public String deQueue() {
		int len = arrayQueue.size();
		
		if(len==0) {
			System.out.println("큐가 비었습니다.");
			return null;
		}
		return arrayQueue.remove(0);
	}
}

public class QueueTest {

	public static void main(String[] args) {

		
	}

}
```

### Iterator 사용하여 순회

- Collection의 개체를 순회하는 인터페이스
- iterator() 메서드 호출
- 선언된 메서드

### Set 인터페이스

- 중복을 허용하지 않음
- get(i) 메서드가 제공되지 않음
- List는 순서기반 인터페이스지만, Set은 순서가 없음
- 아이디, 주민번호, 사번 등 유일한 값이나 객체를 관리할 때 사용

### TreeSet 클래스

- 객체의 정렬에 사용되는 클래스
- 내부적으로 이진 검색 트리(binary search tree)로 구현
- 객체 비교를 위해 Comparable이나 Comparator 인터페이스를 구현

### Comparable 인터페이스와 Comparator 인터페이스

- 정렬 대상이 되는 클래스가 구현해야 하는 인터페이스
- Comparable 은 compareTo() 메서드를 구현
  - 매개 젼수와 객체 자신(this)를 비교
- Comparator는 compare() 메서드를 구현
  - TreeSet 생성자에 Comparator가 구현된 객체를 매개변수로 전달
- 일반적으로 Comparable을 더 많이 사용
- 이미Comparable이 구현된 경우 comparator를 이용하여 다른 정렬 방식을 정의 가능

```java
package collection;

import java.util.Comparator;

public class Member implements Comparable<Member>, Comparator<Member>{

	private int memberId;
	private String memberName;
	
	public Member() {
	}
	public Member(int memberId,String memberName) {
		this.memberId = memberId;
		this.memberName = memberName;
	}
	
	public int getMemberId() {
		return memberId;
	}
	public void setMemberId(int memberId) {
		this.memberId = memberId;
	}
	public String getMemberName() {
		return memberName;
	}
	public void setMemberName(String memberName) {
		this.memberName = memberName;
	}
	
	public String toString() {
		return memberName + "회원님의 아이디는 " + memberId + " 입니다.";
	}

	@Override
	public int hashCode() {
		// TODO Auto-generated method stub
		return memberId;
	}

	@Override
	public boolean equals(Object obj) {
		if(obj instanceof Member) {
			Member member = (Member)obj;
			
			if(this.memberId == member.memberId) {
				return true;
			}
			else return false;
		}
		return false;
	}

	@Override
	public int compareTo(Member member) {
		return (this.memberName.compareTo(member.memberName))*(-1);
	}

	@Override
	public int compare(Member member1, Member member2) {
		return member1.memberId - member2.memberId;
	}
	
}

```

### Map 인터페이스

- key- value pair 의 객체를 관리하는데 필요한 메서드가 정의 됨
- key는 중복 X
- 검색을 위한 자료 구조
- key를 이용하여 값을 저장하거나 검색, 삭제 할때 사용하면 편리
- 내부적으로 hash 방식으로 구현

- key가 되는 객체는 객체의 유일성함의 여부를 알기 위해 equals()와 hashCode() 메서드를 재정의

### TreeMap 클래스

- key 객체를 정렬하여 key-value를 pair로 관리하는 클래스
- key에 사용되는 클래스에 Comparable, Comparator 인터페이스를 구현
- java에 많은 클래스들은 이미 comparable이 구현

## 내부 클래스

### 내부 클래스 (Inner Class)

- 인스턴스 내부 클래스
- 정적 내부 클래스
- 지역 내부 클래스
- 익명 내부 클래스

```java
package innerclass;

class OutClass{
	
	private int num = 10;
	private static int sNum = 20;
	private InClass inClass;
	
	public OutClass(){
		inClass = new InClass();
	}
	
	class InClass{
		int inNum =200;
//		static int sInNum = 100;
		
		void inTest() {
			System.out.println(num);
			System.out.println(sNum);
		}
		
//		static void sTest() {
//			
//		}
	}
	public void usergInTest() {
		inClass.inTest();
	}
	
	static class InStaticClass{
		int iNum = 100;
		static int sInNum = 200;
		
		void inTest() {
//			num +=10;
			sNum += 10;
			System.out.println(sNum);
			System.out.println(iNum);
			System.out.println(sInNum);
		}
		static void sTest() {
			System.out.println(sNum);
			System.out.println(sInNum);
		}
	}
}

class InnerTest {

	public static void main(String[] args) {
		
//		OutClass outClass = new OutClass();
//		outClass.usergInTest();
		
//		OutClass.InStaticClass sInClass = new OutClass.InStaticClass();
//		sInClass.inTest();
		
		OutClass.InStaticClass.sTest();
	}
}
```

## 람다식

### 람다식 (lambda expression)

- 자바에서 합수형 프로그래밍(functional programming)을 구현하는 방식
- 자바 8 부터 지원
- 클래스를 생성하지 않고 함수의 호출만으로 기능을 수행
- 함수행 프로그래밍
  - 순수 함수(pure function)를 구현하고 호출함으로써 외부 자료에 부수적인 영향을 주지 않고 매개 변수만을 사용하도록 만든 함수
  - 함수를 기반으로 구현
  - 입력 받은 자료를 기반으로 수행되고 외부에 영향을 미치지 않으므로 병렬처리등에 가능
  - 안정적인 확장성 있는 프로그래밍 방식

### 람다식 구현

- 익명 함수 만들기
- 매개 변수와 매개 변수를 활용한 실행문 구현

```
int add(int x, int y){
	return x+y;
}
-> (int x, int y)->{return x+y;}
```

- 함수 이름 반환 형을 없애고 ->를 사용
- {}까지 실행문을 의미

### 람다식 문법

- 매개변수(하나인 경우) 자료형과 괄호 생략

`str -> {System.out.println(str);}`

- 매개 변수가 두 개인 경우 괄호를 색략할 수 없음
- 중괄호 안의 구현부가 한 문장인 경우 중괄호 생략

`str -> System.out.println(str);`

- 중괄호 안의 구현부가 한 문장이라도 return문은 중괄호 생략 X
- 중괄호 안의 구현부가 반환문 하나라면 return과 중괄호 모두 생략

`(x, y) -> x + y // str -> str.length()`

## 스트림 (stream)

- 자료의 대상과 관계 없이 동일한 연산을 수행
  - 배열, 컬렉션을 대상으로 동일한 연산을 수행
  - 일관성 있는 연산으로 자료의 처리를 쉽고 간단
- 한 번 생성하고 사용한 스트림은 재사용 할 수 없음
  - 자료에 대한 스트림을 생성하여 연산을 수행하면 스트림은 소모
  - 다른 연산을 위해서는 새로운 스트림을 생성
- 스트림 연산은 기존 자료를 변경 X
  - 자료에 대한 스트림을 생성하면 별도의 메모리 공간을 사용하므로 기존 자료 변경 x
- 스트림 연산은 중간 연산과 최종 연산으로 구분 됨
  - 스트림에 대해 중간 연산은 여러 개 적용될 수 있지만 최종 연산은 마지막에 한번만 적용
  - 최종연산이 호출되어야 중간연산의 결과가 모두 적용
  - 이를 '지연 연산'

```java
package stream;

import java.util.ArrayList;
import java.util.List;
import java.util.stream.Stream;

public class ArrayListTest {

	public static void main(String[] args) {

		List<String> sList = new ArrayList<String>();
		sList.add("Tomas");
		sList.add("James");
		sList.add("Edward");
		
		Stream<String> stream = sList.stream();
		stream.forEach(s->System.out.println(s));
		
		sList.stream().sorted().forEach(s->System.out.println(s));	
	}
}
```

### reduce() 연산

- 정의된 연산이 아닌 프로그래머가 직접 지정하는 연산을 적용
- 최종 연산으로 스트림의 요소를 소모하며 연산 수행
- 배열의 모든 요소의 합을 구하는 reduce() 연산

`Arrays.stream(arr).reduce(0,(a,b) -> a + b));`

- 두 번째 요소로 전달되는 람다식에 따라 다양한 기능을 수행

```java
package stream;

import java.util.Arrays;
import java.util.function.BinaryOperator;

class CompareString implements BinaryOperator<String>{

	@Override
	public String apply(String s1, String s2) {
		if(s1.getBytes().length <= s2.getBytes().length)
			return s1;
		else return s2;
	}
	
}

public class ReduceTest {

	public static void main(String[] args) {

		String[] greetings = {"안녕하세요~~~","hello","Good morning","반갑습니다"};
		
		System.out.println(Arrays.stream(greetings).reduce("", (s1, s2) ->{
			if(s1.getBytes().length >= s2.getBytes().length)
				return s1;
			else return s2;
		}		
				));
		
		String str =Arrays.stream(greetings).reduce(new CompareString()).get();
		System.out.println(str);
	}

}
```

## 예외 처리

### 오류

- 컴파일 오류(compile error) : 프로그램 코드 작성 중 발생하는 문법적 오류
- 실행 오류(reuntime error) : 실행 중인 프로그램이 의도 하지 않은 동작을 하거나(bug) 프로그램이 중지되는 오류
- 실행 오류 시 비정상 종료는 서비스 운영에 치명적
- 오류가 발생할 수 있는 경우, 로그를 남겨 추후 이를 분석하고 원인을 찾아야 함
- 자바는 예외 처리를 통하여 프로그램의 비정상 종료를 막고 log를 남길 수 있음

### 오류와 예외 클래스

- 시스템 오류(error) : 가상 머신에서 발생, 프로그래머가 처리 할 수 없음
- 예외 (Exception) : 프로그램에서 제어 할 수 있는 오류

### 예외 클래스의 종류

- 모든 예외 클래스의 최상위 클래스는 Exception
- 다양한 예외 클래스가 제공

### try-with-resources문

- 리소스를 자동 해제 하도록 제공해주는 구문
- close()를 명시적으로 호출하지 않아도 try{} 블록에서 열린 리소스는 정삭적인 경우, 예외 발생한 경우 모두 자동 해제 됨

### 사용자 정의 예외

- 
