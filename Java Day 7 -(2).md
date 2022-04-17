# 4/17 Java Day7 - (2)



## 알고리즘 테스트 연습

### Team1 - 1. 장바구니 시스템

1. 예산을 입력한다.

2. 과일의 목록을 출력시킨다.

3. 과일을 선택하면  해당과일의 정보와 입력한 예산으로 몇개를 살 수 있는지와 잔돈을 출력시킨다.

```java
package review;

import java.util.Arrays;
import java.util.Scanner;

import javax.naming.directory.SchemaViolationException;

public class team12 {

	public static void main(String[] args) {
		String arr[][] = {{"사과","1000"},
				{"바나나","2000"},
				{"배","3000"},
				{"포도","3000"},
				{"딸기","2000"}};

		Scanner sc = new Scanner(System.in);
		System.out.println("예산을 입력하세요");
		String budgit = sc.next();
		double b1 = Double.valueOf(budgit);
		for (int i = 0; i < arr.length; i++) {
			for (int j = 0; j < arr[i].length; j++) {
				System.out.printf("%s \t",arr[i][j]);
				
			}
			System.out.println("");
			
		}
		System.out.println("과일은 선택하세요 사과:a,바나나:b,배:p,포도:g,딸기:s");
		String fruit = sc.next();
		if (fruit.equals("a")) {
//			String apple = arr[0][0];
			double many = b1/(Double.valueOf(arr[0][1]));
			double namuge = b1%(Double.valueOf(arr[0][1]));
			System.out.printf("과일 : %s 개수:%.0f 개  잔돈 : %.0f원",arr[0][0],many,namuge);
//			System.out.printf("과일:%s 개수:%f개 잔돈:%f원 \n",arr[0][0],arr[0][1],arr[0][1]);
			
		}else if (fruit.equals("b")) {
			double many = b1/(Double.valueOf(arr[1][1]));
			double namuge = b1%(Double.valueOf(arr[1][1]));
			System.out.printf("과일 : %s 개수:%.0f 개  잔돈 : %.0f원",arr[1][0],many,namuge);
			
		}else if (fruit.equals("p")) {
			double many = b1/(Double.valueOf(arr[2][1]));
			double namuge = b1%(Double.valueOf(arr[2][1]));
			System.out.printf("과일 : %s 개수:%.0f 개  잔돈 : %.0f원",arr[2][0],many,namuge);
		}else if (fruit.equals("g")) {
			double many = b1/(Double.valueOf(arr[3][1]));
			double namuge = b1%(Double.valueOf(arr[3][1]));
			System.out.printf("과일 : %s 개수:%.0f 개  잔돈 : %.0f원",arr[3][0],many,namuge);
		}else if (fruit.equals("s")) {
			double many = b1/(Double.valueOf(arr[4][1]));
			double namuge = b1%(Double.valueOf(arr[4][1]));
			System.out.printf("과일 : %s 개수:%.0f 개  잔돈 : %.0f원",arr[4][0],many,namuge);
		}
		
		sc.close();
	}

}
```

