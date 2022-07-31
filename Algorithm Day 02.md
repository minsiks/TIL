# 7/24 Algorithm Day 01

## 개인 공부 (백준 온라인 단계별)

### 1. 입출력과 사칙연산

#### 1-10 ??!

```java
package backjoon1;

import java.util.Scanner;

public class Main {

	public static void main(String[] args) {
		Scanner in = new Scanner(System.in);
		String b = in.nextLine();
		System.out.println(b+"??!");

	}

}
```

#### 1-11 1998년생인 내가 태국에서는 2541년생?!

```java
import java.util.Scanner;

public class Main1 {

	public static void main(String[] args) {
		Scanner in = new Scanner(System.in);
		Integer y = in.nextInt();
		Integer x = y-(2541-1998);
		System.out.println(x);

	}

}
```



#### 1-12 나머지

```java
import java.util.Scanner;

public class Main2 {

	public static void main(String[] args) {
		Scanner in = new Scanner(System.in);
		Integer A = in.nextInt();
		Integer B = in.nextInt();
		Integer C = in.nextInt();
		System.out.println((A+B)%C);
		System.out.println(((A%C)+(B%C))%C);
		System.out.println((A*B)%C);
		System.out.println(((A%C)*(B%C))%C);
	}
```

#### 1-13 곱셈

```java
package backjoon1;

import java.util.Scanner;

public class Main3 {

	public static void main(String[] args) {
		Scanner in = new Scanner(System.in);
		Integer A = in.nextInt();
		Integer B = in.nextInt();
		System.out.println((B%10)*A);
		System.out.println(((B%100)/10)*A);
		System.out.println((B/100)*A);
		System.out.println(A*B);
	}

}
```

#### 1-14 새싹

```java
public class Main {

	public static void main(String[] args) {
		System.out.println("         ,r'\"7");
		System.out.println("r`-_   ,'  ,/");
		System.out.println(" \\. \". L_r'");
		System.out.println("   `~\\/");
		System.out.println("      |");
		System.out.println("      |");

	}

}
```

