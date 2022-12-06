# 22-12-06 Java Programming Beginners - Day 01

## Inflearn 자바 프로그래밍 입문

> 박은종

### 자바의 장점

- 플랫폼에 영향을 받지 않으므로 다양한 환경에서 사용
- JVM(자바 가상 머신)이 있다면 한번 컴파일러를 거쳐 모든 환경에 적용 가능
- 객체 지향 언어이기 때문에 유지보수가 쉽고 확장성이 좋다
- 안정적 프로그램
- 풍부한 기능을 제공하는 오픈 소스

## 변수와 자료형

### 데이터 표현

- 0과 1로 데이터를 저장
- bit : 컴퓨터가 표현하는 데이터의 최소 단위, 2진수 하나의 값을 저장할 수 있는 메모리의 크기
- byte : 1byte = 8bit
- 2진수
  - 숫자나 문자도 0과 1의 조합으로 표현된다.
- 16진수
  - 10 = A, 11 = B, 12 = C, 13 = D, 14 = E, 15 = F, 16 = 10

```java
package first;

public class BinaryTest {

	public static void main(String[] args) {
		
		int num = 10;
		int bNum = 0B1010; //2진수는 앞에 0B 작성
		int oNum = 012; // 0가 앞에있으면 8진수
		int hNum = 0XA; // 0X는 16진수
		
		System.out.println(num);
		System.out.println(bNum);
		System.out.println(oNum);
		System.out.println(hNum);		
		//모두 10 
	}

}
```

- 음의 정수

  - 정수의 가장 왼쪽에 존재하는 비트는 부호비트
    - MSB(Most Significant Bit) 가장 중요한 비트
  - 가장 왼쪽 비트가 0이라면 양수, 1이라면 음수

  - 음수를 만드는 방법은 양수의 2의 보수를 취한다.

```java
package first;

public class BinaryTest2 {

	public static void main(String[] args) {
		
		int num1 = 0B00000000000000000000000000000101; //5
		int num2 = 0B11111111111111111111111111111011; //-5	
		System.out.println(num1);
		System.out.println(num2);
	}
}
```

### 변수

- 프로그램에서 사용되는 자료를 저장하기 위한 공간
- variable

- 변수가 저장되는 공간의 특성 - 자료형
  - 정수형, 문자형, 실수형, 논리형

- byte 와 short

  - byte : 1바이트 단위의 자료형
    - 동영상,음악 파일 등 실행 파일의 자료를 처리 할 때 사용하기 좋은 자료형
  - short : 2바이트 단위의 자료형, 주로 c/c++ 언어와의 호환 시 사용

- int

  - 자바의 정수의 기본 자료형
  - 4바이트

- long

  - 8바이트, 가장 큰 정수 자료형

  - 숫자의 뒤에 L또는 l을 써서 long 형임을 표시해야 함

- char - 문자 자료형

  - 문자도 내부적으로는 비트의 조합으로 표현

- 문자 세트

  - 아스키(ASCII) : 1 바이트로 영문자, 숫자, 특수문자 등을 표현
  - 유니코드 (Unicode) : 한글과 같은 복잡한 언어를 표현하기 위한 표준 인코딩 UTF-8, UTF-16이 대표적

```java
package first;

public class VariableEx {

	public static void main(String[] args) {
		char ch = 'A';
		
		System.out.println(ch);
		System.out.println((int)ch);
		
		ch = 66;
		
		System.out.println(ch);
		
		int ch2 = 67;
		System.out.println(ch2);
		System.out.println((char)ch2);
		
	}

}
```

- float, double - 실수 자료형

- boolean - 논리형
  - true,false를 나타냄

- 자료형 없이 변수 사용하기
  - var

### 상수

- 상수 : 변하지 않는 값
- 상수를 선언 : final 키워드 사용

### 리터럴(literal)

- 프로그램에서 상용하는 모든 숫자, 값, 논리

- 리터럴에 해당되는 값은 특정 메모리 공간인 상수 풀에 있음

### 형 변환(type conversion)

- 묵시적 형변환 : 작은 수에서 큰 수로 덜 정밀한 수에서 더 정밀한 수로 대입되는 경우
- 명시적 형 변환 : 반대의 경우

## 자바의 여러 가지 연산자

### 항과 연산자

- 항(operand) : 연산에 사용되는 값
- 연산자(operator) : 항을 이용하여 연산하는 기호

### 대입 연산자

- 왼쪽 변수(lvalue)에 오른쪽 변수(값) (rvalue)를 대입

### 부호 연산자

- +, -  : 양수/ 음수의 표현, 값의 부호를 변경
- 변수에 +,- 를 사용한다고해서 변수의 값이 변하지 X

```java
int n = 10;
int n2 = -n; // n2 = -10 그러나 n은 여전히 10

int n = -n; // n을 음수로 바꾸고싶으면 대입연산자 사용
```

### 산술 연산자

- +,-,*,/,%

### 증가 감소 연산자

- ++, --

```java
int num = 10;
System.out.println(++num); // 10 화면에 나온후 11로 변환
System.out.println(num++); // 11부터 나오고 11로 변환
```

### 관계 연산자

- *>*,<,>=,<=,==,!=

- 연산의 결과가 true, false로 반환

### 논리 연산자

- &&, ||, !
- 연산의 결과가 true, false로 반환 됨

### 단락 회로 평가

```java
package first;

public class OperationEx {

	public static void main(String[] args) {
		int num1 = 10;
		int i = 2;

		boolean value = ((num1 = num1 + 10) > 10 ) || ((i = i+2) > 10 );
		System.out.println(value);
		System.out.println(num1);
		System.out.println(i);
		
	}

}
```

- 논리합, 논리곱에 따라 먼저 처리 되면 뒤에는 보지도 않음

### 복합 대입 연산자

- +=,-=,*=,/=,%=,<<=,>>=,>>>=,&=,|=,^=
- 대입 연산자와 다른 연산자를 함께 사용함

### 조건 연산자

- 삼항 연산자
  - 조건식 ? 결과1 : 결과2;

### 비트 연산자

- ~,&,|,^,<<,>>,>>>

### 비트 연산자의 활용

```java
int num3 = 5;
		System.out.println(num3 << 1);// <<1은 num3곱하기 2의1승
		System.out.println(num3 << 5);// <<5은 num3곱하기 2의5승

```

### 조건문

- 주어진 조건에 따라수행문이 실행되도록 프로그래밍 하는것

- if문, if else문, if-else if-else 문 (else if)

### 조건문과 조건 연산자

- 간단한 if-else 조건문은 조건 연산자로 구현 가능

### switch-case문

```java
/*
		int rank = 0;
		char medalcolor = G;
		
		switch(medalcolor) { // 케이스에 문자도 사용가능  
		case 'G' : rank = 1;
				break;
		case 'S' : rank = 2;
				break;
		case 'B' : rank = 3;
				break;
		default : rank = 4;
		}
		System.out.println(rank);
		*/
		
		int month = 10;
		int day = 0;
		
		switch (month) {
		case 1: case 3: case 5: case 7: case 8: case 10: case 12: // 케이스 결과가 같을시 같이 사용 가능
			day = 31;
			break;
		case 2:
			day = 28;
			break;
		case 4: case 6: case 9: case 11:
			day = 30;
			break;
		default:
			break;
		}
		System.out.println(month + "월은" + day + "일 까지 있습니다.");
```

### 반복문

- 주어진 조건이 만족 할 때까지 수행문을 반복적으로 수행함
- while, do-while, for

- while문
  - 무한반복할때 많이 사용 while(true){}

```java
int num = 1;
int sum = 0;

while(num <= 10) {
    sum += num;
    num++;
}
System.out.println(sum);
	
```

- do-while문

```java
int num = 1;
int sum = 0;

do{
    sum += num;
    num++;
}while(num < 1); //do-while은 조건식이 아래 한번은 무조건 실행

System.out.println(sum);
```

- for 문
  - 가장 많이 사용하는 반복문
  - 초기화식, 조건식, 증감식을 한꺼번에 작성

### 중첩된 반복문

- 반복문 내부에 또 반복문이 사용

```java
for(int i=1; i<=9; i++) {
    System.out.println(i+"단 시작");
    for(int j=1; j<=9; j++) {
        System.out.println(i+" * " + j +" = " + (i*j));
    }
}
```

### continue 문

- 반복문과 함게 쓰이며, 반복문 내부 continue 문을 만나면 이후 반복되는 부분을 수행하지 않고 조건식이나 증감식을 수행

```java
int total = 0;
int num;

for(num = 1; num<=100; num++) {
    if((num%2)==0) {
        continue;
    }
    total += num;
}
System.out.println(total);
```

### break 문

- break 문을 만나면 더 이상 반복을 수행하지 않고 반복문을 빠져 나옴

```java
int sum = 0;
int num = 1;

while(true){
    sum += num;

    if(sum>100)
        break;
    num++;
}

System.out.println(sum);
System.out.println(num);
```

## 클래스와 객체

### 객체 지향프로그래밍과 클래스

- 객체
  - 의사나 행위가 미치는 대상 - 사전적 의미
- 객체지향 프로그래밍 : 객체를 기반으로 하는 프로그래밍

### 클래스

- 객체에 대한 속성과 기능을 코드로 구현 한 것
- 객체에 대한 청사진(blueprint)
- 객체의 속성
  - 객체의 특성 , 속성 멤버 변수
- 객체의 기능
  - 객체가 하는 기능들을 메서드로 구현

### 클래스 정의

```java
(접근 제어자) class 클래스 이름{
	멤버 변수;
	메서드;
}
```

- 학생 클래스의 예
  - 속성 : 학번, 이름, 학년, 사는 곳 등등
  - 기능 : 수강신청, 수업듣기, 시험 보기 등

> class 는 대부분 대문자로 시작
>
> public이라는 클래스는 하나

### 메서드

- 함수의 일종
- 객체의 기능을 제공하기 위해 클래스 내부에 구현되는 함수
- 함수
  - 하나의 기능을 수행하는 일련의 코드

```java
package classpart;

public class FunctionTest {

	public static void main(String[] args) {
		int num1 = 10;
		int num2 = 20;
		
		int sum = addNum(num1, num2);
		
		System.out.println(sum);
	}
	
	public static int addNum(int n1, int n2<-매개변수) {
		int result = n1 + n2;
		return result;
	}
}
```

### 함수와 스택 메모리

- 함수가 호출될 때 사용하는 메모리 - 스택(stack)

### 클래스와 인스턴스

- 클래스 (static 코드) ->(생성, 인스턴스화) -> 인스턴스 (dynamic memory)
  - 예) 클래스가 개라면, 인스턴스는 진돗개
  - 클래스 생성하려면 new 생성자

### 인스턴스와 힙(heap) 메모리

- 인스턴스는 힙메모리에 생성

```java
package classpart;

public class Student {
	
	int studentID;
	String studentName;
	int grade;
	String address;
	
	public void showStudentInfo() {
		System.out.println(studentName + "," + address);
	}
	
	public String getStudentName() {
		return studentName;
	}
	
	public void setStudentName(String name) {
		studentName = name;
	}
	
	public static void main(String[] args) {

		Student studentLee = new Student();
		studentLee.studentName = "이순신";
		studentLee.studentID = 100;
		studentLee.address = "서울시 영등포구 여의도동";
		
		Student studentKim = new Student();
		studentKim.studentName = "김유신";
		studentKim.studentID = 101;
		studentKim.address = "미국 산호세";
		//studentkim.* <- 참조 변수
        
		studentLee.showStudentInfo();
		studentKim.showStudentInfo();		
	}
}
```

### 생성자 (constructor)

```java
public class Student {
	
	int studentID;
	String studentName;
	int grade;
	String address;
	
	public Student() {
		
	} // 디폴트 생성자
	
	public Student(int id, String name) {  // 생성자 오버로드
		studentID = id;
		studentName = name;
	}
	
```

### 참조 자료형 (reference data type)

- 클래스 형으로 선언하는 자료형
  - String, Date, Student 등

### 정보 은닉 (information hiding)

- private 접근 제어자

  - 클래스 외부에서 접근을 못함

  - setter로 세팅
