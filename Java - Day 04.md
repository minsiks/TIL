# 4/11 Java Day4

### day3 복습

> - n1 값과 n2 값을 표시하려할시 사용
> - ````java
    Scanner sc = new Scanner(System.in);
    System.out.println("Input Number 1..?")
    String n1. = sc.next();
    System.out println("Input Number 2..?")
    String n2. = sc.next();

  

  ### 1) switch 문

- ```java
  int a = 10;
  		switch (a) {
  		case 10:
  			System.out.println("큰수");
  			break;
  		case 5:
  			System.out.println("중간수");
  		case 1:
  			System.out.println("작은수");
  		default:
  			System.out.println("입력오류");
  			break;
  ```

- 특정한 값으로 주어진 특정한 조건을 찾을 때 if문보다 편하게 사용할 수 있다.

- break : 끝남

  - break를 지우면 입력한 값이 충족되면 그냥 다음으로 넘어간다.

- Ctrl + space : 자동완성

###### ※ Random

> java.util의 기능

```java
public static void main(String[] args) {
		Random r = new Random();
		int n = r.nextInt(3) + 1;  // 1 ~ 3
		System.out.println(n);
```

*난수를 발생하는 기능*

## 1. 반복문 (for문, while문, do-while문)

- 중괄호 블록 내용 영역을 반복적으로 실행할 때 사용
- 종류 : for문, while문, do-while문

### i) for 문

- ```java
  	public static void main(String[] args) {
  		// 1~10까지의 합을 구하시오
  		int sum = 0;
  		int count = 0;
  		for (int i = 1; i <= 10; i++) {
  			sum += i;
  			count++;
  		}
  		System.out.println(sum);
  		System.out.println(sum/count);
  		System.out.println(count);
          
          
  55
  5
  10
  ```

- 1부터 4까지 반복한다 
- i가 1일시 반복되고  ++를 통해 증가한다.
- 반복문은 항상 시작과 끝이 있다.
- 무언가 반복적으로 입력하기 위한 증가가 있다. (i++)

### ii) while 문

- ```java
  int s = 1;
  		int sum2 = 0;
  		while(s <= 10) {
  			sum2 += s;
  			s++;
  		}
  		System.out.println(sum2);
  		System.out.println(s);
  		System.out.println(sum2/(s-1));
  	}
  ```

  ※ 만약 `System.out.println("While!!!"+s);` 을 s++; 구문 이후에 s++;을 처리한 계산이후가 나온다.

  ex. While!!!2
  While!!!3
  While!!!4
  While!!!5
  While!!!6
  While!!!7
  While!!!8
  While!!!9
  While!!!10
  While!!!11

  * 대입연산자 

    : 특정한 상수 값이나 변수 값 또는 객체를 변수에 전달하여 기억시킬 때 사용하는 연산자이다.

    * +=  a+=b : a= a+ b;와 같은 의미, 왼쪽 변수에 더하면서 대입한다.

#### 응용문제

- ```java
  public static void main(String[] args) {
  		// Scanner 를 이용하여 1 ~ 99까지의 정수를 입력 받는다.
  				// 범위를 벗어나면 Bye 프로그램 종료
  				
  				// 1부터 입력 받은 수까지의 합과 평균을 출력 하시오 while
  				Scanner sc = new Scanner(System.in);
  				System.out.println("Input Number(2~99)?");
  				String snum = sc.next();
  				System.out.println(snum);
  				int num = 0;
  				
  				try {		
  				    num = Integer.parseInt(snum);
  				    // 2 ~ 99
  				    if (num <2 || num > 99) {
  				    	System.out.println("Bye 1");
  						sc.close();
  						return;
  				    }
  				}catch(Exception e) {
  					System.out.println("Bye 2");
  					sc.close();
  					return;
  				}
  
  				double sum = 0.0;
  				int s = 1;
  				
  				while(s <= num) {
  					sum += s;
  					s++;
  				}
  				
  				System.out.println(sum);
  				System.out.println(sum/(s-1));
  				sc.close();
  				
  
  	}
  ```

### iii) 반복문에 대한 제어

- break문 : 반복문을 실행 중지할 때 사용된다.

  - ```java
    	public static void main(String[] args) {
    		// i 값이 7일때 까지만 실행 해라
            for (int i = 1; i <= 10; i++) {
    			System.out.println("For Loop:"+i);
    			if (i == 7) {
    				break;
    			}
    		} // end for
            
            System.out.println("-----------------------------------");
            
            int s = 1;   
            while (s <= 10) {
            	System.out.println("For while:"+s);
            	if (s == 7) {
    				break;
    			}
    			s++;
    		}
            
            
    	}
    ```

- continue문 : 특정 조건을 만족하는 경우에 continue문을 실행해서 그 이후 문장을 실행하지 않고 다음 반복으로 넘어간다.

  - ```java
    	public static void main(String[] args) {
    		// i가 짝수 일때만 출력 하시오
            for (int i = 1; i <= 10; i++) {
    			if (i%2 == 1) {
    				continue;
    			}
    			System.out.println("For Loop:"+i);
            } //end for
            
            System.out.println("-----------------------------------");
            
            int s = 1;   
            while (s <= 10) {
            	if (s%2 == 1) {
    				continue;
    			}
            	System.out.println("For while:"+s);
    			s++;
    		}
            
    	}
    ```

#### while(true) 루프

- P.127

- ```java
  public static void main(String[] args) {
  		System.out.println("Start....");
  		Scanner sc = new Scanner(System.in);
  		while(true) {
  			System.out.println("Input cmd(i,s,q) ...");
  			String cmd = sc.next();
  			if(cmd.equals("q")) {
  				System.out.println("Bye");
  				break;
  			}else if(cmd.equals("i")) {
  				System.out.println("Inserted");
  			}else if(cmd.equals("s")) {
  				System.out.println("Selected");
  			}
  		}
  		
  		sc.close();
  		System.out.println("End....");
  
  	}
  ```

- 시스템이 계속 돌아가고 설정한 경로로 움직이며, 설정한 경로를 통하여 종료되거나 터미네이터로 종료된다.