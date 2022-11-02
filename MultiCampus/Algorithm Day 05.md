# 8/13 Algorithm Day 05

## 개인 공부 (백준 온라인 단계별)

### 3-6 A+B -7

```java
import java.util.Scanner;

public class Main {

	public static void main(String[] args) {
		Scanner in = new Scanner(System.in);
		int T = in.nextInt();
		
		for (int i = 1; i <= T; i++) {
			int A = in.nextInt();
			int B = in.nextInt();
			
			System.out.println("Case #"+i+":"+(A+B));
		}
		
		in.close();
		
	
	}

}
```

### 1-10 킹, 퀸, 룩, 비숏, 나이트, 폰

```java
import java.util.Scanner;

public class Main {

	public static void main(String[] args) {
		Scanner in = new Scanner(System.in);
		
		int a = in.nextInt();
		int b = in.nextInt();
		int c = in.nextInt();
		int d = in.nextInt();
		int e = in.nextInt();
		int f = in.nextInt();
		
		int A = 1;
		int B = 1;
		int C = 2;
		int D = 2;
		int E = 2;
		int F = 8;
		
		System.out.println(A-a);
		System.out.println(B-b);
		System.out.println(C-c);
		System.out.println(D-d);
		System.out.println(E-e);
		System.out.println(F-f);
	
	}

}
```

### 3-7 A+B * 8

```java
import java.util.Scanner;

public class Main {

	public static void main(String[] args) {
		Scanner in = new Scanner(System.in);
		
		int a = in.nextInt();
		for (int i = 1; i <= a; i++) {
			int b = in.nextInt();
			int c = in.nextInt();
			
			System.out.println("Case #"+i+": "+b+" + "+c+" = "+(b+c));
		}
		
		in.close();
		
	
	}

}
```

### 3-8 별 찍기 -1

```java
import java.util.Scanner;

public class Main {

	public static void main(String[] args) {
		Scanner in = new Scanner(System.in);
		
		int N = in.nextInt();
		
		for (int i = 1; i <= N; i++) {
			for (int j = 1; j <= i; j++) {
				System.out.print("*");
			}
			System.out.println();
		}
	
	}

}
```

