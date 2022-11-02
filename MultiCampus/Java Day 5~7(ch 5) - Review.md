# 4/16 Java Day7 - (1)



## day 5 ~ 7 복습

### Ch.05 참조 타입

#### I) 데이터 타입 분류

- 기본 타입(원시 타입 : primitive type)
  - 정수, 실수, 문자, 논리 리터럴을 저장
- 참조 타입(reference type)
  - 배열, 열거, 클래스, 인터페이스

### II) 메모리 사용 영역

- JVM이 사용하는 메모리 영역
  - 메소드 영역(Method Area)
  - 스레드
  - 힙 영역(Heap Area)

#### 2.1 

- 메소드(Method) 영역

- Jvm이 시작할 때 생성되고 모든 스레드가 공유하는 영역
- 힙(heap영역)
  - 객체와 배열이 생성되는 영역
- JVM 스택(Stack) 영역

### III) 참조 변수의 ==. != 연산

- ==,!= 연산은 동일한 객체를 참조하는지, 다른 객체를 참조하는지 알아볼 때 사용된다.

### IV)  null과 NullPointerException

- 참조 타입 변수는 힙 영역의 객체를 참조하지 않는다는 뜻으로 null 값을 가질 수 있다.
- null 값을 초기값으로 사용할 수 있다.
- NullPointerException
  - 참조 타입 변수를 잘못 사용하면 발생

### V) String 타입

- 자바는 문자열을 String 변수에 저자하기 때문에 다음과 같이 String 변수를 우선 선언해야 한다.
  - `String 변수;`
- String 변수에 문자열을 저장하려면 큰 따옴표로 감싼 문자열 리터럴을 대입하면 된다.
  - `변수 = "문자열";`
- 변수 선언과 동시에 문자열을 저장할 수도 있다.
  - `String 변수 = "문자열";`

- new 연산자 : 힙 영역에 새로운 객체를 만들 때 사용
  - `String name1 = new String("김민식");`
  - `String name2 = new String("김민식");`
  - name1과 name2는 서로 다른 String 객체를 참조

*※ 동일한 String 객체이건 상관없이 문자열만을 비교할 때에는 String 객체의 equls() 메소드를 사용해야 한다.*

*`boolean result = str1.equals(str2);`*

- 참조를 잃은 String 객체는 쓰레기 수집기를 구동, 자동 제거

#### VI) 배열 타입

##### 6.1 배열이란

- 같은 타입의 데이터를 연속된 공간에 나열시키고, 각 데이터에 인덱스(index)를 부여해 놓은 자료구조

##### 6.2 배열 선언

- 배열 변수 선언
  - `타입 [] 변수;` or `타입 변수 [];`
  - 대괄호는 배열 변수를 선언하는 기호
  - 타입은 배열에 저장될 데이터의 타입
  - ex.`int[] intArray = new [6] intArray;` or `int intArray[] = new intArray[6];`
  - 참조할 배열 객체가 업삳면 배열 변수는 null 갑승로 초기화 할 수 있다.

##### 6.3 값 목록으로 배열 생성

- 배열 항목에 저장될 값의 목록이 있다면, 간단하게 배열 객체를 만들 수 있다.
- `String[] names = {"김민식","홍길동","감자바"};`
- 이렇게 생긴 배열에서 "김민식"은 데이터타입[1], "홍길동"은 데이터타입[2]~~.. 이 된다.  

##### 6.4 new 연산자로 배열 생성

- 향후 값들을 저장할 배열을 미리 만들고 싶다면 new 연산자 사용
- `타입[] 변수 = new 타입[길이];`
- `int[] intArray = new int[5]; // 길이 5인 int[]배열`

##### 6.5 배열 길이

- 배열에 저장할 수 있는 전체 항목 수
- 배열의 길이를 얻으려면 배열 객체의 length 필드를 읽으면 된다.
- `배열변수.length;`

##### 6.6 다차원 배열

- ```java
  int[][] scores = new int[2][3];
  
  scores.length //2
  socres[0].length //3
  socres[0].length //3
  ```
##### 6.7 객체를 참조하는 배열

- ```java
  String[] strArray = new String[3];
  strArray[0] = "java";
  strArray[1] = "C++";
  strArray[2] = "C#";
  ```

- String[] 배열은 각 항목에 문자열이 아니라 객체의 주소를 가지고 있다.

- 배열 항목간에 문자열을 비교하기 위해서는 == 연산자 대신 equals() 메소드를 사용해야 한다.

##### 6.8 배열 복사

- 배열 간의 항목 값들을 복사하려면 for문을 사용하거나 System.arraycopy () 메소드를 사용

예제

```java
package review;

import java.util.Arrays;//최대값 구하기

public class p181q7 {

	public static void main(String[] args) {
		int max = 0;
		int[] arry = {1,5,3,8,2};
		
		
		for (int i = 0; i < arry.length; i++) {
			if(arry[i]>max) {
				max = arry[i];
			}
		}
		System.out.println(Arrays.toString(arry));
		
		System.out.println("max: "+max);

	}

}

```

```java
```

