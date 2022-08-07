# 8/07 Algorithm Day 04

## 개인 공부 (백준 온라인 단계별)

### 3. 반복문

#### 3-1 구구단

```java
package Backjoon0807;

import java.util.Scanner;

public class Main {
    public static void main(String[] args) {
        
        Scanner in = new Scanner(System.in);
        int a = in.nextInt();
        
        for (int i = 1; i < 10; i++) {
			System.out.println(a+" * "+i+" = "+(a*i));
		}
		in.close();
    }
}



```

#### 3-2 A+B-3

```java
import java.util.Scanner;

public class Main {
    public static void main(String[] args) {
        
    	Scanner in = new Scanner(System.in);
    	
    	int T = in.nextInt();
    	for (int i = 0; i < T; i++) {
    		int A = in.nextInt();
        	int B = in.nextInt();
        	System.out.println(A+B);
		}
    	
        
    }
}
```

#### 3-3 합

```java
import java.util.Scanner;

public class Main {
    public static void main(String[] args) {
        
    	Scanner in = new Scanner(System.in);
    	
    	int n = in.nextInt();
    	int r = 0;
    	for (int i = 0; i <= n; i++) {
			r += i;
		}
    	System.out.println(r);
    	in.close();
    }
}
```

#### 3-4 영수증

```java
package Backjoon0807;

import java.util.Scanner;

public class Main {
    public static void main(String[] args) {
		
    	Scanner in = new Scanner(System.in);
    	
    	int X = in.nextInt();
    	int N = in.nextInt();
    	int r = 0;
    	in.nextLine();
    	int a = 0;
    	int b = 0;
    	
    	for (int i = 0; i < N; i++) {
    		String ab = in.nextLine();
        	String[] splited = ab.split("\\s");
        	a = Integer.parseInt(splited[0]);
        	b = Integer.parseInt(splited[1]);
        	r += a*b;
		}
    	if (r==X) {
			System.out.println("YES");
		}else if (r!=X) {
			System.out.println("NO");
		}
    	in.close();
    }
}
```

#### 3-5 빠른 A+B

```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.StringTokenizer;
 
public class Main {
 
	public static void main(String[] args) throws IOException {
 
		BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
 
		int N = Integer.parseInt(br.readLine());
        
		StringTokenizer st;
		StringBuilder sb = new StringBuilder();
        
		for (int i = 0; i < N; i++) {
			st = new StringTokenizer(br.readLine()," ");
			sb.append(Integer.parseInt(st.nextToken()) + Integer.parseInt(st.nextToken())).append('\n');
		}
		br.close();
 
		System.out.println(sb);
 
	}
}
```

