# 22-12-07 Java Programming Beginners - Day 02

## Inflearn 자바 프로그래밍 입문

> 박은종

## 클래스와 객체

### this 가 하는 일

- 자신의 메모리를 가르킴
- 생성자에서 다른 생성자를 호출
- 자신의 주소를 반환

### 생성자 안에 다른 생성자 사용

```java
package thisex;

class Person{
	
	String name;
	int age;
	
	public Person() {
		this("이름없음",1);
	}
	
	public Person(String name, int age) {
		this.name = name;
		this.age = age;
	}
}

public class CallAnotherConst {

	public static void main(String[] args) {

		Person p1 = new Person();
		Person p2 = new Person("개똥이",24);
		System.out.println(p1.name);
		System.out.println(p2.name);
		
	}

}
```

### 객체 간의 협력

#### 객체 간의 협력 예

- 학생의 속성 : 학생이름, 돈, 학년
- 버스 : 버스 번호, 승객수, 돈
- 지하철 : 노선번호, 승객수, 돈

- 행위 : 버스를 탄다. , 지하철을 탄다

- 학생

```java
package coperation;

public class Student {

	String studentName;
	int grade;
	int money;
	
	public Student(String studentName, int money) {
		this.studentName = studentName;
		this.money = money;
	}
	
	public void takeBus(Bus bus) {
		bus.take(1000);
		money -= 1000;
	}
	
	public void takeSubway(Subway subway) {
		subway.take(1250);
		money -= 1250;
	}
	
	public void showInfo() {
		System.out.println(studentName + "님의 남은 돈은" + money + "입니다.");
	}
	
}

```

- 버스

```java
package coperation;

public class Bus {

	int busNumber;
	int passengerCount;
	int money;
	
	public Bus(int busNumber) {
		this.busNumber = busNumber;
	}
	
	public void take(int money) {
		passengerCount++;
		this.money += money;
	}
	
	public void showInfo() {
		System.out.println("버스" + busNumber + "번의 승객은" + passengerCount + "명이고, 수입은 " + money + "입니다.");
	}
}
```

- 지하철

```java
package coperation;

public class Subway {
	
	int lineNumber;
	int passengerCount;
	int money;
	
	public Subway(int lineNumber) {
		this.lineNumber = lineNumber;
	}
	
	public void take(int money) {
		passengerCount++;
		this.money += money;
	}
	
	public void showInfo() {
		System.out.println("지하철" + lineNumber + "의 승객은" + passengerCount + "명이고, 수입은 " + money + "입니다.");
	}
}
```

### static 변수

- 여러 개의 인스턴스가 같은 메모리의 값을 공유하기 위해 사용

> 예) 학생 클래스 -> Student James = new~ id: 1~;
>
> ​                            -> Student Tomas = new Stu~
>
> ​                           <James와 Tomas가 인스턴스>
>
> 인스턴스의 아이디값은 다르지만 공유하는 어떤 값을 static이라는 변수로 선언

- 정적 영역, 데이터영역 이라고 함
- 인스턴스가 생성될 때 마다 다른 메모리를 가지는 것이 아니라 프로그램이 메모리에 적재(load) 될때 데이터 영역의 메모리에 생성

- static 변수 또는 클래스 변수라고도 함

### static 메서드

- 클래스 메서드라고도 함
- static 메서드에서 인스턴스 변수(멤버 변수)를 사용 불가
