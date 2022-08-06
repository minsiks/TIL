# 8/06 Algorithm Day 03

## 개인 공부 (백준 온라인 단계별)

### 2. 조건문

#### 2-1 두 수 비교하기

```java
package Backjoon2;

import java.util.Scanner;

public class Main1 {

	public static void main(String[] args) {
		Scanner in = new Scanner(System.in);
		int a = in.nextInt();
		int b = in.nextInt();
		if (a>b) {
			System.out.println(">");
		}
		if (a<b) {
			System.out.println("<");
		}
		if (a==b) {
			System.out.println("==");
		}
	}

}
```

#### 2-2 시험성적

```java
package Backjoon2;

import java.util.Scanner;

public class Main {

	public static void main(String[] args) {
		
		Scanner in = new Scanner(System.in);
		int a = in.nextInt();
		if (a>=90&&a<=100) {
			System.out.println("A");
		}else if (a>=80&&a<=89) {
			System.out.println("B");
		}else if (a>=70&&a<=79) {
			System.out.println("C");
		}else if (a>=60&&a<=69) {
			System.out.println("D");
		}else {
			System.out.println("F");
		}
	}

}

```

#### 2-3 윤년

```java
import java.util.Scanner;

public class Main {

	public static void main(String[] args) {
		
		Scanner in = new Scanner(System.in);
		int a = in.nextInt();
		if (a%4==0&&a%100!=0||a%400==0) {
			System.out.println(1);
		}else {
			System.out.println(0);
		}
	}

}
```

#### 2-4 사분면 구하기

```java
import java.util.Scanner;

public class Main {

	public static void main(String[] args) {
		
		Scanner in = new Scanner(System.in);
		int x = in.nextInt();
		int y = in.nextInt();
		
		if (x<0&&y<0) {
			System.out.println("3");
		}else if (x<0&&y>0) {
			System.out.println("2");
		}else if (x>0&&y>0) {
			System.out.println("1");
		}else if (x>0&&y<0) {
			System.out.println("4");
		}in.close();
		
	}

}

```

#### 2-5 알람 시계

```java
import java.util.Scanner;

public class Main {

	public static void main(String[] args) {
		
		Scanner in = new Scanner(System.in);
		int H = in.nextInt();
		int M = in.nextInt()-45;
		
		int h = 0;
		int m = 0;
		String a = null;
		if (M<0&&H-1>=0) {
			h = H-1;
			m = M+60;
			a = "1";
		}else if (M>=0&&H>=0) {
			m = M;
			h = H;
			a = "2";
		}else if (M<0&&H-1<0) {
			h = H-1+24;
			m = M+60;
			a = "3";
		}else {
			h = H-1;
			m = M+60;
			a = "4";
		}
		
		System.out.println(h+" "+m);
		
	}

}

```

#### 2-5 오븐 시계

```java
import java.util.Scanner;

public class Main {
    public static void main(String[] args) {
        
        Scanner in = new Scanner(System.in);
 
        int A = in.nextInt();
        int B = in.nextInt();
 
        int C = in.nextInt();
 
        int min = 60 * A + B;   // 시 -> 분
        min += C;
 
        int hour = (min / 60) % 24;
        int minute = min % 60;
 
        System.out.println(hour + " " + minute);
 
    }
}
```

#### 2-6 주사위 세개

```java
import java.util.Scanner;

public class Main {
    public static void main(String[] args) {
        
        Scanner in = new Scanner(System.in);
 
        int a= in.nextInt();
        int b= in.nextInt();
        int c= in.nextInt();
        if (a==b&&b==c) {
			System.out.println(10000+a*1000);
		}else if (a==b||b==c||a==c) {
			if (a==b) {
				System.out.println(1000+a*100);
			}else if (b==c) {
				System.out.println(1000+b*100);
			}else if (a==c) {
				System.out.println(1000+a*100);
			}	
		}else {
			if (a>=b&&a>=c) {
				System.out.println(a*100);
			}else if (b>=a&&b>=c) {
				System.out.println(b*100);
			}else if (c>=a&&c>=b) {
				System.out.println(c*100);
			}
		}
    }
}

```

