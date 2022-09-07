# 9/7 Algorithm Day 09

## 개인 공부 (백준 온라인 단계별)

### 4-6 평균은 넘겠지

```java
import java.util.Scanner;

public class Main {
	public static void main(String[] args) {
		Scanner in = new Scanner(System.in);
 
		int C = in.nextInt();
		for (int i = 0; i < C; i++) {
			int N = in.nextInt();
			int[] arr = new int[N];
			int sum = 0;
			for (int j = 0; j < N; j++) {
				arr[j] = in.nextInt();
				sum += arr[j];
			}
			double avr = sum/N;
			double cnt = 0;
			for (int j = 0; j < N; j++) {
				if (arr[j]> avr) {
					cnt++;
				}
			}
			double per = cnt/N*100;
			System.out.printf("%.3f%%\n",per);
		}
	in.close();
	}
}
```

