# 7/24 Algorithm Day 01

## 개인 공부 (백준 온라인 단계별)

### 1. 입출력과 사칙연산

#### 1-Hello World

```java
public class Main {
	public static void main(String[] args) {
		System.out.println("Hello World!");
	}
```

> args **문자열을 배열로 사용하겠다 라는 의미**입니다. args 는 변수명이기 때문에 꼭 args 가 아니어도 상관은 없으나, String[] args 구문 자체를 뺄 수는 없습니다.

#### 1- 고양이

```java
public class Main {
	
	public static void main(String[] args) {
		
		System.out.println("\\    /\\");
		System.out.println(" )  ( ')");
		System.out.println("(  /  )");
		System.out.println(" \\(__)|");
		
	}
}
```

> ```
> 자바를 비롯한 C계열의 언어에서, 백슬래쉬(\) 문자는 이스케이프 문자이므로, 그 자체를 표현할 수 없다.
> 이때는 백슬래쉬(Backslash)를 두번 써서 해결할 수 있습니다. 
> 즉 "\" 를 표현하고 싶으면  "\\"로 백슬래쉬를 두번 쓰면 됩니다.
> ```

#### 1- 개

```java
public class Main {

	public static void main(String[] args) {

		System.out.println("|\\_/|");
		System.out.println("|q p|   /}");
		System.out.println("( 0 )\"\"\"\\");
		System.out.println("|\"^\"`    |");
		System.out.println("||_/=\\\\__|");

	}
}
```

> ```
> "를 표현하고 싶으면 앞에 \(역슬래쉬)를 붙여야 한다.
> ```

#### 1- A+B

```JAVA
import java.util.*;
public class Main{
	public static void main(String args[]){
		Scanner sc = new Scanner(System.in);
		int a, b;
		a = sc.nextInt();
		b = sc.nextInt();
		System.out.println(a + b);
	}
}
```

> Scanner는 입력받는 값
> java.util에서 임포트 해온다.

#### 1-  사칙연산

```java
import java.util.Scanner;
 
public class Main {
	public static void main(String[] args) {
		Scanner in = new Scanner(System.in);
		int A = in.nextInt();
		int B = in.nextInt();
 
        in.close();
 
		System.out.println(A+B);
		System.out.println(A-B);
		System.out.println(A*B);
		System.out.println(A/B);
		System.out.println(A%B);
	}
}
```

