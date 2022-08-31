# 8/31 Algorithm Day 07

## 개인 공부 (백준 온라인 단계별)

### 3-13 더하기 사이클

```java
import java.util.Scanner;

public class Main {
	public static void main(String args[]){
		
		Scanner in=new Scanner(System.in);
			
		int N = in.nextInt();
		int b = 0;
		int T = N;
		while (true) {
			N = ((N%10)*10)+(((N/10)+(N%10))%10);
			b++;
			if (T==N) {
				break;
			}
		}
		System.out.println(b);
	}
}
```

### 4. 1차원 배열

#### 4-1 최소, 최대

```java
import java.util.Arrays;
import java.util.Scanner;

public class Main {
	public static void main(String args[]){
		
		Scanner in=new Scanner(System.in);
			
		int N = in.nextInt();
		int[] array = new int[N];
		
		for (int i = 0; i < N; i++) {
			array[i] = in.nextInt();
		}
		in.close();
		Arrays.sort(array);
		System.out.print(array[0]+" "+array[N-1]);
	}
}
```

#### 4-2 최댓값

```java
import java.util.Scanner;

public class Main {
	public static void main(String args[]){
		
		Scanner in=new Scanner(System.in);
		int[] arr = new int[9];	
		
		for (int i = 0; i < 9; i++) {
			arr[i] = in.nextInt();
		}
		
		in.close();
		
		int max = arr[0];
		int cnt = 0;
		for (int i = 0; i < arr.length; i++) {
			if (arr[i]> max) {
				max = arr[i];
				cnt = i;
			}
		}
		
		System.out.println(max);
		System.out.println(cnt+1);
	}
}
```

