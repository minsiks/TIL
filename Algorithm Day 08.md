# 8/31 Algorithm Day 07

## 개인 공부 (백준 온라인 단계별)

### 4-3 나머지

- Hashset은 Set의 파생클래스
- HashSet은 중복되는 원소를 넣을 경우 하나만 저장
- 순서 개념이 없다

```java
import java.util.HashSet;
import java.util.Scanner;

public class Main {
	public static void main(String args[]){
		
		Scanner in=new Scanner(System.in);
		HashSet<Integer> h = new HashSet<Integer>();
		for (int i = 0; i < 10; i++) {
			h.add(in.nextInt()%42);
			
		}
		in.close();
		
		System.out.println(h.size());
	}
}
```

### 4-4 평균

```java
import java.util.Arrays;
import java.util.Scanner;

public class Main {
	public static void main(String[] args) {
 
		Scanner in = new Scanner(System.in);
 
		double arr[] = new double[in.nextInt()];
		
		for(int i = 0; i < arr.length; i++) {
			arr[i] = in.nextDouble();
		}
		in.close();
		
		double sum = 0;
		Arrays.sort(arr);
		
		for(int i = 0; i < arr.length; i++) {
			sum += ((arr[i] / arr[arr.length-1]) * 100);
		}
		System.out.print(sum / arr.length);
	}
}
```

### 4-5 OX퀴즈

```java
import java.util.Scanner;
 
public class Main {
	public static void main(String[] args) {
		Scanner in = new Scanner(System.in);
 
		String arr[] = new String[in.nextInt()];
 
		for (int i = 0; i < arr.length; i++) {
			arr[i] = in.next();
		}
		
		in.close();
		
		for (int i = 0; i < arr.length; i++) {
			
			int cnt = 0;	// 연속횟수
			int sum = 0;	// 누적 합산 
			
			for (int j = 0; j < arr[i].length(); j++) {
				
				if (arr[i].charAt(j) == 'O') {
					cnt++;
				} 
				else {
					cnt = 0;
				}
				sum += cnt;
			}
			
			System.out.println(sum);
		}
	}
}
```

