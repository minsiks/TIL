# 8/30 Algorithm Day 06

## 개인 공부 (백준 온라인 단계별)

### 3-9 별 찍기 -2

``` java
import java.util.Scanner;
 
public class Main {
 
	public static void main(String[] args) {
		Scanner in = new Scanner(System.in);
 
		int N = in.nextInt();
		in.close();
 
		for (int i = 1; i <= N; i++) {
			for (int j = 1; j <= N - i; j++) {
				System.out.print(" ");
			}
			for (int k = 0; k < i; k++) {
				System.out.print("*");
			}
			System.out.println();
		}
	}
}
```

### 3-10 X보다 작은 수

``` java
import java.util.Scanner;

public class Main {

	public static void main(String[] args) {
		Scanner in = new Scanner(System.in);
		
		int N = in.nextInt();
		int X = in.nextInt();
		int arr[] = new int[N];
		
		for (int i = 0; i < N; i++) {
			arr[i] = in.nextInt();
		}
		in.close();

		for (int i = 0; i < N; i++) {
			if(arr[i] < X){
				System.out.print(arr[i]+" ");
			}
		}
	}

}
```

### 3-11 A+B -5

```java
import java.util.Scanner;

public class Main {

	public static void main(String[] args) {
		Scanner in = new Scanner(System.in);
		
		
		for (int i = 0; i < 500; i++) {
			int A = in.nextInt();
			int B = in.nextInt();
			
			if (A==0) {
				break;
			}
			System.out.println(A+B);
		}
		in.close();
	}

}
```

### 3-12 A+B - 4

```java
import java.util.Scanner;

public class Main {
	public static void main(String args[]){
		
		Scanner in=new Scanner(System.in);
			
		while(in.hasNextInt()){
		
			int a=in.nextInt();
			int b=in.nextInt();
			System.out.println(a+b);
		
		}	
		in.close();
	}
}
```

