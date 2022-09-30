# 9/27 Algorithm Day 11

## 개인 공부 (백준 온라인 단계별)

### 5-1 정수 N개의 합

```java
class Test {

	long sum(int[] a) {
		long sum = 0;
		
		for (int i = 0; i < a.length; i++) {
			sum += a[i];
		}
		return sum;
	}
}
```

