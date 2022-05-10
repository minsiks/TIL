# 4/11 Java Day5 - (1)



## day 1~5 복습

### Ch.01 자바 시작하기

#### I) 프로그래밍 언어

- 이진코드(0과 1)로 이루어진 기계어의 다리 역할을 한다.
- 고급언어
  - 컴퓨터와 대화할 수 있도록 만든 언어 중에서 사람이 쉽게 이해할 수 있는 언어
  - 컴파일 (compile) 과정을 통해서 컴퓨터가 이해할 수 있는 기계어로 변환한 후 컴퓨터가 사용
- 저급언어
  - 기계어에 가까운 언어
  - 대표적으로 어셈블리어
  - 다루기가 매우 까다롭다
- 일반적으로 프로그래밍 언어라고 하면 고급 언어를 말함
- 대표적으로 C++,자바(Java)
- 작성된 내용은 소스 (source)라고 하며 이 소스는 컴파일러(compiler)라는 소프트웨어에 의해 기계어로 변환된 후 컴퓨터에서 실행할 수 있게 된다. 

#### II) 자바

##### 2.1 자바 소개
- 썬 마이크로시스템즈 (Sun Microsystems)에서 자바(Java) 언어를 발표
- 1991년 썬의 엔지니어들에 의해서 고안된 오크(Oak)라는 언어에서부터 시작
- 처음에는 가전제품에서 사용될 목적
- 현재는 스마트폰을 비롯해서 각종 장비와 앱, 금융, 공공, 대기업 등의 엔터프라이즈[^1]기업 환경에서 실행되는 서버 애플리케이션을 개발하는 중추적인 언어로 자리매김
[^엔터프라이즈]: 지정된 작업을 수행하는 조직이나 기업
##### 2.2 자바 특징

- (1) 이식성이 높은 언어
  - 이식성이란 서로 다른 실행 환경을 가진 시스템 간에 프로그램을 옮겨 실행할 수 있는 것
  - 예) MS 윈도우 (Microsoft Windows)에서 실행하는 프로그램을 리눅스 또는 유닉스에서 실행 가능
  - 자바 실행 환경 (JRE : Java Runtime Environment)이 설치되어 있는 모든 운영체제에서 실행 가능

- (2) 객체 지향 언어
  - 부품에 해당하는 객체들을 먼저 만들고, 이것들을 하나씩 조립 및 연결해서 전체 프로그램을 완성하는 기법에 사용되는 언어
  - 자바는  100% 객체 지향 언어
  - 클래스 : 설계도
  - 아무리 작은 프로그램이라도 객체를 만들어 사용
  - 캡슐화, 상속, 다형성 기능을 완벽하게 지원
- (3) 함수적 스타일 코딩 지원
  - 대용량 데이터의 병렬 처리, 이벤트 지향 프로그래밍을 위해 적합
  - 람다식 (Lambda Expressions)을 자바 8부터 지원
- (4) 메모리를 자동으로 관리
  - C++은 메모리에 생성된 객체를 제거하기 위해 개발자가 직접 코드를 작성해야 함
  - 자바는 개발자가 직접 메모리에 접근 할 수 없도록 설계, 메모리는 자바가 직접 관리
  - 객체 생성 시 자동으로 메모리 영역을 찾고 사용이 완료된면 쓰레기 수집기 (Garbage Collector)를 실행시켜 자동적으로 사용하지 않는 객체를 제거시켜준다.
- (5) 다양한 애플리케이션을 개발할 수 있다.
  - 다양한 운영체제에서 실행되는 프로그램을 개발할 수 있다.
  - 단순한 콘솔 프로그램부터 클라이언트용 윈도우 애플리케이션, 서버용 웹 애플리케이션 그리고 모바일용 안드로이드 앱에 이르기까지 거의 모든 곳에서 실행되는 프로그램을 개발 가능
  - 자바는 다양한 운영체제에서 사용할 수 있는 개발 도구와 API를 묶어 에디션(Edition) 형태로 정의
    - Java SE (Standard Edition) - 기본 에디션
      - 자바 프로그램들이 공통적으로 사용하는 자바 가상 기계를 비롯해서 자바 프로그램 개발에 필수적인 도구와 라이브러리 API를 정의.
    - Java EE (Enterprise Edition) - 서버용 애플리케이션 개발 에디션
- (6) 멀티 스레드(Multi-Thread)를 쉽게 구현 가능
  - 대용량 작업을 빨리 처리하기 위해 서브 작업으로 분리해서 병력 처리하기 위한 프로그래밍
- (7) 동적 로딩(Dynamic Loading) 지원
  - 객체가 필요한 시점에 클래스를 동적 로딩해서 객체를 생성
  - 따라서 유지보수를 쉽고 빠르게 진행 할 수 있다.
- (8) 막강한 오픈소스 라이브러리
  - 자바는 오픈소스 (Open Source)  언어
  - 개발 기간 단축
  - 안전성이 높은 애플리케이션을 쉽게 개발 가능
##### 2.3 자바 가상 기계 (JVM)

  - 운영체제는 자바 프로그램을 바로 실행 불가능	
    - Why : 자바 프로그램은 완전한 기계어가 아닌, 중간 단계의 바이트 코드이기 때문
  - 그렇기에 자바 가상 기계(JVM: Java Virtual Machine)이 필요하다.
  - 자바를 실행하는 가상의 운영체제 역할
  - 바이트 코드는 모든 JVM에서 동일한 실행결과를 보장하지만, JVM은 운영체제에 종속적이다.
  - JVM은 운영체제에 맞게 설치되어야 한다.
  - **소스파일 (.java) -> 컴파일러(javac.exe) -> 바이트 코드 파일(.class)-> JVM 구동 명령어 (java.exe) -> 각각 운영체제  -> 기계어**
  - Write once, run anywhere (한 번 작성하면 어디서든 실행된다)
  - JVM에 의해 기계어로 번역되고 실행되기 때문에, C와 C++보다는 컴파일 단계에서 속도가 느리다.

#### III) 자바 개발 환경 구축

##### 3.1 자바 개발 도구 설치

- 먼저 Java SE(Standard Edition)의 구현체인 JDK를 설치
- Java SE의 구현체
  - 자바 개발 키트 (JDK : Java Development Kit)
    - 프로그램 개발에 필요한 자바 가상기계(JVM), 라이브러리 API, 컴파일러 등 개발 도구가 포함
    - : JRE + 개발에 필요한 도구

  - 자바 실행 환경 (JRE : Java Runtime Environment)
    - 프로그램 실행에 필요한 자바 가상 기계(JVM), 라이브러리 API만 포함
    - 개발 하고자 하는 것이 아니고, 이미 개발된 프로그램만 실행
    - : JVM+표준 클래스 라이브러리

- JDK는 [오라클](http://www.oracle.com) 사이트에서 무료로 다운로드 받을 수 있다.
- 자바 8(JDK 1.8) 이상의 설치 파일을 다운로드
- 기본위치는 `C:\Progrm Files\Java`

##### 3.2 API 도큐먼트

- JDK에서 제공하는 표준 클래스 라이브러리
- Application Programming Interface
- JDK에 포함
- http://docs.oracle.con/javase/8/docs/api/

#### IV) 자바 프로그램 개발 순서

##### 4.1 소스 작성부터 실행

- .java 소스 파일 작성 -> 컴파일러(javac.exe)로 바이트 코드 파일(.class) 생성 -> JVM 구동 명령어 (jave.exe)로 실행

##### 4.2 프로그램 소스 분석

- 자바 실행 프로그램은 반드시 클래스(class) 블록과 main () 메소드(method) 블록으로 구성되어야 한다.

- 메소드 블록은 단독으로 작성될 수 없고 항상 클래스 블록 내부에서 작성되어야 한다.

  - 클래스 : 필드 또는 메소드를 포함하는 블록
  - 메소드 : 어떤 일을 처리하는 실행문을 모아 놓은 블록

- ```java
  public class Ws04 {
  }
  ```

  - Ws04가 클래스 이름 
  - 중괄호는 클래스 블록
  - 클래스의 이름은 개발자가 마음대로 정할 수 있다.(숫자로 시작 및 공백 포함 X)

- ```java
  package ch01;
  
  public class Hello {
  
  	public static void main(String[] args) {
  		System.out.println("Hello, welcome to the java world!");
  
  	}
  
  }
  
  ```

  - main이 메소드 이름
  - 그 아래 중괄호가 메소드 블록
  - 메소드 이름도 개발자가 마음대로 정할 수 있지만 main() 메소드 만큼은 다른 이름으로 바꾸면 안된다.
  - main() 메소드를 프로그램 실행 진입점(entry point)이라고 한다.
  - main() 메소드가 없는 클래스를 java.exe로 실행시키면 에러 메세지가 나타난다.

#### V) 주석과 실행문

##### 5.1 주석 사용하기

-  주석 : 프로그램 실행과는 상관없이 코드에 설명을 붙이는 것
- 컴파일 과정에서 주석은 무시되고 실행문만 바이트 코드로 변역
- 가급적 설명이 필요한 코드에 주석을 달아 두는 것이 좋음
- 주석기호 : // (행 주석) , /* ~*/ : 범위 주석
- 문자열("") 내부에는 올 수 없다.

  ##### 5.2 실행문과 세미콜론(;)

- 실행문 끝에는 반드시 세미콜론(;)을 붙여야 한다.

#### VI) 이클립스 설치

##### 6.1 이클립스 소개

- 개발자의 코딩 실수를 줄여주기 위한 키워드의 색깔 구분, 자동 코드 완성 기능 및 디버깅[^2] 기능을 갖춘 소스 편집 툴

[^2 디버깅(debugging)]: 모의 실행을 해서 코드의 오류를 찾는 것

- 2003년도 IBM에서 개발
- 자바 프로그램을 개발하기 위한 통합 개발 환경 (IDE : Integrated Development Environments)
- 프로젝트 생성, 자동 코드 완성, 디버깅 기능을 가지고 있다.
- 오픈소스 개발 플랫폼으로 무료로 제공
- 기업체에서 가장 선호하는 개발 전문 툴

##### 6.2 이클립스 다운로드

- 이클립스 압축 파일 "http://www.eclipse.org"사이트에서 무료로 다운 가능
- [Eclipse IDE for Enterprise Java and Web Developers](https://www.eclipse.org/downloads/packages/release/2022-03/r/eclipse-ide-enterprise-java-and-web-developers) 다운

##### 6.3 워크스페이스

- 생성한 프로젝트가 기본적으로 저장되는 디렉토리

##### 6.4 퍼스펙티브와 뷰

- 퍼스펙티브(Perspective)는 이클립스에서 프로젝트를 개발할 때 유용하게 사용하는 뷰(view)들을 묶어 놓은것을 말한다.

##### 6.5 프로젝트 생성

- JavsSE-1.8로 설정
- day01에 해당

##### 6.6 소스 파일 생성과 컴파일

##### 6.7 바이트 코드 실행

------------------------

### Ch.02 변수와 타입

#### I) 변수

##### 1.1 변수란

- 변수는 값을 저장할 수 있는 메모리의 공간
- 프로그램에 의해서 수시로 값이 변동될 수 있다.
- 변수에는 복수 개의 값을 저장할 수 없고, 하나의 값만 저장할 수 있다.
- 다양한 타입의 값을 저장할 수 없고 한 가지 타입의 값만 저장 가능
  - 정수 타입 변수에는 정수값만, 실수 타입 변수에는 실수 값만

##### 1.2 변수의 선언

- ```java
  int <변수이름>     //정수(int)값을 저장할 수 있는 age 변수 선언
  double <변수이름>  //실수(double)값을 저장할 수 있는 value 변수 선언
  int x, y, z;
  ```

- 같은 타입의 변수는 콤마(,)를 이용해서 한꺼번에 선언할 수도 있다.

- 변수 명명 규칙
  - 첫 번째 글자는 문자이거나 '$','_'이어야 하고 숫자로 시작할 수 없다 (필수)
  - 영어 대소문자가 구분된다(필수)
  - 첫 문자는 영어 소문자로 시작하되, 다른 단어가 붙을 경우 첫 문자를 대문자로 한다.(관례)
  - 문자 수(길이)의 제한은 없다
  - 자바 예약어는 사용할 수 없다(필수)

##### 1.3 변수의 사용

- 변수에 값을 저장하고 읽는 행위

- (1) 변수값 저장

  - 변수에 값을 저장할 때에는 대입 연산자(=)를 사용

    - ```java
      int score; //변수 선언
      score = 90; //값 저장
      int score = 90;//변수 선언, 값 저장 동시 가능
      ```

    - 소스 코드 내에서 입력된 값을 리터럴(literal)이라고 부른다.

      - 정수 리터럴 : byte, char, short, int, long
      - 실수 리터럴 : float, double
      - 문자 리터럴 : 작은 따옴표로 묶음 char 하나
        - \가 붙은 문자 리터럴은 이스케이프(escape) 문자라고도 한다.
      - 문자열 리터럴 : 큰 따옴표로 묶음 String 하나
      - 논리 리터럴 : true와 false boolean 하나

- (2) 변수값 읽기
  - 변수는 초기화가 되어야 읽을 수가 있고, 초기화가되지 않은 변수는 읽을 수가 없다.
  - `int a = 0;`

##### 1.4 변수의 사용 범위

- 변수는 중괄호 {} 블록 내에서 선언되고 사용된다.

#### II) 데이터 타입

- 모든 변수에는 타입(type: 형(形))이 있으며, 타입에 따라 저장할 수 있는 값으 종류와 범위가 달라진다.
- 변수를 선언할 때 주어진 타입은 변수를 사용하는 도중에 변경할 수 없다.

##### 2.1 기본(원시: primitive) 타입

- 정수, 실수, 문자, 논리 리터럴을 직접 저장하는 타입

  - | 값의종류 | 기본 타입 | 메모리 | 사용 크기 | 저장되는 값의 범위           |
      | -------- | --------- | ------ | --------- | ---------------------------- |
      | 정수     | byte      | 1 byte | 8 bit     | -128~127                     |
      |          | char      | 2 byte | 16 bit    | 0~65535                      |
    |          | short     | 2 byte | 16 bit    | -32,768~32,767               |
    |          | int       | 4 byte | 32 bit    | -2,147,483,648~2,147,483,647 |
    |          | long      | 8 byte | 64 bit    | -2의 64승 ~(2의 64승 -1)     |
    | 실수     | float     | 4 byte | 32 bit    | 많음                         |
    |          | double    | 8 byte | 64 bit    | 많음                         |
    | 논리     | boolean   | 1 byte | 8 bit     | true, false                  |

##### 2.2 정수 타입 (byte, char, short, int, long)

- 자바는 기본적으로 정수 연산을 int 타입으로 수행

##### 2.3 실수 타입(float, double)

- 소수점이 있는 실수 데이터를 저장할 수 있는 타입

##### 2.4 논리 타입(boolean0)

- 논리값(true/false)을 저장할 수 있는 데이터 타입

#### III) 타입 변환

- 데이터 타입을 다른 데이터 타입으로 변환
- 예를 들어 byte 타입을 int 타입으로 변환
- 두가지 종류가 있다
  - 자동(묵시적) 타입 변환
  - 강제(명시적) 타입 변환

##### 3.1 자동 타입 변환

- 자동 타입 변환(Promotion)은 프로그램 실행 도중에 자동적으로 타입 변환이 일어나는 것을 말한다.
- 작은 크기를 가지는 타입이 큰 크기를 가지는 타입에 저장될 때 발생

##### 3.2 강제 타입 변환

- 큰 데이터 타입을 작은 데이터 타입으로 쪼개어서 저장하는 것

- 캐스팅(Casting)이라고 함

- 캐스팅 연산자 ()를 사용

- ```java
  int intValue = 103089770;
  byte byteValue = (byte) intValue; // 강제 타입 변환(캐스팅)
  ```

- 주의할 점 

  - 사용자로부터 입력받은 값을 변환할 대 값의 손실이 발생하면 안된다.
  - 정수 타입을 실수 타입으로 변환할 때 정밀도 손실을 피해야 한다.

##### 3.3 연산식에서의 자동 타입 변환

- 서로 다른 타입의 피연산자가 있을 경우 두 피연산자 중 크기가 큰 타입으로 자동 변환

### Ch 03 연산자

#### I) 연산자와 연산식

- 연산(operations) : 프로그램에서 데이터를 처리하여 결과를 산출하는 것
- 연산자(operator) : 연산에 사용되는 표시나 기호
- 피연산자(operand) : 연산되는 데이터
- 연산식(expressions) : 연산자와 피연산자를 이용하여 연산의 과정을 기술한 것
- 필요로 하는 피연산자의 수에 따라 단항, 이항, 삼항 연산자로 구분된다.

#### II) 연산의 방향과 우선순위

- &&보다는 >,<가 우선순위가 높다.

#### III) 단항 연산자

##### 3.1 부호 연산자(+,-)

##### 3.2 증감 연산자(++,--)

##### 3.3 논리 부정 연산자(!)

##### 3.4 비트 반전 연산자(~)

- 정수 타입의 피연산자에만 사용되며, 피연산자를 2진수로 표현했을 때 비트값인 0을 1로, 1은 0으로 반전한다.

#### IV) 이항 연산자

##### 4.1 산술 연산자(+,-,*,/,%)

- (1) 오버플로우 탐지

  - 산술 연산을 할 때 연산후의 산출값이 표현할 수 없는 산출 타입이라면 오버플로우가 발생

- (2) 정확한 계산은 정수 사용

  - 정확한 계산이 필요하다면 정수 연산으로 변경해서 계산 해야한다.

- (3) NAN과 Infinity 연산

  - / 또는 %연산자를 사용할 때 주의

  - ```java
    5/0.0 -> infinity //무한대
    5%0.0 -> NAN    // Not a Number
    ```

  - 결과가 Infinity 또는 NaN이 나오면 다음 연산을 수행해서는 안된다.

  - /와 % 연산의 결과가 Infinity 또는  NaN인지 확인하려면 Double.isInfinite ()와 Double.inNaN() 메소드를 이용하면 된다.

- (4) 입력값의 NaN검사

  - 부동소수점(실수)을 입력받을 때는 반드시 NaN 검사를 해야 한다.

##### 4.2 문자열 연결 연산자(+)

- 문자열 연결 연산자인 +는 문자열을 서로 결합하는 연산자이다.

- ```java
  String str1 = "JDK"+6.0; // JDK6.0
  String str2 = str1 + "특징"; // JDK6.0특징
  ```

##### 4.3 비교 연산자 (<,<=,>,>=,==,!=)

##### 4.4 논리 연산자 (&&, ||, &, |, ^, !)

- 논리곱 (&&), 논리합 (||), 배타적 논리합(^), 논리 부정(!)

##### 4.5 비트 연산자(&,|,^,~,<<,>>,>>>)

- 비트 연산자는 데이터를 비트(bit) 단위로 연산한다.
- 즉 0과 1이 피연산자가 된다.
  - 비트 논리 연산자(&,|,^,~)
  - 비트 이동 연산자(<<,>>,>>>)

##### 4.6 대입 연산자(=,+=,-=,8=,/=,%=,&=,^=,|=,<<=,>>=,>>>=)

- 대입 연산자는 오른쪽 피연산자의 값을 좌측 피연산자인 변수에 저장한다.
  - 단순 대입 연산자 : 단순히 오른쪽 피연산자의 값을 변수에 저장
  - 복합 대입 연산자 : 정해진 연산을 수행한 후 결과를 변수에 저장

#### V) 삼항 연산자

- 삼항 연산자(?:)는 세 개의 피연산자를 필요로 하는 연산자
- 조건 연산식
- 조건식(피연산자1) ? 값 또는 연산식 (피연산자2) : 값 또는 연산식 (피연산자3)

### Ch04 조건문과 반복문

#### I) 코드 실행 흐름 제어

- 흐름 제어문 : 실행 흐름을 개발자가 원하는 방향으로 바꿀 수 있도록 해주는 것

#### II)조건문

##### 2.1 if문

- 조건식 결과에 따라 블록 실행 여부가 결정된다.

##### 2.2 if-else문

- else 블록과 함께 사용되어 조건식의 결과에 따라 실행 블록을 선택

##### 2.3 if-else if-else문

- 조건문이 여러 개인 if문

##### 2.4 중첩 if문

- if문의 블록 내부에 또 다른 if 문 사용

##### 2.5 switch 문

- 값과 동일한 값을 갖는 case로 가서 실행문을 실행시킨다.

- ```java
  public static void main(String[] args) {
  		int num = (int)(Math.random()*6)+1;
  		
  		switch(num) {
  		case 1:
  			System.out.println("1번이 나왔습니다.");
  			break;
  		case 2:
  			System.out.println("2번이 나왔습니다.");
  			break;
  		case 3:
  			System.out.println("3번이 나왔습니다.");
  			break;
  		case 4:
  			System.out.println("4번이 나왔습니다.");
  			break;
  		case 5:
  			System.out.println("5번이 나왔습니다.");
  			break;
  		case 6:
  			System.out.println("6번이 나왔습니다.");
  			break;
  		}
  
  	}
  ```

- char 타입 변수도 switch문에 사용될 수 있다.

#### III) 반복문(for문, while문, do-while문)

- 어떤 작업(코드들)이 반복적으로 실행되도록 할 때 사용

##### 3.1  for문

- 주어진 횟수만큼 실행문을 반복 실행할 때 적합

##### 3.2 while문

- 조건식이 true일 경우에 계속해서 반복

##### 3.3 break문

- 반목문을 실행 중지할 때 사용
- break문은 가장 가까운 반복문만 종료하고 바깥쪽 반복문은 종료시키지 않는다.
- 중첩된 반복문에서 바깥쪽 반복문까지 종료시키려면 바깥쪽 반복문에 이름(라벨)을 붙이고, "break 이름;"을 사용하면 된다.

##### 3.4 continue문

- 반복문 블록 내부에서 continue문이 실행되면 for문의 증감식 또는 while의 조건식으로 이동
- 반복문을 종료하지 않고 계속 반복을 수행
- 특정 조건을 만족하는 경우 continue문을 실행하여 그 이후의 문장을 실행하지 않고 다음 반복으로 넘어간다.