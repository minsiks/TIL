# 22-12-16 Java Programming Beginners - Day 04

## Inflearn 자바 프로그래밍 입문

> 박은종

## 이클립스에서 자바 디버깅

1. BreakPoint 설정 - debug 실행 - F5,F6,F7을 이용해서 조작

## 상속과 다형성

### 상속

- 클래스를 정의 할 때 이미 구현된 클래스를 상속 받아서 속성이나 기능을 확장되는 클래스를 구현함
- 상속하는 클래스 : 상위 클래스 , parent class, base class, super class
  - 하위 클래스 보다 일반적인 의미를 가짐
  - ex ) 포유류
- 상속 받는 클래스 : 하위 클래스, child class, derived class, subclass
  - 상위 클래스 보다 구체적인 의미를 가짐
  - ex ) 사람
- 클래스 상속 문법 : class B extends A{}

### 접근 제한자 가시성

![access](C:\Users\김민식\Documents\TIL\assets\access.png)

### 상속에서의 메모리 상태

- 상위 클래스의 인스턴스가 먼저 생서잉 되고, 하위 클래스의 인스턴스가 생성

### super 예약어

- this가 자기 자신의 인스턴스의 주소를 가지는 것처럼 super는 하위 클래스가 상위클래스에 대한 주소를 가지게 됨
- 하위 클래스가 상위 클래스에 접근 할 때 사용

### 상속 형변환

- 업캐스팅 
  - 상위 클래스로의 묵시적 형 변환
  - 하위 클래스는 상위 클래스의 타입을 내포하고 있으므로 상위 클래스로 묵시적 형 변환이 가능
  - Custoemr vc = new VIPCustomer();

## 오버라이딩과 다형성

### 매서드 오버라이딩

- 상위 클래스에 정의 된 메서드 중 하위 클래스와 기능이 맞지 않거나 추가 기능이 필요한 경우 같은 이름과 매개변수로 하위 클래스에서 재정의

### 가상 메서드(virtual method)

- 프로그램에서 어떤 객체의 변수나 메서드의 참조는 그 타입에 따라 이루어 짐
- 가상 메서드의 경우, 타입과 상관없이 실제 생성된 인스턴스의 메서드가 호출되는 원리

### 다형성(polymorphism)

- 하나의 코드가 여러가지 자료형으로 구현되어 실행
- 정보은닉, 상속과 더불어 객체지향 프로그램이 가장 큰 특징
- 객체지향 프로그래밍의 유연성, 재활용성, 유지보수성에 기본이 되는 특징

```java
package inheritance;

class Animal{
	public void move() {
		System.out.println("동물이 움직입니다.");
	}
}

class Human extends Animal{
	public void move() {
		System.out.println("사람이 두발로 걷습니다.");
	}
}

class Tiger extends Animal{
	public void move() {
		System.out.println("호랑이가 네발로 뜁니다/");
	}
}

class Eagle extends Animal{
	public void move() {
		System.out.println("독수리가 하늘을 날읍니다.");
	}
}
public class AnimalTest {
	
	public static void main(String[] args) {

		AnimalTest test = new AnimalTest();
		test.moveAnimal(new Human());
		test.moveAnimal(new Tiger());
		test.moveAnimal(new Eagle());
		
	}
	
	public void moveAnimal(Animal animal) {
		
		animal.move();
	}
	
}
```

## 다형성 활용과 다운캐스팅

### 상속 관계

- IS-A 관계 ( is a relationship : inheritance)
  - 일반 적인 개념과 구체적인 개념과의 관계
  - 상위 클래스 : 일반적인 개념 (예: 포유류)
  - 하위 클래스 : 구체적인 개념 (예: 사람, 원숭이, 고래 ..)
- HAS -A 관계(composition) : 한 클래스가 다른 클래스를 소유한 관계
  - ex_ 코드 재사용의 한 방법 Student가 Subject를 포함한 관계
    - class Student{ Subject majorSubject; }

### 다운 캐스팅 - instanceof

- 다시 원래 자료 형인 하위 클래스로 형 변환하려면 명시적으로 다운 캐스팅을 해야함
  - instanceof

```java
package inheritance;

import java.util.ArrayList;

class Animal{
	public void move() {
		System.out.println("동물이 움직입니다.");
	}
}
class Human extends Animal{
	public void move() {
		System.out.println("사람이 두발로 걷습니다.");
	}
	
	public void readBook() {
		System.out.println("사람이 책을 읽습니다.");
	}
}
class Tiger extends Animal{
	public void move() {
		System.out.println("호랑이가 네발로 뜁니다/");
	}
	public void hunting() {
		System.out.println("호랑이가 사냥을 합니다.");
	}
}
class Eagle extends Animal{
	public void move() {
		System.out.println("독수리가 하늘을 날읍니다.");
	}
	public void flying() {
		System.out.println("독수리가 하늘을 날아요");
	}
}
public class AnimalTest {
	public static void main(String[] args) {
		AnimalTest test = new AnimalTest();
		test.moveAnimal(new Human());
		test.moveAnimal(new Tiger());
		test.moveAnimal(new Eagle());
	}
	public void moveAnimal(Animal animal) {
		
		animal.move();
		
		if(animal instanceof Human) {
			Human human = (Human) animal;
			human.readBook();
		}else if (animal instanceof Tiger) {
			Tiger tiger = (Tiger)animal;
			tiger.hunting();
		}else if (animal instanceof Eagle) {
			Eagle eagle = (Eagle)animal;
			eagle.flying();
		}else {
			System.out.println("지원되지 않는 기능입니다.");
		}
	}
}
```

## 추상 클래스

### 추상 클래스(abstract class)

- 추상 메서드를 포함한 클래스
- 추상 메서드는 구현코드 없이 메서드의 선언만 있음
  - ex_ abstract int add(int x, int y); // 선언만 있는 추상 메서드
  - int add(int x, int y){ } // {} 부분이 구현 내용, 추상 메서드 X
- abstract 예약어 사용
- 추상 클래스는 new (인스턴스 화) 할 수 없음
